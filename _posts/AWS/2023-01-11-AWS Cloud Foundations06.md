---
title: module 6(컴퓨팅)
tags: AWS(Cloud_Foundations)
---

## Summry

본 문서에서는 AWS Academy Cloud Foundations module 6(컴퓨팅)를 정리한다.  
서비스 별 기능과 개요에 대해 정리된 문서이므로 해당 서비스를 사용하기 위해선 AWS 가이드를 참조할 것

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## AWS Computing Service

1. **Amazon EC2**
   - 크기 조정이 가능한 가상 머신 제공
2. **Amazon EC2 Auto Scaling**
   - EC2 Instance를 자동으로 시작하거나 종료하는 조건을 정의할 수 있도록 하여 애플리케이션 가용성을 지원
3. **Amazon Elastic Container Registry(Amazon ECR)**
   - Docker 이미지를 저장하고 검색하는 데 사용
4. **Amazon Elastic Container Service(Amazon ECS)**
   - Docker를 지원하는 container orchestration service
5. **VMware Cloud on AWS**
   - 맞춤형 하드웨어 없이 하이브리드 클라우드를 프로비저닝
6. **AWS Elastic Beanstalk**
   - 간편하게 web application을 실행하고 관리
7. **AWS Lambda**
   - serverless computing solution으로 사용한 컴퓨팅 시간에 대해서만 비용 지불
8. **Amazon Elastic Kubernetes Service(Amazon EKS)**
   - AWS에서 관리형 Kubernetes를 실행
9. **Amazon Lightsail**
   - Application 또는 web site를 구축할 수 있는 간편한 서비스
10. **AWS Batch**
    - 규모에 관계없이 배치 작업을 실행할 수 있는 도구를 제공
11. **AWS Fargate**
    - 서버 또는 클러스터를 관리해야 하는 부담이 적은 컨테이너를 실행하는 서비스
12. **AWS Outposts**
    - 온프레미스 데이터 센터에서 엄선된 AWS 서비스를 실행할 수 있도록 지원
13. **AWS Serverless Application Repository**
    - serverless application을 검색, 배포 및 게시

- **컴퓨팅 서비스 분류**

![그림1](/assets/AWS/Cloud_Foundations6/1.png)

---

## Amazon EC2

### 인스턴스를 생성할 떄 결정해야할 주요 결정사항

1. **Amazon Machine Image(AMI)**
   - Image를 캡처해 다른 리전에서 동일한 환경으로 시작하거나 온프레미스의 Image를 캡처해 가져올 수도 있음
2. **Instance Type**

   - EC2의 스펙을 결정(RAM, CPU 등..)
   - 네트워킹 대역폭을 고려해야함
     - a1.medium Instance는 초당 최대 10GIB but p3dn.24xlarge Instance는 초당 최대 100GIB 제공
   - Instance Type 선택 가이드

     ![그림2](/assets/AWS/Cloud_Foundations6/2.png)

   - C5 Instance use case
     - 과학적 모델링
     - 배치 처리
     - 광고 서비스
     - 대규모 확장이 가능한 멀티플레이어 게임
     - 비디오 인코딩 등
   - R5 Instance use case
     - 고성능 데이터베이스
     - 데이터 마이닝
     - 데이터 분석
     - 인 메모리 데이터 베이스
     - 비정형 빅 데이터의 실시간 처리
     - Apache Spark 클러스터
     - 기타 엔터프라이즈 애플리케이션

3. **Network Setting**
   - VPC 및 서브넷(선택 사항) 확인
   - Public IP 주소 자동 할당 여부(인터넷 액세스 허용 여부)
     - VPC 선택 안하면 기본 VPC가 할당되며 Public IP 주소가 할당됨
4. **IAM Role**
   - EC2 Instance의 software가 다른 AWS service와 상호 작용해야 하는지 여부
   - 시작 시에만 설정하는건 아니며 기존 Instance에 대해서도 설정 가능
5. **User Data Script**
   - 인스턴스 시작 시 데이터 스크립트 지정(선택 사항)
   - ex) yum update -y
   - 인스턴스가 처음 시작될 때 스크립트가 실행됨
6. **Storage Option**
   - 아래 항목들 지정
     - 디스크 크기
     - 볼륨 유형
     - 인스턴스 종료 시 볼륨 삭제 여부
     - 암호화 사용 여부
   - Amazon Elastic Block Store(Amazon EBS)
     - 내구성이 뛰어난 블록 수준 스토리지 볼륨
     - 인스턴스를 중지한 후 다시 시작할 수 있으며 데이터가 그대로 유지
   - Amazon EC2 Instance Store
     - 스토리지는 EC2 Instance가 실행 중인 호스트 컴퓨터에 연결된 디스크에서 제공
     - 인스턴스가 중지되면 저장된 데이터가 삭제
   - 기타 스토리지 옵션
     - Amazon Elastic File System(Amazon EFS) - 파일 시스템 탑재
     - Amazon Simple Storage Service(Amazon S3) - S3에 연결
7. **Tag**
   - AWS 리소스에 할당할 수 있는 레이블
   - 태깅은 EC2 인스턴스에 메타데이터를 연결할 수 있는 방법
   - 필터링, 자동화, 비용 할당 및 액세스 제어의 잠재적 이점이 있음
8. **Security Group**
   - 인스턴스에 대한 트래픽을 제어하는 방화벽 규칙 세트
   - ex) SSH port 22 에 대해 특정 IP만을 허용
9. **Key pair**
   - 인스턴스에 대한 보안 연결 지원
   - Window AMI의 경우 프라이빗 키를 사용하여 인스턴스에 로그인하는 관리자 암호 획득
   - Linux AMI의 경우 프라이빗 키를 사용하여 SSH를 통해 인스턴스에 연결

- **기타 옵션**
  - AWS CLI를 사용하여 EC2 Instance 생성
  - 프로그래밍 방식으로 EC2 Instance 생성

### Amazon EC2 Instance Life Cycle

![그림3](/assets/AWS/Cloud_Foundations6/3.png)

- EBS는 비휘발성 스토리지
- 인스턴스가 중지되었다가 다시 시작된 경우 Public IPv4 주소와 외부 DNS 호스트 네임이 변경됨(탄력적 IP로 해결 가능)
- 종료된 Instance는 복구 불가

---

## Amazon EC2 비용 최적화

### Amazon EC2 요금 모델

![그림4](/assets/AWS/Cloud_Foundations6/4.png)

</br>

- **Amazon EC2 요금 모델 : 이점**
  ![그림5](/assets/AWS/Cloud_Foundations6/5.png)

</br>

- **Amazon EC2 요금 모델 : 사용 사례**
  ![그림6](/assets/AWS/Cloud_Foundations6/6.png)

### 비용 최적화의 4가지 원칙

1. **적절한 크기**

   - 성능 요구사항을 충족할 수 있는 선에서 서버 크기를 축소하거나 해제

     ![그림7](/assets/AWS/Cloud_Foundations6/7.png)

2. **탄력성 증대**

   - 유휴 상태의 서버 용량을 줄이도록 배포를 설계

     - 자동 조정을 사용하여 최대 부하(peak load)를 처리하는 것과 같이 탄력적인 설계

     ![그림8](/assets/AWS/Cloud_Foundations6/8.png)

3. **최적의 요금 모델**

   - 현재 사용량 패턴을 분석하여 적절한 요금 조합으로 EC2 Instance를 실행

     ![그림9](/assets/AWS/Cloud_Foundations6/9.png)

4. **스토리지 선택 최적화**

   - 사용하지 않는 스토리지 오버헤드를 줄이고 스토리지 성능 요구 사항을 충족할 수 있는 더 저렴한 스토리지 옵션을 선택

     ![그림10](/assets/AWS/Cloud_Foundations6/10.png)

---

## Container Service

- 컨테이너는 운영 체제를 가상화하는 방식 중 하나이다.

- **컨테이너 이점**
  1. 반복 가능
  2. 독립형 실행 환경
  3. 소프트웨어가 서로 다른 환경에서 동일하게 실행됨
     - 개발자의 랩탑, 테스트, 프로덕션 등
  4. 가상 머신보다 빠르게 시작하거나 중지, 종료할 수 있음
- **컨테이너와 가상 머신의 차이**
  ![그림11](/assets/AWS/Cloud_Foundations6/11.png)
  - 하이퍼바이저에서 직접 실행되는 가상 머신과 달리 컨테이너는 Docker 호스트 소프트웨어와 사전 요구 사항을 지원하는 적절한 커널 기능이 있는 모든 운영 체제에서 실행됨
- **Amazon Elastic Container Service(Amazon ECS)**
  - Docker container의 실행을 오케스트레이션하는 확장 가능한 고성능 컨테이너 관리 서비스
  - Amazon ECS 클러스터 옵션
    ![그림12](/assets/AWS/Cloud_Foundations6/12.png)
  - Kubernetes
    ![그림13](/assets/AWS/Cloud_Foundations6/13.png)
  - Amazon Elastic Kubernetes SErvice(Amazon EKS)
    - 자체 Kubernetes 설치, 운영 및 유지 관리할 필요 없이 AWS에서 Kubernetes를 손쉽게 실행 가능함. 즉, Kubernetes 클러스터 관리 대폭 간소화
    - EC2 Instance를 시작하고 각 인스턴스에 Docker를 설치한 다음 클러스터에 Kubernetes를 설치하여 관리하는 것 보다는 EKS 사용을 권장
    - ECS, EKS 두 서비스 모두 Docker 컨테이너를 오케스트레이션 하는 기능이 있지만 두 서비스 모두 제공하는 이유는 고객에게 유연한 옵션을 제공하기 위함
      - 요구사항에 적합한 옵션을 선택할 것
  - Amazon Elastic Container Registry(Amazon ECR)
    - 개발자가 Docker 컨테이너 이미지를 손쉽게 저장, 관리 및 배포할 수 있게 해주는 완전관리형 Docker 컨테이너 레지스트리
    - 모든 컨테이너의 저장 및 관리를 추적

---

## AWS Lambda

- **서버를 프로비저닝하거나 관리할 필요 없이 코드를 실행할 수 있는 서버리스 컴퓨팅 서비스**

- 코드는 트리거된 경우에만 실행되며 사용한 컴퓨팅 시간(100 밀리초 단위로 측정)에 대해서만 비용을 지불함
- Lambda는 함수가 처리하는 모든 요청을 자동으로 기록
- **AWS Lambda 함수 구성**
  1. 함수 이름 지정
  2. 런타임 환경 지정
     - Python이나 Node.js 등 선택
  3. 실행 역할 부여(IAM 권한)
     - 필요에 따라 다른 AWS 서비스와 상호 작용하려면 이러한 권한이 필요
  4. 함수 구성
     - 트리거 추가
     - 사용 가능한 이벤트 소스 지정
  5. 함수 코드 작성
  6. 함수에 할당할 메모리를 MB단위로 지정(최대 3,008MB)
  - 필요한 경우 제한 시간, 함수를 실행할 특정 VPC 및 기타 설정과 같은 환경 변수를 지정할 수 있음
- Lambda 함수 예제 : EC2 인스턴스 시작 및 중지
  ![그림14](/assets/AWS/Cloud_Foundations6/14.png)
- AWS Lambda 제한
  ![그림15](/assets/AWS/Cloud_Foundations6/15.png)

---

## AWS Elastic Beanstalk

- **Web Application을 쉽게 시작 및 실행할 수 있는 방법**
  - 서버, 데이터베이스, 로드 밸런서, 방화벽, 네트워크를 구성 및 관리하는데 시간을 쓰지 않고 코드만 작성하면 됨
- **다음을 자동으로 처리하는 관리형 서비스**
  - 인프라 프로비저닝 및 구성
  - 배포
  - 로드 밸런싱
  - 자동 조정(Auto Scaling)
    - 이 설정을 사용하여 Application의 특정 요구 사항에 따라 Application을 자동으로 확장 및 축소할 수 있음
    - CPU 사용률 지표를 사용하여 자동 조정 작업을 트리거할 수도 있음
  - 상태 모니터링
  - 분석 및 디버깅
  - 로깅
- Elastic Beanstalk에 대한 추가요금 없음
  - 사용한 리소스(EC2, S3 등)에 대해서만 요금 지불
- AWS Elastic Beanstalk 배포
  ![그림16](/assets/AWS/Cloud_Foundations6/16.png)
