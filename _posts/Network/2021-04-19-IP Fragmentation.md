---
title: IP Fragmentation(단편화)
tags: Network
---

## 개요  

본 문서에서는 IP Fragmentation(단편화)에 대해 알아본다.

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## IP Fragmentation  

* 패킷을 전송하고자 하는데 패킷의 크기가 MTU(Maximum Transmission Unit)을 초과하면 한번에 전송할 수 없다.  
* 패킷을 MTU 이하의 조각으로 분할하는 것을 단편화(Fragmentation), 분할된 조각을 단편(Fragment)이라고 한다.
* 즉, 거치는 라우터 마다 전송에 적합한 데이터링크계층 프레임으로 변환이 필요하다.  

* **IPv4 에서의 단편화**
    * IPv4에서는 발신지 뿐만 아니라 중간 라우터에서도 IP 단편화가 가능하다.
* **IPv6 에서의 단편화**
    * IPv6에서는 IP 단편화가 발신지에서만 가능하다.
    * IPv6에서는 라우팅 처리 효율을 높이기 위해, 가급적 IP 단편화를 필요없도록 한다.
    * Ipv4와 달리, 기본 헤더 상에 단편화 제어 관련 필드를 두지 않고, 단편화 확장 헤더를 통해 단편화한다.
* 재조립(Reassembly)은 항상 최종 수신지에서만 가능하다.  

**MTU(Maximum Transmission Unit)**  
MTU는 최대 전송 단위로서 TCP/IP Network 등과 같이 패킷 또는 프레임 기반의 네트워크에서 전송될 수 있는 최대 크기의 패킷 또는 프레임을 가리키며 대개 옥텟을 단위로 사용한다.  
* MTU가 너무 크면
    * 큰 크기의 패킷을 처리할 수 없는 라우터를 만나면 재전송을 해야하는 경우가 생길 수 있다.  
* MTU가 너무 작으면
    * 상대적으로 헤더 및 송수신 확인에 따르는 오버헤드가 커진다.

### 단편화 예시  

* 4000바이트의 패킷을 전송하려고 하는데 MTU가 1500이다.
* 패킷은 아래와 같이 분할된다.  

|순서|페이로드|헤더|More Flag|offset|
|---|--------|---|---------|------|
|1|1480|20|1|0|
|2|1480|20|1|185|
|3|1020|20|0|370|  

* 각 단편 모두 헤더를 가져야 하기 때문에 최대 1480 Bytes로 분할 될 수 있다.
* 1번 2번 단편은 뒤에 이어질 단편이 있으므로 More Flag가 1이다.  
* offset은 8 Bytes 단위로 표시된다.

## 참고

> [IP 단편화 - IT위키](https://itwiki.kr/w/IP_%EB%8B%A8%ED%8E%B8%ED%99%94)  
> [IP Fragmentation, IP Segmentation, IP 단편화, IP 조각화, IPv4 단편화, IPv6 단편화 - 정보통신기술용어해설](http://www.ktword.co.kr/abbr_view.php?nav=&m_temp1=5236&id=1003)  
>[MTU란 - 고민하자](https://gominhaja.tistory.com/9)