---
title: Socket Parameters
tags: C Linux
---

## Summry  

본 문서에서는 socket 생성 시 대입하는 파라미터를 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## Socket parameters

**socket create**  
```c
#include <sys/types.h>
#include <sys/socket.h>

int socket (int family, int type, int protocol);
```

### Family

|Family|Description|
|------|-----------|
|AF_INET|IPv4 protocols|
|AF_INET6|IPv6 protocols|
|AF_LOCAL|Unix domain protocols|
|AF_ROUTE|Routing Sockets|
|AF_KEY|Ket socket|

### Type

|Type|Description|
|----|-----------|
|SOCK_STREAM|Stream socket (TCP)|
|SOCK_DGRAM|Datagram socket (UDP)|
|SOCK_SEQPACKET|Sequenced packet socket |
|SOCK_RAW|Raw socket (TCP나 UDP를 거치지 않고 IP계층 사용)|

### Protocol

|Protocol|Description|
|--------|-----------|
|IPPROTO_TCP|TCP transport protocol|
|IPPROTO_UDP|UDP transport protocol|
|IPPROTO_SCTP|SCTP transport protocol|
|0|Type 에서 미리 정해진 경우|

## Reference

> [Unix Socket Userful Resources - tutorialspoint](https://www.tutorialspoint.com/unix_sockets/socket_core_functions.htm)  
