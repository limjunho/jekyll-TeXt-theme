---
title: AF_INET vs PF_INET
tags: Linux Socket
---

## Summry  

본 문서에서는 socket 생성 시 사용되는 파라미터인 AF_INET 과 PF_INET 의 차이를 설명한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## Socket create example

192.168.0.1같은 IP 주소 체계를 인터넷 프로토콜뿐만 아니라 다른 프로토콜에서도 사용할 수 있도록 IP 주소 체계를 지칭할 때는 AF_INET, IP 자체를 가리킬 때는 PF_INET을 사용한다.

**socket create example**  
```c
#include <sys/types.h>
#include <sys/socket.h>

struct sockaddr_in addr;
int sockfd;

// AF_INET
addr.sin_family = AF_INET;
addr.sin_port = htons(9999);
addr.sin_addr.s_addr = htonl(INADDR_ANY);

// PF_INET
sockfd = socket(PF_INET, SOCK_STREAM, 0);
```

### AF_INET, PF_INET 구분 의도

아주 오래 전 소켓 프로그래밍을 설계한 사람들은 확장성을 위해 한 종류의 주소 체계가 여러 프로토콜을 지원할 수 있도록 했다. 인터넷 프로토콜을 예로 들자면, 192.168.0.1, 8.8.4.4같은 주소 체계가 인터넷 프로토콜뿐만 아니라 다른 프로토콜도 지원하는 식.

* sockaddr_in 구조체처럼 소켓의 주소와 함께 **주소 체계**를 지정하기 위해서는 AF로 시작하는 이름(AF_INET, AF_IPX, …)를 사용한다. 
   * AF는 Address Family의 줄임말
* socket() 함수처럼 실제 연결을 하기 위한 **프로토콜**을 지정하기 위해서는 PF로 시작하는 이름(PF_INET, PF_IPX, …)를 사용한다. 
   * PF는 Protocol Family의 줄임말

**하지만 하나의 주소 체계가 여러 프로토콜을 지원하는 일은 일어나지 않았다.**  
따라서 리눅스 커널에서도 AF 로 시작하는 상수와 PF 로 시작하는 상수가 서로 같은 값을 가지도록 값을 정의한다.  

**즉, 리눅스에서는 PF를 써야 할 자리에 AF를 써도 되고, AF를 써야 할 자리에 PF를 써도 무방하다.**  

### 권장 방법

권장 방식이 어느 하나로 통일되어 있지는 않다.  

* 유명한 소켓 프로그래밍 입문서인 <Beej’s Guide to Network Programming>에서는 AF_INET과 PF_INET을 설계 당시의 의도대로 구별하여 사용한다.
* linux manual page, BSD manual page 에서는 모든 곳에 AF를 사용하기를 권장한다.

## Reference

> [AF_INET vs PF_INET - 방성범(Bang Seongbeom)](https://www.bangseongbeom.com/af-inet-vs-pf-inet.html#fn:bgnet-2)  
> [소켓 - 언제나 휴일](https://blog.daum.net/ehclub/606)
