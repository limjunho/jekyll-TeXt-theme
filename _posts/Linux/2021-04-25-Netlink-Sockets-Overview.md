---
title: Netlink Sockets - Overview
tags: Linux
---

## Summry  

[Netlink Sockets - Overview](http://www.worldcolleges.info/sites/default/files/netlink.pdf)  
본 문서에서는 위의 자료를 정리한다.

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## 1. Introduction 

리눅스는 네트워킹 부분에 있어 firewalls, queue, class, filter 등의 Qos, traffic conditioning, netlink sockets 등을 포함한 많은 장점을 가지고 있다. 이 문서는 리눅스 시스템에서 제공하는 netlink sockets 에 대한 사용법 및 실행 등에 관하여 이야기 하고 있다.  
Section 2에서는 생성 및 사용, 커널에서 일반적인 프로토콜의 등록에 대한 이야기를 하고 있으며, Section 3에서는 커널에서 어떻게 packet handling 을 하고 있는지와 user land에서 netlink 의 packaging 에 관한 설명을 한다. 또한 완벽한 예제를 부록에서 제공하고 있다.

## 2. Netlink sockets

<br>

### 2.1 An Overview of netlink sockets

**netlink 는 흔히 kernel module과 user space processes 사이에 정보전송에 사용된다.**  
이것은 kernel/user space간 양방향통신(bi-directional communication)을 제공하며 standard sockets 을 기본으로 구성된 user space 와 kernel module 을 위한 내부의 standard API 를 사용하여 interface 를 제공하게 된다.  
Netlink sockets 은 user space 에서 다음과 같은 형태로 생성된다.  

```c
sock_fd = socket(AF_NETLINK, SOCK_RAW, NETLINK_ROUTE);
```

도메인은 AF_NETLINK를, 소켓타입은 SOCK_RAW 를 사용한다. Netlink가 datagram을 기본으로 동작하지만, SOCK RAW와 SOCK DGRAM은 netlink 에서 둘 다 가능한 값들이다. 그러나 netlink protocol 은 netlink group과 kernel module간의 통신을 위해 Netlink family를 선택한다.  
아래는 규정된 netlink families 이다.  

* NETLINK ROUTE : routing 업데이트 및 ipv4 routing table, network routes, ip addresses, link parameters, neighbour setups, queuing disciplines, traffic class와 packet classifier 에 대하여 NETLINK ROUTE 소켓을 통한 조정.
* NETLINK FIREWALL : ipv4 firewall code를 통한 packet 전송의 입력
* NETLINK ARPD : user space 에서 arp table 관리.
* NETLINK ROUTE6 : ipv6 라우팅 테이블 업데이트에 대한 입출력
* NETLINK IP6 FW : 전송 실패된 packet 을 받기 위한 ipv6 firewall 체크.(현재 제공하지 않음)
* NETLINK TAPBASE...NETLINK TAPBASE+15 : ethertap device 의 보기. Ethertap 은 userspace에서 ethernet device 를 시뮬레이션 가능하게 하는 pseudo network tunnel device 이다.
* NETLINK SKIP : Reserved for ENskip

<br>

### 2.2 Protocol Registration
실제로 netlink socket 이 생성 및 사용되기 이전에, kernel 에서 제공가능한 소켓타입이 어떤 것이 있는지 알아보자.  
리눅스 시스템이 시작되고 메모리와 프로세스과 관리되며 모듈이 시작되었을 때, 어떤 코드들은 이미 끝난다. 이런 것들은 init/main.c 에 존재하는 setup() 함수에서 기본적으로 실행되는 것들이며 특별히 sock_init() 함수도 여기에 포함된다. 이 함수는 net/socket.c 에 존재하며 firewalls, protocols 등과 같은 것들을 초기화한다. 우리는 protocol의 초기화에 관심을 갖도록 하며 proto_init() 이 모든 protocols 에 대하여 그 역할을 한다.  
Protocols에 관한 list의 유지관리는 net/protocols.c 에 있으며 아래에 그 보기가 있다.  

```c
struct net_proto protocols[] = {
#ifdef CONFIG_NETLINK
    { "NETLINK", netlink_proto_init },
#endif
#ifdef CONFIG_INET
    { "INET", inet_proto_init }, /* TCP/IP */
#endif

struct net_proto
{
    const char *name; /* Protocol name */
    void (*init_func)(struct net_proto *); /* Bootstrap */
};
```

table에는 net proto structures 가 포함된다. 각 테이블에 있는 entry 마다 protocol 이름과 초기화 함수가 있다. 이것을 이용하여 proto_init() 함수는 각 protocols 의 초기화 함수를 갖게 되면 각 protocol 의 초기화 함수를 entry 에서 불러들인다.  

```c
void __init proto_init(void)
{
    extern struct net_proto protocols[]; /* Network protocols */
    struct net_proto *pro;
    pro = protocols;
    while (pro->name != NULL)
    {
        (*pro->init_func)(pro);
        pro++;
    }
}
```

이제 netlink proto init 함수에서 어떤 일이 일어나는지 보자.  
아래에는 우리의 관심을 끄는 또 다른 structure 가 있다.  

```c
struct net_proto_family
{
    int family;
    int (*create)(struct socket *sock, int protocol);
    /* These are counters for the number of different methods of
    each we support */
    short authentication;
    short encryption;
    short encrypt_net;
};
```

각 netlink protocol 에 대하여 해당 family 와 생성함수가 주어진 socket 에 대해 binding 을 하게 된다.  

```c
struct net_proto_family netlink_family_ops = {
    PF_NETLINK,
    netlink_create
};
```

netlink proto init() 안에 아래의 코드를 보자.  

```c
void netlink_proto_init(struct net_proto *pro)
{
    .
    .
    sock_register(&netlink_family_ops);
    .
}
```

sock_register()함수는 해당 address family 에 알려지기 원하는 시점에서 protocol handler 에 의해 불려진다. 그리고 socket module 과 link 를 걸게 된다. 이것은 net family table 안에 protocol entry 를 생성하게 된다. Net family 는 protocol list 와 모든 protocols 등록을 포함한다.  

```c
int sock_register(struct net_proto_family *ops)
{
    .
    .
    net_families[ops->family]=ops;
    .
}
```

이제 우리는 기본적인 structure 를 알았다. 이제 user space 에서 netlink socket 을 만들었을 때 어떤 일이 일어나는지 보자. Socket() 함수는 kernel 에서 야기된 system call 이다. 이것은 system call 이 어떻게 작동하는지 이해할 수 있는 좋은 기회가 된다. 부록을 참고하라.  

<br>

### 2.3 Netlink Socket creation

모든 소켓관련된 call 은 sys_socketcall() 함수에 의해 handling 된다. 이것은 SYS SOCKET, SYS BIND, SYS CONNECT 등의 타입에 의해서 동작이 결정되며 적당한 함수가 불려진다.  
Socket 생성에 있어서 sys_socketcall 의 코드는 아래와 같다.  

```c
asmlinkage int sys_socketcall(int call, unsigned long *args)
{
    .
    case SYS_SOCKET:
        err = sys_socket(a0,a1,a[2]);
        break;
    .
}
```

net/socket.c 에 있는 sys_socket 에서 socket 은 생성되고 socket discriptor 를 할당받게 된다.  
아래의 코드를 참조.  

```c
asmlinkage int sys_socket(int family, int type, int protocol)
{
    .
    retval = sock_create(family, type, protocol, &sock);
    .
    retval = get_fd(sock->inode);
    .
}
```

socket 생성 관련 코드이다.  

```c
int sock_create(int family, int type, int protocol, struct socket **res)
{
    .
    .
    sock = sock_alloc();
    i = net_families[family]->create(sock, protocol);
    .
    .
}
```

위에서 설명하였듯이 netlink socket 은 netlink_create 에서 호출된다. 이 함수는 socket 과 protocol 의 동작에 관련이 되어있다.  

```c
static int netlink_create(struct socket *sock, int protocol)
{
    .
    .
    sock->ops = &netlink_ops;
    .
    .
}
```

netlink_ops 는 netlink sockets 에 관련된 동작들의 function 포인터 리스트를 준다.  

```c
struct proto_ops netlink_ops = {
    PF_NETLINK,

    sock_no_dup,
    netlink_release,
    netlink_bind,
    netlink_connect,
    sock_no_socketpair,
    sock_no_accept,
    netlink_getname,
    datagram_poll,
    sock_no_ioctl,
    sock_no_listen,
    sock_no_shutdown,
    sock_no_setsockopt,
    sock_no_getsockopt,
    sock_no_fcntl,
    netlink_sendmsg,
    netlink_recvmsg
};
```

netlink socket 이 생성된 후, 다음 과정은 socket 을 bind 하는 것이다. Bind 가 user level 의 관심사일 때 sys_bind 함수는 kernel 에서 불려진다. 이것을 순서대로 부름으로서 socket 생성에 bind 하게 된다.  

netlink_bind 안에서 netlink_insert() 함수는 nl_table 로 불리우는 sock structure list 를 netlink socket 의 entry 로 생성한다.  

```c
static void netlink_insert(struct sock *sk)
{
    sk->next = nl_table[sk->protocol];
    nl_table[sk->protocol] = sk;
}
```

netlink socket 의 creation 과 binding 사용에 대한 요약은 아래와 같다.  

```c
struct sockaddr_nl address;
sock_fd = socket(AF_NETLINK, SOCK_RAW, NETLINK_ROUTE);
bind(sock_fd, (struct sockaddr*)&address, sizeof(address));

// where sockaddr nl is defined in include/linux/netlink.h

struct sockaddr_nl
{
    sa_family_t nl_family; /* AF_NETLINK */
    unsigned short nl_pad; /* zero */
    __u32 nl_pid; /* process pid */
    __u32 nl_groups; /* multicast groups mask */
};

```

AF_NETLINK family 를 사용하고 nl_groups 은 multicast option 이며 nl_pid 는 process id 를 나타낸다. 만약 주어진 값이 0일 때 kernel 은 current->pid 를 task structure 에서 가지고 와서 채워넣는다.  
Kernel 에서 pid의 check 를 하며 만약 0일 경우 netlink_autobind() 를 아래와 같이 사용한다.  

```c
static int netlink_autobind(struct socket *sock)
{
    struct sock *sk = sock->sk;
    struct sock *osk;
    sk->protinfo.af_netlink.groups = 0;
    sk->protinfo.af_netlink.pid = current->pid;
    .
    .
    netlink_insert(sk);
}
```

<br>

### 2.4 Sending and Receiving messages

read, write 는 socket 을 creation 하고 bind 한 후에 recvmsg() 와 sendmsg() 를 사용하여 할 수 있다.  

```c
sendmsg(sock_fd, &msg, 0);

// where msg is of struct msghdr defined in /include/linux/socket.h

struct msghdr {
    void * msg_name; /* Socket name */
    int msg_namelen; /* Length of name */
    struct iovec * msg_iov; /* Data blocks */
    __kernel_size_t msg_iovlen; /* Number of blocks */
    void * msg_control; /* Per protocol magic (eg BSD file descriptor
    passing) */
    __kernel_size_t msg_controllen; /* Length of cmsg list */
    unsigned msg_flags;
};

// This msghdr is flled as follows,

struct msghdr msg = {
    (void*)&nladdr, sizeof(nladdr),
    &iov, 1,
    NULL, 0,
    0
};

memset(&nladdr, 0, sizeof(nladdr));
nladdr.nl_family = AF_NETLINK;
nladdr.nl_pid = 0;
nladdr.nl_groups 
```

iov 는 struct iovec로 선언되었다.  

```c
struct iovec
{
    void *iov_base; /* BSD uses caddr_t (1003.1g requires void *) */
    __kernel_size_t iov_len; /* Must be size_t (1003.1g) */
};
```

iovec structure 는 kernel 로 pass 되는 data 의 pointer 와 length 정보를 가지고 있다.  
Netlink_sendmsg 에서 iovec 를 통해서 data 를 send 하며 kernel space 에 copy 한다.  

```c
static int netlink_sendmsg(struct socket *sock, struct msghdr *msg, int len,
struct scm_cookie *scm)
{
    .
    memcpy_fromiovec(skb_put(skb,len), msg->msg_iov, len);
    .
}
```

그래서, kernel 로 보내지는 data 를 채우며, iovec structure 는 초기화되고 packet 은 kernel 로 전송된다. iovec 의 data 는 복제되고 kernel 에서 처리된다. Data 는 우리가 원하는 수행 동작의 종류에 따라 채워진다. Nladdr 의 pid 는 0으로 설정해야 하며, 이것에 대한 중요함은 이후에 다루도록 한다.  

### 2.5 NETLINK ROUTE Family

지금까지 우리는 어떻게 kernel netlink socket 을 생성하는 지 알아보았다. 이제 우리는 NETLINK ROUTE family socket 의 생성에 초점을 맞추도록 해보자.  
net/core/rtnetlink.c 에서는 rtnetlink_init 에 대한 부분이 있다.  

```c
__initfunc(void rtnetlink_init(void))
{
    #ifdef RTNL_DEBUG
    printk("Initializing RT netlink socket\n");
    #endif
    rtnl = netlink_kernel_create(NETLINK_ROUTE, rtnetlink_rcv);

    if (rtnl == NULL)
        panic("rtnetlink_init: cannot initialize rtnetlink\n");
    
    register_netdevice_notifier(&rtnetlink_dev_notifier);
    rtnetlink_links[PF_UNSPEC] = link_rtnetlink_table;
    rtnetlink_links[PF_PACKET] = link_rtnetlink_table;
}
```

이 함수는 net/socket.c 에서 socket init 함수의 한 부분으로 불린다. 이 함수는 user 의 요청에 의한 kernel 내의 handle 함수 내에서 netlink socket 을 생성한다. netlink_kernel_create 함수는 다음과 같다.  

```c
struct sock *
netlink_kernel_create(int unit, void (*input)(struct sock *sk, int len))
{
    .
    .
    if (netlink_create(sock, unit) < 0) {
        sock_release(sock);
        return NULL;
    }
    sk = sock->sk;
    if (input)
        sk->data_ready = input;

    netlink_insert(sk);
    .
    .
}
```

시스템이 시작되었을 때 netlink socket 의 생성이 이루어지고 nl_table 의 entry 가 만들어진다. 이 entry 가 table 의 가장 처음에 위치 한다. 이 Netlink socket 은 pid 0 으로 생성이 된다. 그 이유는 NETLINK_ROUTE 와 관계된 함수들을 사용하고자 하는 netlink socket 사용자들이 socket 에 접근할 때 pid 를 0 으로 설정하기 때문이다. 또한, 그 함수들이 사용하는 function pointer 와 rtnetlink_rcv, data_ready pointer 가 이 값을 사용하는 것도 주의하여 볼 사항이다.  
이 함수는 kernel 로 진입하는 entry point 로서 중요하다.  
link_rtnetlink_table 은 다음과 같다.  

```c
struct rtnetlink_link
{
    int (*doit)(struct sk_buff *, struct nlmsghdr*, void *attr);
    int (*dumpit)(struct sk_buff *, struct netlink_callback *cb);
};
```

위 structure 를 보면 doit 과 dumpip 포인터로 구성되어있는 것을 볼 수 있다. 이 테이블은 RTM_NEWQDISC, RTM_DELQDISC etc 로 불리우는 동작들에 관련된 function call 의 index 가 될 수 있다.  

이 테이블은 sched/sch_api.c 에서 채워진다.  

```
link_p[RTM_NEWQDISC-RTM_BASE].doit = tc_modify_qdisc;
link_p[RTM_DELQDISC-RTM_BASE].doit = tc_get_qdisc;
link_p[RTM_GETQDISC-RTM_BASE].doit = tc_get_qdisc;
link_p[RTM_GETQDISC-RTM_BASE].dumpit = tc_dump_qdisc;
link_p[RTM_NEWTCLASS-RTM_BASE].doit = tc_ctl_tclass;
link_p[RTM_DELTCLASS-RTM_BASE].doit = tc_ctl_tclass;
link_p[RTM_GETTCLASS-RTM_BASE].doit = tc_ctl_tclass;
link_p[RTM_GETTCLASS-RTM_BASE].dumpit = tc_dump_tclass;
```

그리고, route 와 관련된 function pointer 는 /net/ipv4/devinet.c 에서 저장된다.  

```c
static struct rtnetlink_link inet_rtnetlink_table[RTM_MAX-RTM_BASE+1] =
{
    .
    .
    { inet_rtm_newroute, NULL, },
    { inet_rtm_delroute, NULL, },
    { inet_rtm_getroute, inet_dump_fib, },
    .
    .
}
rtnetlink_links[PF_INET] = inet_rtnetlink_table;
```

이제 netlink packet 이 어떻게 user space 로부터 kernel 로 가는지에 대해 알아보자.  
send_msg 는 sendmsg 로 연결되며 이것은 netlink_sendmsg() 로 연결된다. 그리고 이 함수는 netlink_unicast() 또는 netlink_broadcast()를 부르게 된다. 이 함수는 netlink socket 에 있는 모든 nl_table 의 pid 값을 비교하여 netlink socket 의 이 message 를 전송할지를 결정하며 NETLINK_ROUTE 에 해당하는 rtnetlink_rcv 함수에 해당하는 소켓의 data_ready 함수를 부른다.  
이와 관련된 코드는 아래와 같다.  

```c
int netlink_unicast(struct sock *ssk, struct sk_buff *skb, u32 pid, int
nonblock)
{
    .
    .
    for (sk = nl_table[protocol]; sk; sk = sk->next) {
        if (sk->protinfo.af_netlink.pid != pid)
            continue;
    .
    sk->data_ready(sk, len);
}
```

rtnetlink_rcv() 로 부터의 코드 흐름을 보면 skb 가 dequeue 되어 rtnetlink_rcv_skb 로 넘겨지고 순서대로 rtnetlink_rcv_msg()가 불려진다. 이 함수가 실제로 netlink packet 에 대한 동작을 수행하며 family 에 관계된 rtnetlink_links 배열의 index 를 이용하여 doit 함수를 부르게 된다. Queue 와 class 에 해당하는 family 는 AF_UNSPEC 이며 indexing 은 link_rtnetlink_table 에서 처리된다. 그러나 route 처리(route modification)에 대한 indexing 은 해당 family 가 AF_INET 이므로 inet_rtnetlink_table 에서 처리 된다.  
따라서 적절한 함수가 불려지고 필요한 동작을 한 후 그 결과를(success/failure) 유저에게 알리게 된다.  

## 3. Packaging a netlink packet

netlink packet을 packaging 하는 것은 커널과의 긴밀한 연관을 포함한다. kernel data structure 와 그에 상응하는 코드를 살펴보고 그들이 어떻게 해석되는지를 이해하라. 다시 말해서, netlink packet 에 채워줘야하는 parameter 에는 format 이 있고, 원하는 parameter 를 채워줌으로서 kernel 모듈에서 원하는 동작을 얻어낼 수 있다.  
이번 단원에서는 어떻게 netlink packet 이 user space 에서 kernel space 의 routing table 에 entry 를 추가할 수 있는지에 대한 간략한 sample code 를 보도록 한다.  

```c
struct nlmsghdr
{
    __u32 nlmsg_len; /* Length of message including header */
    __u16 nlmsg_type; /* Message content */
    __u16 nlmsg_flags; /* Additional flags */
    __u32 nlmsg_seq; /* Sequence number */
    __u32 nlmsg_pid; /* Sending process PID */
};
struct rtmsg
{
    unsigned char rtm_family;
    unsigned char rtm_dst_len;
    unsigned char rtm_src_len;
    unsigned char rtm_tos;

    unsigned char rtm_table; /* Routing table id */
    unsigned char rtm_protocol; /* Routing protocol; see below */
    unsigned char rtm_scope; /* See below */
    unsigned char rtm_type; /* See below */

    unsigned rtm_flags;
};
```

하나의 netlink packet 이 routing table 에서 동작을 하기 위해서는 특별히 아래의 data structure 를 구성해야 한다. Structure 의 멤버는 netlink message header, rtmsg, buffer 를 포함해야 한다. queue 를 setting 하기 위해서는 class 와 filter rtmsg structure 는 traffic control structure 인 tcmsg 로 대치되어야 한다.  
자세한 내용은 subsection 에서 다루도록 한다.  

```c
struct
{
    struct nlmsghdr netlink_header;
    struct rtmsg rt_message;
    char buffer[1024];
} request;
```

이것은 일반적으로 route 정보를 encapsulation 하기 위하여 사용하는 structure 이다. 위에 있는 buffer 는 rtattribute 로 채워진다. 각 type/len 의 구성은 아래의 실제 동작값이다. 따라서 rtattribute 는 특별히 <length, type, value> 단위로 움직인다. Length 값은 structure rta 의 size 와 data 값의 size 를 포함하고 있다.  

```c
struct rtattr
{
    unsigned short rta_len;
    unsigned short rta_type;
};
```

### 3.1 Illustration of the usage of netlink sockets with an example

zebra 로부터 가져온 예가 아래에 있다. GNU licensed package 는 router 처럼 linux box 의 환경을 꾸미는데 사용된다. Zebra 와 iproute2 package 는 netlink socket 의 동작과 kernel 에서의 상호작용에 대하여 이해 할 수 있는 최상의 소스이다. 이 단원에서는 zebra 와 iproute2 에서 코드 walkthough 와 기본적인 data structure 의 이해를 돕도록 starting potin 를 제공하기로 한다.  

### 3.1.1 Macros for handling rtattributes in the kernel

실제 동작하는 parameter 들을 보기전에 rtattrubute 에서 사용하는 몇가지 macros 를 이해하도록 한다. 이 코드는 include/linux/rtnetlink.h 에 있다.  
RTA ALGIN macro 는 nearest nibble boundary 의 length 를 가져온다.  

```c
#define RTA_ALIGNTO 4
#define RTA_ALIGN(len) ( ((len)+RTA_ALIGNTO-1) & ~(RTA_ALIGNTO-1) )

RTA_ALIGN(3) translates to (7 & ~3) returns 4
RTA_ALIGN(9) translates to (12 & ~3) returns 12

#define RTA_OK(rta,len) ((len)>0 && (rta)->rta_len>=sizeof(struct rtattr)&& \
                        (rta)->rta_len <= (len))
#define RTA_LENGTH(len) (RTA_ALIGN(sizeof(struct rtattr)) + (len))
```

RTA_OK macro 는 다음과 같은 값들을 체크한다.  

* 주어진 len 이 0보다 큰가?
* Rtattribute 의 length 가 적어도 rta struct size 보다 큰가?
* Attribute 의 length 가 argument len 보다 작은가?

RTZ_LENGTH macro 는 type/len field 의 length 를 parameter 값의 length 에 더한다. 예를 들어 RTA_LENGTH(4) 의 경우 RTA_ALGN(4+4) 로 변환되어 nearest nibble 은 output 8 로서 동작한다.  
RTA_DATA 는 attribute 의 data pointer 를 돌려준다. 즉, type&length 의 offset pointer 를 계산함으로서 parameter 값의 시작 번지를 돌려준다. RTA_LENGTH(0) 는 structure rta 의 size 를 돌려준다. RTA_PAYLOAD 는 parameter 값의 length 를 돌려준다. 이 값이 payload 이다.  

```c
#define RTA_DATA(rta) ((void*)(((char*)(rta)) + RTA_LENGTH(0)))
#define RTA_PAYLOAD(rta) ((int)((rta)->rta_len) - RTA_LENGTH(0))
```

이 macro 의 정의는 include/linux/netlink.h 에 있다.  

```c
#define NLMSG_ALIGNTO 4
#define NLMSG_LENGTH(len) ((len)+NLMSG_ALIGN(sizeof(struct nlmsghdr)))
#define NLMSG_ALIGN(len) ( ((len)+NLMSG_ALIGNTO-1) & ~(NLMSG_ALIGNTO-1) )
```

NLMSG_ALIGN 은 RTA_ALIGN macro 와 유사하다. 단지 nearest nibble boundary 에서 length 를 정렬한다는 것 뿐이다. NLMSG_LENGTH(len) 는 structure nlmsghdr 의 size 에 주어진 len 을 이용하여 length 를 더한다.  

### 3.1.2 Utility function for adding the rtatribute

netlink packet 에서 rtattribute 를 추가 사용하는데 있어서 standard 한 utility 함수이다.  
이 함수는 iproute2 에서 가져온 함수이다.  

```c
int addattr_l(struct nlmsghdr *n, int maxlen, int type, void *data, int alen)
{
    int len = RTA_LENGTH(alen);
    struct rtattr *rta;
    if (NLMSG_ALIGN(n->nlmsg_len) + len > maxlen)
        return -1;
    rta = (struct rtattr*)(((char*)n) + NLMSG_ALIGN(n->nlmsg_len));
    rta->rta_type = type;
    rta->rta_len = len;
    memcpy(RTA_DATA(rta), data, alen);
    n->nlmsg_len = NLMSG_ALIGN(n->nlmsg_len) + len;
    return 0;
}
```

len = RTA_LENGTH(alen) 은 macro 설명에서 했던 것처럼 alen 에 structure rta 의 length 를 더한 값으로 주어진 데이터의 length 를 할당한다. n->mlmsg_len 은 항상 packet 의 length 를 갖는다. 이 함수에서 기본적으로 무엇을 하는가는 지난 packet 의 length 를 가져와서 offset 을 계산함으로서 새로운 parameter 가 그 pointer 를 갖게끔 하는 것이다. 포인터는 typecast 한 structure rta 이다. 그리고, parameter 의 type&length 는 실제 data 를 복사하며 채워진다. 위에서 이야기 되었듯이 RTA_DATA 는 struct rta 와 data 의 pointer 를 같게 되며 그리고 copy 된다. N->nl_msg_len 는 새로운 parameter 의 길이에 의해 증가된다. 그럼으로서 새로운 parameter 가 더해지고 나서 packet 의 length 를 반영하게 된다. 모든 parameter 는 이 함수를 통해서 netlink packet 을 채워 넣게 된다.  

### 3.1.3 Adding a new route

이제 하나의 예를 들어 보자. 우리는 kernel 의 routing table 에 새로운 route 를 추가할 수 있는 parameter 를 볼 수 있다.  
초기화 과정 포함.  

```c
memset(&request, 0, sizeof(request));
request.netlink_header.nlmsg_len = NLMSG_LENGTH(sizeof(struct rtmsg));
request.netlink_header.nlmsg_flags = NLM_F_REQUEST|NLM_F_CREATE;
request.netlink_header.nlmsg_type = RTM_NEWROUTE;
request.rt_message.rtm_family = AF_INET;
request.rt_message.rtm_table = RT_TABLE_MAIN;

if (cmd != RTM_DELROUTE)
{
    request.rt_message.rtm_protocol = RTPROT_BOOT;
    request.rt_message.rtm_scope = RT_SCOPE_UNIVERSE;
    request.rt_message.rtm_type = RTN_UNICAST;
}
```

초기 netlink_header->nlmsg_len 가 parameter 를 추가하기 전에 structure nlmsghdr 와 rtmsgt 의 length 를 포함하고 있다. 그래서 첫 paramneter 에 대해서는 우리가 packet 의 character buffer 의 시작점으로 이동 할 수 있다. AF_INET 로 family 를 설정하여 커널에서 inet_netlink_table 의 index 를 할 수 잇는데 이는 RTM_NEWROUTE command 를 이용하여 inet_rtm_newroute 를 불러오게 함으로서 이루어진다. lnet_rtm_newroute. 커널에 존재하는 이 함수는 커널에서 routing table 에 새로운 route 를 추가할 수 있게 한다. 또한 이후에 다루어지게 될 RT_TABLE_MAIN 에 주의를 할 필요가 있다.  
Ipv4 에서는 destination 과 gateway ipaddress 를 채우기 위해 unsigned integer array 4 byte 가 필요하다. 그리고  addattri_l() 함수를 부름으로서 netlink packet 의 packaging 은 끝난다.  

```c
addattr_l(&request.netlink_header, sizeof(request), RTA_GATEWAY, &gw,4);
addattr_l(&request.netlink_header, sizeof(request), RTA_DST, &dst,4);
addattr32 (&request.netlink_header, sizeof(request), RTA_OIF, index);
```

outgoing interface 는 index 에 의해 나타난다. Parameters 와 RTA_GATEWAY, RTA_DST(include/linux/rtnetlink.h 에 정의 되어있다.) 와 같은 것들로 채워지는 name 을 주의해 볼 필요가 있다. 각 필요한 동작들에 대하여 각기 다른 parameter 들로 채워져야 한다. 이는 Route, gateway, destination address 그리고 interface 의 추가를 위해 만족해야 할 사항이다. 이제, netlink packet 은 kernel 로 내려갈 모든 설정이 되었다.  
Packet 을 kernel 로 보내기 위해서 아래의 코드를 보자.  

```c
    int status;
    struct sockaddr_nl nladdr;
    struct iovec iov = { (void*)netlink_header, netlink_header->nlmsg_len };
    char buf[8192];
    struct msghdr msg = {
        (void*)&nladdr, sizeof(nladdr),
        &iov, 1,
        NULL, 0,
        0
    };

    memset(&nladdr, 0, sizeof(nladdr));
    nladdr.nl_family = AF_NETLINK;
    nladdr.nl_pid = 0;
    nladdr.nl_groups = 0;

    n->nlmsg_seq = ++rtnl->seq;
    if (answer == NULL)
        n->nlmsg_flags |= NLM_F_ACK;

    status = sendmsg(rtnl->fd, &msg, 0);
```

family 는 AF_NETLINK 로 설정한다. iovector 와 message 를 형성하고 sendmsg 를 부른다.  
Success/failure 를 상태에 따라 return 한다.  

### 3.1.4 What happens in the kernel

netlink packet 이 커널로 갔을 때, 이전에 설명한 바대로, inet_rtnetlink_table 의 doit 함수는 RTM_NEWROUTE 에 의해서 indexing 되며 RTM_NEWROUTE 는 inet_rtm_newroute() 함수를 부르고 control 한다. 이 함수는 net/ipv4/fib_frontend.c 에 있다.  

```c
int inet_rtm_newroute(struct sk_buff *skb, struct nlmsghdr* nlh, void *arg)
{
    struct fib_table * tb;
    struct rtattr **rta = arg;
    struct rtmsg *r = NLMSG_DATA(nlh);

    if (inet_check_attr(r, rta))
        return -EINVAL;
    tb = fib_new_table(r->rtm_table);

    if (tb)
        return tb->tb_insert(tb, r,(struct kern_rta*)rta,nlh,&NETLINK_CB(skb));
    return -ENOBUFS;
}
```

NLMSG_DATA macro 는 netlink packet 에 있는 rtmessage 의 시작점을 가져온다.  
inet_check_attr 는 특별히 parameter list 를 통해서 loop 를 돌고 data 로만 구성된 parameter arry 를 생성한다. 이것이 중요한 이유는 이후에 include/net/fib.h 에 정의 되어있는 kern_rta structure 를 typecast 하기 때문이다.  

```c
struct kern_rta
{
    void *rta_dst;
    void *rta_src;
    int *rta_iif;
    int *rta_oif;
    void *rta_gw;
    u32 *rta_priority;
    void *rta_prefsrc;
    struct rtattr *rta_mx;
    struct rtattr *rta_mp;
    unsigned char *rta_protoinfo;
    unsigned char *rta_flow;
    struct rta_cacheinfo *rta_ci;
};
```

이 structure 는 각각의 member 가 routing table 항목에 1대1 대응된다. 따라서 typecast 한다.  

```c
enum rtattr_type_t
{
    RTA_UNSPEC,
    RTA_DST,
    RTA_SRC,
    RTA_IIF,
    RTA_OIF,
    RTA_GATEWAY,
    RTA_PRIORITY,
    RTA_PREFSRC,
    RTA_METRICS,
    RTA_MULTIPATH,
    RTA_PROTOINFO,
    RTA_FLOW,
    RTA_CACHEINFO
};

static int inet_check_attr(struct rtmsg *r, struct rtattr **rta)
{
    int i;
    for (i=1; i<=RTA_MAX; i++) {
        struct rtattr *attr = rta[i-1];
        .
        .
        if (i != RTA_MULTIPATH && i != RTA_METRICS)
        {
            rta[i-1] = (struct rtattr*)RTA_DATA(attr);
        }
    }
    return 0;
}
```

new_b table 을 생성하였다. 이 때 RT_TABLE_MAIN parameter 가 사용되었고, 추가는 net/ipv4/fib_hash.c 에 정의 되어있는 fn_hash_insert() 함수의 포인터가 된다. 여기서 개개의 parameter 를 알아내고, entry 는 커널로 정보를 전송함으로서 추가 한다.  
따라서 이전에 이야기 되었듯이, netlink packet 이 package 되기 위해서 Kernel space 의 해당 코드는 packet 의 정확한 설명을 이해 한다.  
특별한 함수를 수행하기 위해 커널은 netlink packet 이 특정한 형태로 netlink packet 을 package 한다. 그리하여 user space 로부터 parameter 는 커널 내에서 각 사항을 채울 수 있다.  

## A Appendix - System calls

System call 은 user mode 에서 system mode 로 이행되는 과정이며 기본적 정의들로 의해 동작한다.  
linux 에서 system call 은 interrupt 0x80 과 실제 register 값, 그리고 system  call 번호를 사용한다. kernel(system mode) 에서 kernel function 을 부르는 것은 arch/i386/kernel/entry.S 에 정의 되어있는 _sys_call_table 을 실행하는 것이다.  
프로그램에 의해 사용되는 함수로부터 system call 로의 전환은 C library 에 의해 이루어진다.  
실제 system call 의 동작은 interrupt routine 에 의해 동작한다. 이것은 arch/i386/kernel/entry.S 에 정의된 _system_call() 의 entry address 에서 시작한다. Interrupt service routine 이 return 되었을 때 return 값은 적절한 전송 register 로부터 읽어온다. 그리고 library function 은 종료된다.  

예를 들어보면, 아래엔 하나의 system call 인 sys_socket_call() 함수가 있고 이 함수는 전체 프로그램에서 socket 을 사용할 수 있도록 해준다. 해당 코드는 아래와 같다.  
sys_call_table 의 entry 가 아래와 같으며 이는 system call 을 결정하게 된다.  

``` c
ENTRY(sys_call_table)

    .long SYMBOL_NAME(sys_socketcall)

    // The number and name of the system call is dened in include/asmi386/unistd.h as
#define __NR_socketcall 102

```

## B Appendix - Code Listing routeadd.c

아래는 kernel 의 routing table 에 routing entry 를 추가하는 간략한 코드이며 flow of control 과 예상 가능한 not complete 에 대한 처리를 담고 있다.  

******

```c
#include <stdio.h>
#include <asm/types.h>
#include <linux/netlink.h>
#include <linux/rtnetlink.h>

struct rtnl_handle
{
    int         fd;
    struct sockaddr nl local;
    struct sockaddr nl peer; 
    __u32       seq;
    __u32       dump;
};

// This code may not be complete in all respects, it just shows the flow of
// control, for more detailed information refer to the code of iproute2 and
// zebra packages.




// This function is to open the netlink socket as the name suggests. 
int netlink open(struct rtnl handle* rth)
{
    int addr_len;

    memset(rth, 0, sizeof(rth));

    // Creating the netlink socket of family NETLINK ROUTE

    rth->fd = socket(AF_NETLINK, SOCK_RAW, NETLINK_ROUTE);
    if (rth->fd < 0) {
        perror("cannot open netlink socket");
        return -1;
    }

    memset(&rth->local, 0, sizeof(rth->local));
    rth->local.nl_family = AF_NETLINK;
    rth->local.nl_groups = 0;

    // Binding the netlink socket
    if (bind(rth->fd, (struct sockaddr*)&rth->local, sizeof(rth->local)) < 0)
    {
        perror("cannot bind netlink socket");
        return -1;
    }
    addr_len = sizeof(rth->local);
    
    if (getsockname(rth->fd, (struct sockaddr*)&rth->local, &addr_len) < 0)
    {
        perror("cannot getsockname");
        return -1;
    }

    if (addr len != sizeof (rth->local)) {
        fprintf (stderr, "wrong address lenght %d\n", addr_len);
        return -1;
    }

    if (rth>local.nl_family != AF_NETLINK) {
        fprintf(stderr, "wrong address family %d\n", rth->local.nl_family);
        return -1;
    }
    rth->seq = time(NULL);
    return 0;
}

// This function does the actual reading and writing to the netlink socket
int rtnl talk(struct rtnl handle *rtnl, struct nlmsghdr *n, pid_t peer, unsigned groups, struct nlmsghdr *answer)
{
    int status;
    struct nlmsghdr *h;
    struct sockaddr_nl nladdr;

    // Forming the iovector with the netlink packet. 
    struct iovec iov = { (void*)n, n->nlmsg len }; 
    char buf [8192];

    // Forming the message to be sent.
    struct msghdr msg = {
        (void*)&nladdr, sizeof(nladdr)
        &iov, 1,
        NULL, 0,
        0
    };

    // Filling up the details of the netlink socket to be contacted in the
    // kernel. 
    memset(&nladdr, 0, sizeof (nladdr));
    nladdr.nl_family = AF_NETLINK;
    nladdr.nl_pid = peer;
    nladdr.nl_groups = groups;

    n->nlmsg_seq = ++rtnl->seq;
    if (answer == NULL) 
        n->nlmsg_flags |= NLM_F_ACK;

    // Actual sending of the message, status contains success/failure
    status = sendmsg(rtnl->fd, &msg, 0);

    if (status < 0)
        return -1;
}

// This function forms the netlink packet to add a route to the kernel routing
// table
route_add(__u32* destination, __u32* gateway)
{
    struct rtnl_handle rth;

    // structure of the netlink packet. 
        struct {
        struct nlmsghdr n;
        struct rtmsg r; 
        char buf [1024];
    } req;

    char mxbuf [256];
    struct rtattr * mxrta = (void*)mxbuf;
    unsigned mxlock = 0;

    memset(&req, 0, sizeof(req));

    // Initialisation of a few parameters
    req.n.nlmsg_len = NLMSG_LENGTH(sizeof(struct rtmsg));
    req.n.nlmsg_flags = NLM_F_REQUEST | NLM_F_CREATE;
    req.n.nlmsg_type = RTM_NEWROUTE;

    req.r.rtm_family = AF_INET;
    req.r.rtm_table = RT_TABLE_MAIN;

    req.r.rtm_protocol = RTPROT_BOOT;
    req.r.rtm_scope = RT_SCOPE_UNIVERSE;
    req.r.rtm_type = RTN_UNICAST;

    mxrta>rta->type = RTA_METRICS;
    mxrta>rta->len = RTA_LENGTH(0);

    // RTA DST and RTA GW are the two esential parameters for adding a route, 
    // there are other parameters too which are not discussed here. For ipv4,
    // the length of the address is 4 bytes. 
    addattr_l(&req.n, sizeof(req), RTA_DST, destination, 4);
    addattr_l(&req.n, sizeof(req), RTA_GATEWAY, gateway, 4);

    // opening the netlink socket to communicate with the kernel
    if (rtnl_open(&rth) < 0)
    {
        fprintf(stderr, "cannot open rtnetlink\n");
        exit(1);
    }

    // sending the packet to the kernel. 
    if (rtnl_talk(&rth, &req.n, 0, 0, NULL) < 0)
        exit(2);

    return 0;
}

// This is the utility function for adding the parameters to the packet. 
int addattr_l(struct nlmsghdr *n, int maxlen, int type, void *data, int alen)
{
    int len = RTA_LENGTH(alen);
    struct rtattr *rta;

    if (NLMSG_ALIGN(n>nlmsg len) + len > maxlen)
        return -1;
    rta = (struct rtattr*)(((char*)n) + NLMSG_ALIGN(n->nlmsg_len));
    rta->rta_type = type;
    rta->rta_len = len;
    memcpy(RTA_DATA(rta), data, alen);
    n->nlmsg_len = NLMSG_ALIGN(n->nlmsg_len) + len;
    return 0;
}
```