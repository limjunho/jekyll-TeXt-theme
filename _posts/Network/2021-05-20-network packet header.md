---
title: network packet header
tags: Network
---

## Summry  

본 문서에서는 packet header의 필드와 역할을 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## TCP header  

![그림1](/assets/Network/packet-header/1.png)  
응용 계층으로부터 데이터를 받은 TCP는 헤더를 추가한 후에 이를 IP로 보낸다.  

|필드|내용|
|---|---|
|Source Port|발신지 포트 주소|
|Destination Port|목적지 포트 주소|
|Sequence Number|송신자가 지정하는 순서 번호, 전송되는 바이트 수를 기준으로 증가. SYN = 1 : 초기 시퀀스 번호가 된다. ACK 번호는 이 값에 1을 더한 값.SYN = 0 : 현재 세션의 이 세그먼트 데이터의 최초 바이트 값의 누적 시퀀스 번호|
|Acknowledgment Number|수신 프로세스가 제대로 수신한 바이트의 수를 응답하기 위해 사용.|
|Header Length|TCP 헤더 길이를 4 바이트(32 비트) 단위로 표시 따라서, TCP 헤더 길이는 4 x (24 - 1) = 60 바이트 이하|
|Reserved|사용을 하지 않지만 나중을 위한 예약 필드이며 0으로 채워져야한다.|
|Flag Bit|SYN, ACK, FIN 등의 제어 번호 -> 아래 추가 설명 참조|
|Window Size|수신 윈도우의 버퍼 크기를 지정할 때 사용. 0이면 송신 프로세스의 전송 중지|
|TCP Checksum|TCP 세그먼트에 포함되는 프로토콜 헤더와 데이터에 대한 오류 검출 용도|
|Urgent Pointer|긴급 데이터를 처리하기 위함, URG 플래그 비트가 지정된 경우에만 유효|

**Flag Bit**  

|종류|내용|
|---|---|
|URG|긴급 위치를 필드가 유효한지 설정|
|ACK|응답 번호 필드가 유효한지 설정. 클라이언트가 보낸 최초의 SYN 패킷 이후에 전송되는 모든 패킷은 이 플래그가 설정되어야 한다. ACK 번호를 사용하여 패킷이 도착했는지 확인하며 송신한 패킷이 제대로 도착하지 않았으면 재송신을 요구한다.|
|PSH|수신 애플리케이션에 버퍼링된 데이터를 상위 계층에 즉시 전달할 때 설정|
|RST|연결의 리셋이나 유효하지 않은 세그먼트에 대한 응답용|
|SYN|연결 설정 요구. 동기화 시퀀스 번호. 양쪽이 보낸 최초의 패킷에만 이 플래그가 설정되어 있어야 한다.|
|FIN|더 이상 전송할 데이터가 없을 때 연결 종료 의사 표시|

## UDP header

![그림2](/assets/Network/packet-header/2.png)  

|필드|내용|크기|
|---|---|---|
|source port|데이터를 보내는 애플리케이션의 포트 번호|16 bits|
|destination port|데이터를 받을 애플리케이션의 포트 번호|16 bits|
|length|UDP 헤더와 데이터의 총 길이|16 bits|
|checksum|데이터 오류 검사에 사용|16 bits|

## IP header

![그림3](/assets/Network/packet-header/3.PNG)  

|필드|내용|크기|
|---|---|---|
|Version|이진수로 표현 되며 0100은 IPv4이고 0110은 IPv6이다.|4 bits|
|Header Length|IPv4의 헤더의 길이를 표현한다. 하지만 옵션이 필드가 가변 필드이므로 길이는 달라질 수 있다.(기본 20바이트)|4 bits|
|Type of Service(ToS) or DiffServ|packet을 특별히 조정 할 수 있다.|8 bits|
|Total Length|packet의 길이 이며 최대 65535 옥텟을 가질 수 있다.|16 bits|
|Identifier|분할한 packet을 알아볼 수 있게 해준다.|16 bits|
|Flags|첫 번째 bit는 사용하지 않으며 2번째 bit는 Don't Fragment(DF)이다. DF값이 1로 설정되어 있으면 router는 packet을 분할할 수 없다.|3 bits|
|Fragment Offset|패킷을 분할하여 보낼 때 fragment는 반드시 연속하여 오지 않는다. identifier로 나눠진 패킷을 구분하여 이 필드가 재 조립할수 있도록 해준다.|13 bits|
|TTL(Time To Live)|패킷이 첫번째 생성될 때 숫자를 부여 받는다. 패킷은 라우터에서 라우터를 통과할 때 각각의 라우터가 이 넘버를 1씩 감소 시킨다.|8 bits|
|Protocol|프로토콜의 종류를 보여준다.|8 bits|
|Header Checksum|IP Header를 위한 error감지 필드 이다. 체크섬은 encapsulate 된 data를 검사하지 않는다.|16 bits|
|Source Address|소스와 주소를 나타낸다.|32 bits|
|Destination Address|목적지 주소를 나타낸다.|32 bits|
|Option|가변 길이 필드이며 패킷 헤더에 각각의 발생지 정보나 다른 router에 들어가야하는 정보가 포함되어 있다.|24 bits|
|Padding|헤더가 항상 32의 배수가 되도록 하기위해 여분의 0이 추가되는 필드||

**well-known protocol numbers**  

|Protocol Number|Host-to-Host Layer Protocol|
|---------------|---------------------------|
|1|Internet Control Message Protocol(ICMP)|
|2|Internet Group Management Protocol(IGMP)|
|4|IP in IP(encapsulation)|
|6|Transmission Control Protocol(TCP)|
|17|User Datagram Protocol(UDP)|
|45|Inter-Domain Routing Protocol(IDRP)|
|46|Resource Reservation Protocol(RSVP)|
|47|Generic Routing Encapsulation(GRE)|
|54|NBMA Next Hop Resolution Protocol(IGRP)|
|88|Cisco Internet Gateway Routing Protocol(IGRP)|
|89|Open Shortest Path First(OSPF)|

## Ethernet header

![그림3](/assets/Network/packet-header/4.PNG)  

|필드|내용|크기|
|---|---|---|
|Destination MAC Address|목적지 MAC Address|6 bytes|
|Source MAC Address|소스 MAC Address|6 bytes|
|EtherType|상위 프로토콜 종류를 표시|2 bytes|
|Data|상위계층으로부터 받은 데이터 또는 전달할 데이터|46 bytes ~ 1500 bytes|
|CRC checksum|오류 검사 필드|4 bytes|

**EtherType 대표적인 값**  

|필드|내용|
|---|----|
|0x0600|Xerox XNS IDP|
|0x0800|IPv4|
|0x0805|X.25|
|0x0806|ARP|
|0x0835|RARP|
|0x6003|DEC DECnet Phase Ⅳ|
|0x8100|VLAN ID|
|0x8137|Novell Netware IPX|
|0x8191|NetBIOS |
|0x86DD|IPv6|
|0x8847|MPLS  |
|0x8863|PPPoE Discovery Stage |
|0x8864|PPPoE PPP Session Stage|
|0x888E|IEEE 802.1X|
|0x88CC|LLDP (Link Layer Discovery Protocol)|

## Reference

> [TCP vs UDP - linjunho](https://limjunho.github.io/2021/04/17/TCP_UDP.html)  
> [[문서] TCP, UDP, IP 헤더구조 및 정리 - 절규](https://m.blog.naver.com/PostView.nhn?blogId=thescream&logNo=168961557&proxyReferer=https:%2F%2Fwww.google.com%2F)  
> [[Ethernet Header] 구조 정리](https://rednooby.tistory.com/13)  
> [Ethernet frame - WIKIPEDIA](https://en.wikipedia.org/wiki/Ethernet_frame)