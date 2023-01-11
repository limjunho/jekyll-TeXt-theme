---
title: module 5(네트워킹 및 콘텐츠 전송)
tags: AWS(Cloud_Foundations)
---

## Summry

본 문서에서는 AWS Academy Cloud Foundations module 5(네트워킹 및 콘텐츠 전송)를 정리한다.  
서비스 별 기능과 개요에 대해 정리된 문서이므로 해당 서비스를 사용하기 위해선 AWS 가이드를 참조할 것

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## Amazon Virtual Private Cloud(VPC)

- **AWS 클라우드의 논리적으로 격리된 영역을 프로비저닝할 수 있는 서비스**
- **다음과 같은 기능을 설정할 수 있음**
  - IP 주소 범위 선택
  - 서브넷(VPC를 분할하는 IP 주소의 범위) 생성
  - 라우팅 테이블 및 네트워크 게이트웨이 구성
- **퍼블릭 인터넷 액세스가 전혀 없는 프라이빗 서브넷에 DB나 back-end system 배치 가능**
- **보안 그룹 및 네트워크 액세스 제어 목록을 비롯한 다중 보안 계층을 사용하여 EC2 Instance에 대한 액세스를 제어할 수 있음**
- **단일 AWS region에 속하며 여러 가용 영역에 걸쳐 구현될 수 있음**

### 퍼블릭 IP 주소 유형

![그림1](/assets/AWS/Cloud_Foundations5/1.png)

- **탄력적 IP는 고정 IP로 탄력적 IP를 네트워크 인터페이스와 연결하면 인스턴스에 직접 연결하는 것 보다 이점이 있음**
  - 장애 발생 시 네트워킹 인터페이스의 모든 속성을 한 인스턴스에서 다른 인스턴스로 한 번에 옮길 수 있음
  - 탄력적 IP는 유료 서비스

---

## VPC Networking

### Internet Gateway

![그림2](/assets/AWS/Cloud_Foundations5/2.png)

- **Internet Gateway는 확장 가능하며 이중화를 지원하는 고가용성 VPC 구성 요소**
  - Internet Gateway가 있어 인스턴스와 퍼블릭 인터넷 간의 통신이 가능
- **사용 목적**
  1. 인터넷 트래픽에 대해 VPC 라우팅 테이블의 대상을 제공
  2. 퍼블릭 IPv4 주소가 할당된 인스턴스의 네트워크 주소 변환을 수행

### NAT(Network Address Translation) Gateway

![그림3](/assets/AWS/Cloud_Foundations5/3.png)

- **사용 목적**
  - NAT Gateway를 사용하면 프라이빗 서브넷의 인스턴스가 인터넷 또는 다른 AWS 서비스에 연결할 수 있다.
- **NAT Gateway 생성**
  - NAT Gateway가 상주할 퍼블릭 서브넷을 지정
  - NAT Gateway와 연결할 탄력적 IP 주소 지정
  - 생성 후에는 인터넷 바운드 트래픽이 NAT Gateway를 가리키도록 하나 이상의 프라이빗 서브넷과 연결된 라우팅 테이블을 업데이트해야함
- **NAT Gateway 대신 VPC의 퍼블릭 서브넷에 NAT Instance를 사용할 수도 있다.**
  - NAT Gateway가 더 향상된 가용성, 높은 대역폭, 관리 부담이 더 적은 환경을 제공하므로 NAT Gateway를 권장함.

### VPC Sharing

![그림4](/assets/AWS/Cloud_Foundations5/4.png)

- 동일한 조직(organization)내의 다른 AWS 계정과 서브넷을 공유할 수 있다.
- VPC Sharing을 사용하면 여러 AWS 계정에서 Amazon EC2 Instance, RDB, Amazon Redshift 클러스터 및 AWS Lambda 함수와 같은 application resource를 중앙 관리형 공유 VPC로 생성할 수 있음

### **VPC Peering**

- **두 VPC 간의 트래픽을 프라이빗으로 라우팅 할 수 있음**
- **양쪽 VPC의 Instance가 동일한 네트워크에 있는 것처럼 서로 통신할 수 있음**
- **제한 사항**
  - IP 주소 범위는 중복 불가
  - 전이적 피어링은 지원하지 않음
    - 즉, VPC의 트래픽이 두 번째 VPC로 전송되고 거기서 멈춤
  - 동일한 두 VPC 간에는 하나의 피어링 리소스만 설정할 수 있음

### AWS Transit Gateway

![그림5](/assets/AWS/Cloud_Foundations5/5.png)

- 가상 사설 클라우드를 상호 연결하는 데 사용하는 네트워크 전송 허브. 많은 수의 VPC를 연결하기 위해 사용하며 온프레미스 네트워크를 연결할 수도 있음
- 각 VPC pair에는 전용 VPC peering 연결이 필요함
  - 연결의 복잡성이 큰 부담이 될 수 있으므로 Transit Gateway사용

### AWS Direct Connect

- 데이터 센터가 AWS region에서 멀리 있는 등의 이유로 성능에 부정적인 영향을 끼치는 경우 AWS Direct Connect를 사용하여 Direct Connect 로케이션 중 하나 간에 전용 프라이빗 연결을 설정할 수 있음
- 대역폭 처리량을 높이고 인터넷 기반 연결 또는 VPN 연결보다 더 일관된 네트워크 환경을 제공

### VPC End-Point

- 지원되는 AWS 서비스에 VPC를 비공개로 연결할 수 있는 가상 디바이스
- S3 또는 DynamoDB로 전달되는 트래픽에 대한 라우팅 테이블에서 경로의 대상으로 지정하는 게이트웨이

---

## VPC Security

- **보안 그룹**
  - 인스턴스 수준에서 작동
  - 인스턴스에 대한 인바운드 및 아웃바운드 트래픽을 제어하는 가상 방화벽
- **네트워크 액세스 제어 목록(ACL)**
  - 서브넷 수준에서 작동
  - 서브넷에서 송수신되는 트래픽을 제어
- **보안 그룹과 네트워크 ACL 비교**
  ![그림6](/assets/AWS/Cloud_Foundations5/6.png)

---

## Amazon Route 53

- 가용성과 확작성이 우수한 DNS 웹 서비스
- 지원하는 라우팅 정책
  ![그림7](/assets/AWS/Cloud_Foundations5/7.png)
- 도메인 구매 가능

---

## Amazon CloudFront

- 빠르고 안전한 글로벌 CDN(Contents Delivery Network) 서비스
- R53 geolocation routing 사용
- 사용한 만큼 비용을 지불
