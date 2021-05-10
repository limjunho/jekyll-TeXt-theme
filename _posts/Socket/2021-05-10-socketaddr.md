---
title: Socket sockaddr, sockaddr_in, sockaddr_un
tags: Socket
---

## Summry  

리눅스/유닉스 시스템에서는 소켓(socket)의 통신 대상을 지정하기 위해 주소(address)를 사용한다. 이 주소라는 것을 저장하거나 표현하는데 사용하는 구조체가 struct sockaddr 이다.  
**bind( ), connect( ) 와 같은 함수들이 2번째 파라미터로 struct sockaddr 을 받는다.**  

본 문서에서는 socketaddr 구조체의 종류와 역할을 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## 관련 헤더파일

```c
#include <sys/socket.h>
#include <netinet/in.h>
#include <netinet/ip.h> /* superset of previous */
```

## sockaddr

```c
struct sockaddr { 
    u_short sa_family; // address family, 2 bytes 
    char sa_data[14];  // IP address + Port number, 14 bytes 
};
```

**socket 의 주소를 담는 기본 구조체 역할을 한다.**  

* sa_family : 주소체계를 구분하기 위한 변수이며, 2 bytes 이다. 참고로, u_short는 unsigned short를 말한다.  
* sa_data : 실제 주소를 저장하기 위한 변수다. 14 bytes 이다.  

## sockaddr_in : AF_INET

```c
struct sockaddr_in { 
    short sin_family; // 주소 체계: AF_INET 
    u_short sin_port; // 16 비트 포트 번호, network byte order 
    struct in_addr sin_addr; // 32 비트 IP 주소 
    char sin_zero[8]; // 전체 크기를 16 비트로 맞추기 위한 dummy 
}; 
    
struct in_addr {
    u_long s_addr; // 32비트 IP 주소를 저장 할 구조체, network byte order 
};
```

**sockaddr 구조체에서 sa_family가 AF_INET인 경우에 사용하는 구조체이다.**  

sockaddr 를 그대로 사용하는 경우 sa_data 에 IP Address 와 Portnumber 가 조합되어 있어 읽거나 쓰기 불편하기 때문에 sockaddr_in 을 사용한다. 이 구조체에서 사용하는 IP Address 는 IPv4 체계를 사용한다.  

* sin_family : 항상 AF_INET을 설정한다. 이것은 필수다.
* sin_port : 포트번호를 가진다. 2bytes 이다. 즉, 포트번호는 0~65535 의 범위를 갖는 숫자 값이다. 이 변수에 저장되는 값은 network byte order이어야 한다. 
* sin_addr : 호스트 IP주소이다.
* sin_zero : 8 bytes dummy data이다. 반드시 모두 0으로 채워져 있어야 한다.
    * sockaddr_in가 sin_zero를 제외한 크기(sin_family + sin_port + sin_addr)가 8 bytes이므로, 총합이 16 bytes이다. struct sockaddr구조체와 크기를 일치시키기 위해 존재하는 필드이다.

### sockaddr_in example

```c
#include <sys/types.h>
#include <sys/socket.h>
#include <stdio.h>
#include <netinet/in.h>
#include <inttypes.h>
#include <strings.h>

int main(void)
{
	int sock;
	struct sockaddr_in in;

	if ((sock = socket(AF_INET, SOCK_STREAM, 0)) < 0) {
		perror("socket");
		return (1);
	}

	bzero(&in, sizeof (struct sockaddr_in));
	in.sin_family = AF_INET;
	in.sin_port = htons(80);
	if (inet_pton(AF_INET, "127.0.0.1", &in.sin_addr) != 1) {
		perror("inet_pton");
		return (1);
	}

	if (connect(sock, (struct sockaddr *)&in,
				sizeof (struct sockaddr_in)) != 0) {
		perror("connect");
		return (1);
	}

	/* use socket */

	return (0);
}
```

## sockaddr_in6 : AF_INET6

```c
#include <netinet/in.h>

struct sockaddr_in6 {
	sa_family_t     sin6_family;   /* Address family, AF_INET6 */
	in_port_t       sin6_port;     /* port number */
	uint32_t        sin6_flowinfo; /* IPv6 flow information and traffic class */
	struct in6_addr sin6_addr;     /* IPv6 address */
	uint32_t        sin6_scope_id; /* Interface Scope ID (new in 2.4) */
};

struct in6_addr {
	unsigned char   s6_addr[16];   /* IPv6 address */
}; 
```

**IPv6 주소체계의 소켓주소를 사용하는 구조체이다.**  
IPv4에 사용되는 struct sockaddr_in과는 달리 struct sockaddr_in6는 bzero( ) 또는 memset( ) 과 같은 함수를 통해 0으로 초기화 해주어야 하는 몇가지 멤버변수들이 추가되어 있다. 만약 strcut sockaddr_in6을 사용하기 전에 모두 0으로 초기화 하지 않으면, 프로그램은 정의되지 않은 동작을 야기할 수 있으므로 주의가 필요하다.  

* sin6_family : 항상 AF_INET6 이어야만 한다.
* sin6_port : IPv6 포트를 저장하는 변수이다.
    * 이 값은 직접 조작하기보다는, ntohs( )와 htons( ) 로 조작하는게 좋다.
* sin6_flowinfo : IPv6 헤더와 연관된 트래픽 클래스와 플로우 레이블을 포함한다. 
* sin6_addr : 16bytes (128bits)의 IPv6주소를 저장하는 변수이다.
* sin6_scope_id : sin6_addr의 주소범위에 따라 달라지는 식별자를 포함할 수 있다. 프로그램은 sin6_scope_id를 초기화 할 필요는 없다. 다양한 라이브러리 함수 호출의 결과로, 운영체제에 의해 채워지는 값이다.

### sockaddr_in6 example

```c
#include <sys/types.h>
#include <sys/socket.h>
#include <stdio.h>
#include <netinet/in.h>
#include <inttypes.h>
#include <strings.h>

int main(void)
{
	int sock6;
	struct sockaddr_in6 in6;

	if ((sock6 = socket(AF_INET6, SOCK_STREAM, 0)) < 0) {
		perror("socket");
		return (1);
	}

	bzero(&in6, sizeof (struct sockaddr_in6));
	in6.sin6_family = AF_INET6;
	in6.sin6_port = htons(12345);
	if (inet_pton(AF_INET6, "::1", &in6.sin6_addr) != 1) {
		perror("inet_pton");
		return (1);
	}

	if (bind(sock6, (struct sockaddr *)&in6,
				sizeof (struct sockaddr_in6)) != 0) {
		perror("bind");
		return (1);
	}

	/* use server socket */

	return (0);
}
```

## sockaddr_un : AF_UNIX or AF_LOCAL

```c
#include <sys/un.h>

#define UNIX_PATH_MAX    108

struct sockaddr_un {
    sa_family_t sun_family;               /* AF_UNIX */
    char        sun_path[UNIX_PATH_MAX];  /* pathname */
};
```

**struct sockaddr_un은 하나의 시스템에서 서로다른 프로세스 사이의 통신에 사용되는 소켓의 주소를 지정하는데 사용하는 구조체이다.**  
'Unix Domain Socket' 이라고도 한다. 이 Unix domain 소켓은 파일 시스템의 경로에 의해 식별된다.  

* sun_family : 항상 AF_UNIX 값을 가져야 한다.
* sun_path : 파일 시스템 경로를 지정한다. NULL 로 끝나는 문자열(C string)이어야 한다. 경로의 최대 길이는 NULL terminator를 포함해서 108 bytes이다.



## Reference

> [illumos.org/man/3socket/sockaddr_in](https://illumos.org/man/3socket/sockaddr_in)  
> [[네트워크/C] sockaddr, sockaddr_in, sockaddr_un 구조체 - 소켓 주소 정보를 나타낸다 - 하얀쿠아](https://techlog.gurucat.net/292)
