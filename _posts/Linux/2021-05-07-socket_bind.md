---
title: Socket bind
tags: C Linux Socket
---

## Summry  

본 문서에서는 socket 에 IP 주소와 포트번호를 지정해주는 bind()함수를 설명한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## bind()

**bind 헤더 및 형태**  
```c
#include <sys/types.h>
#include <sys/socket.h>

int bind(int sockfd, struct sockaddr *myaddr, socklen_t addrlen);
```

### int sockfd

fd는 file discriptor 의 약자이다. socket 함수의 리턴으로 받은 socket discriptor 를 인자로 사용한다.  
[Linux discriptor](https://limjunho.github.io/2021/04/26/discriptor.html)

### struct sockaddr *myaddr

주소 정보로 인터넷을 이용하는 AF_INET인지 시스템 내에서 통신하는 AF_UNIX에 따라서 달라지는데 인터넷을 통해 통신하는 AF_INET인 경우에는 struct sockaddr_in 을 사용하고 시스템 내부 통신인 AF_UNIX인 경우에는 struct sockaddr 를 사용한다.  

**struct sockaddr_in**
```c
struct sockaddr_in {
   sa_family_t sin_family;       /* Address family	*/
   unsigned short int sin_port        /* Port number */
   struct in_addr sin_addr;         /* Internet address */
   
   /* Pad to size of ´struct sockaddr'. */
   unsigned char  __pad[__SOCK_SIZE__ - sizeof(short int) -
      sizeof(unsigned short int) - sizeof(struct in_addr)];
};
```

**struct sockaddr**
```c
struct sockaddr {
   sa_family_t sa_family;   /* address family, AF_xxx */
   char sa_data[14];  /* 14 bytes of protocol address */
};
```

쉽게말해 server 의 ip 주소이다.  

### socklen_t addrlen

myaddr 구조체의 크기  

### return

* 0 : 성공
* -1 : 실패

## Reference

> [Unix Socket Userful Resources - tutorialspoint](https://www.tutorialspoint.com/unix_sockets/socket_core_functions.htm)  
> [bind() 소켓에 IP주소와 포트번호 지정 - FALiNUX Forum](http://forum.falinux.com/zbxe/index.php?document_srl=430926&mid=C_LIB)
