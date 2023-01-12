---
title: module 3(AWS 글로벌 인프라 개요, AWS 서비스 소개)
tags: AWS(Cloud_Foundations)
---

## Summry

본 문서에서는 AWS Academy Cloud Foundations module 3(AWS 글로벌 인프라 개요, AWS 서비스 소개)를 정리한다  
서비스 별 기능과 개요에 대해 정리된 문서이므로 해당 서비스를 사용하기 위해선 AWS 가이드를 참조할 것

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## AWS 글로벌 인프라

- region간 통신에는 AWS backbone network infrastructure가 사용되며 각 AWS region은 완전한 이중화 및 네트워크 연결을 제공
- region은 일반적으로 2개 이상의 가용 영역으로 구성됨
- 하나의 region에서의 데이터 복제는 aws에서 지원하나 aws region 간의 데이터 복제는 고객이 제어
- **region 을 선택할 때 고려할 것**
  1. data governance, 법적 요구사항
  2. 고객에 대한 근접성(지연 시간)
     1. 사용자의 위치와 region의 지연성을 테스트 하려면 Cloude Ping과 같은 곳에서 테스트 가능
  3. 리전 내에서 사용 가능한 서비스
  4. 비용(region 별로 다름)
- 복원력을 위해 여러 가용 영역에 걸쳐 데이터와 리소스를 복제하는 것이 좋다.

---

## AWS 서비스 및 서비스 범주 개요

- 다양한 서비스가 있으나 이번 강의에서는 많이 사용되는 서비스를 특정하여 소개함

  ### Storage Service

  - **AWS Simple Storage Service(S3)**
  - **Amazon Elastic Block Store(EBS)**
    - 높은 처리량이 요구되는 트랜잭션 집약적 workload에 EC2와 함께 사용될 수 있도록 설계된 고성능 블록 스토리지이다.
  - **Amazon Elastic File System(EFS)**
    - AWS cloud service 및 on-premise 리소스 내에서 사용할 수 있는 확장 가능하고 탄력적인 완전관리형 Network File System(NFS)을 제공한다.
  - **Amazon Simple Strorage Services Glacier**
    - 데이터 아카이빙 및 장기 백업을 위한 안전하고 내구성이 높으며 비용이 매우 저렴한 AWS S3 cloud 스토리지 클래스이다.

  ### Computing service

  - **Amazon Elastic Compute Cloud(EC2)**
    - 크기 조정이 가능한 컴퓨팅 용량을 cloud의 가상 머신으로 제공
  - **Amazon EC2 Auto Scaling**
    - 정의한 조건에 따라 EC2 instance를 자동으로 추가하거나 제거할 수 있다.
  - **Amazon Elastic Container Service(ECS)**
    - Docker container를 지원하는 확장성이 뛰어난 고성능 container orchestration service이다.
  - **Amazon Elastic Container Registry Service(ECR)**
    - 개발자가 Docker container 이미지를 손쉽게 저장, 관리 및 배포할 수 있게 해주는 완전관리형 Docker container 레지스트리이다.
  - **AWS Elastic Beanstalk**
    - Apache 및 Microsoft Internet Information Services(IIS)와 같은 잘 알려진 서버에 web application 및 service를 배포하고 확장할 때 사용되는 service이다.
  - **AWS Lambda**
    - 서버를 provisioning하거나 관리하지 않고도 코드를 실행할 수 있으며 사용한 컴퓨팅 시간에 대한 요금만 지불(코드 실행 안하면 비용 X)한다.
  - **Amazon Elastic Kubernetes Service(EKS)**
    - AWS에서 Kubernetes를 사용하는 container식 application을 손쉽게 배포하고 관리 및 확장할 수 있다.
  - **AWS Fargate**
    - 서버 또는 클러스터를 관리할 필요 없이 container를 실행할 수 있는 Amazon ECS용 컴퓨팅 엔진이다.

  ### Database Service

  - **Amazon Relational Database Service(RDS)**
    - cloud에 간편하게 설정할 수 있고 확장 가능한 관계형 database
    - 크기 조정이 가능한 용량을 제공하며 하드웨어 provisioning, database 설정 패치 적용, 백업과 같은 시간 소모적인 관리 작업을 자동화
  - **Amazon Aurora**
    - MySQL 및 PostgreSQL과 호환되는 관계형 database
    - 표준 MySQL DB보다 최대 5배 빠르며 표준 Postgres DB보다 3배 빠르다.
  - **Amazon Redshift**
    - 로컬로 저장된 페타바이트 규모의 데이터에 대해 분석 쿼리를 실행할 수 있다.
    - 이 service는 모든 규모에서 빠른 성능을 제공한다.
  - **Amazon DynamoDB**
    - 내장된 보안, 백업 및 복원, 인 메모리 캐싱을 통해 모든 규모에서 10밀리초 미만의 성능을 제공하는 완전관리형 key-value NoSQL DB이다.

  ### Networking & Content Delivery Network(CDN)

  - **Amazon Virtual Private Cloude(VPC)**
    - AWS cloud의 논리적으로 격리된 섹션을 provisioning하여 정의된 가상 네트워크에서 AWS 리소스를 시작할 수 있다.
  - **Elastic Load Balancing(ELB)**
    - application traffic을 Amazon EC2 Instance, container, IP 주소 및 Lambda 함수와 같은 여러 대상에 자동으로 분산
    - 보통 ELB라 하면 AWS의 로드 밸런싱 서비스 전체를 통틀어 칭하는 말
      [4가지 로드밸런서 타입별 차이(ALB, ELB, CLB, NLB)](https://y-oni.tistory.com/313#toc3)
  - **Amazon CloudFront**
    - 짧은 지연 시간과 빠른 전송 속도로 데이터, 동영상 application 및 Application Programing Interface(API)를 전 세계 고객에게 안전하게 전송하는 Content Delivery Network(CDN) service이다.
  - **AWS Transit Gateway**
    - Amazon Virtual Private Cloude와 on-premise 네트워크를 중앙에서 관리하는 단일 게이트웨이에 연결할 수 있다.
  - **Amazon Route 53**
    - 최종 사용자를 인터넷 application으로 라우팅하는 안정적인 수단을 제공하도록 설계된 확장형 cloud Domain Name System(DNS) web service
  - **AWS Direct Connect**
    - 데이터 센터 또는 사무실에서 AWS로 연결되는 전용 private 네트워크 연결을 설정하는 기능을 제공하므로 비용을 대폭 줄이고 대역폭 처리량을 늘릴 수 있다.
  - **AWS VPN**
    - 네트워크 또는 디바이스에서 AWS 글로벌 네트워크로 연결되는 보안 private tunnel을 제공

  ### 보안, 자격 증명 및 규정 준수 서비스

  - **AWS Identify and Access Management(IAM)**
    - 리소스에 대한 엑세스를 안전하게 관리
  - **AWS Organizations**
    - 계정에 허용되는 service와 작업을 제한할 수 있음
  - **Amazon Cognito**
    - web 및 mobile application에 사용자 인증 및 액세스 제어 기능을 추가할 수 있음
  - **AWS Artifact**
    - AWS 보안 및 규정 준수 보고서와 일부 온라인 계약에 대한 ondemand 엑세스를 제공
  - **AWS Key Management Service(KMS)**
    - 암호화 키를 생성 및 관리할 수 있음.
  - **AWS Shield**
    - AWS에서 실행되는 application을 보호하는 관리형 DDoS 공격 방지 service

  ### AWS 비용 관리 서비스

  - **AWS 비용 및 사용 보고서**
    - 보고서에는 AWS service 요금 및 예약에 대한 추가 메타데이터를 비롯하여 사용 가능한 가장 포괄적인 AWS 비용 및 사용량 데이터 세트가 포함됨
  - **AWS 예산**
    - AWS 비용 또는 사용량이 예산 금액을 초과하거나 초과할 것으로 예상될 때 알려주는 사용자 지정 예산을 설정할 수 있음
  - **AWS Cost Explorer**
    - 사용이 쉬운 인터페이스를 통해 시간 경과에 따른 AWS 비용 및 사용량을 시각화하여 파악 및 관리할 수 있다.

  ### AWS management & governance service

  - **AWS Management Console**
    - AWS 계정에 액세스 하기 위한 web 기반 사용자 인터페이스 service
  - **AWS config**
    - 리소스 인벤토리 및 변경 사항을 추적하는데 유용
  - **Amazon CloudWatch**
    - 리소스와 application을 모니터링
  - **AWS Auto Scaling**
    - 수요에 따라 여러 리소스를 확장할 수 있는 기능을 제공
  - **AWS Command Line Interface**
    - AWS service를 관리하는 통합 도구를 제공하는 AWS CLI 인터페이스
  - **AWS Trusted Advisor**
    - AWS 모범 사례를 바탕으로 성능과 보안을 최적화하는 데 유용한 온라인 도구
  - **AWS Well-Architected Tool**
    - workload를 검토하고 개선하는 데 도움
  - **AWS CloudTrail**
    - AWS 계정 전체에서 사용자 활동 및 API 사용을 추적

  ## 용어 및 정의

  > 워크로드(Workload) : 주어진 기간에 시스템에 의해 실행되어야 할 작업의 할당량을 의미한다.
  > 프로비저닝(Provisioning) : 어떤 종류의 서비스든 사용자의 요구에 맞게 시스템 자체를 제공 하는 것을 provisioning이라고 하며 제공해줄 수 있는 것은 인프라 자원이나 서비스, 또는 장비가 될 수도 있다.
  > Content Delivery Network(CDN) : 데이터 사용량이 많은 애플리케이션의 웹 페이지 로드 속도를 높이는 상호 연결된 서버 네트워크이다
  >
  > [CDN이란 무엇인가요? - CDN 설명 - AWS](https://aws.amazon.com/ko/what-is/cdn/)
  > 쿠버네티스(Kubernetes) : 컨테이너를 쉽고 빠르게 배포/확장하고 관리를 자동화해주는 오픈소스 플랫폼
  >
  > [쿠버네티스 시작하기 - Kubernetes란 무엇인가?](https://subicura.com/2019/05/19/kubernetes-basic-1.html)
  > 오케스트레이션(orchestration) : 컴퓨터 시스템과 애플리케이션, 서비스의 자동화된 설정, 관리, 조정을 의미
  >
  > [오케스트레이션이란?](https://www.redhat.com/ko/topics/automation/what-is-orchestration)
  > 도커 레지스트리(Docker Registry) : 도커 이미지를 관리하는 일종의 저장소
  > 대표적으로 **dockerhub** 가 있으며, AWS, GCP와 같은 메이저 클라우드 사업자 또한 **컨테이너 레지스트리** 로 불리는 이미지를 보관할 수 있는 저장소를 제공하고 있다. AWS에는 Amazon Elastic Container Registry (Amazon ECR), GCP에는Artifact Registry가 있으며, Azure와 Oracle도 동일한 서비스를 제공한다.
  >
  > [[Docker / Kubernetes] 6-1. 도커 레지스트리](https://velog.io/@jkseo50/Docker-Kubernetes-6.-%EB%8F%84%EC%BB%A4-%EB%A0%88%EC%A7%80%EC%8A%A4%ED%8A%B8%EB%A6%AC)
