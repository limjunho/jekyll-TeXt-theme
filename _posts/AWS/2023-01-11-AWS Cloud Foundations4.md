---
title: AWS 클라우드 보안
tags: AWS(Cloud_Foundations)
---

## Summry

본 문서에서는 AWS Academy Cloud Foundations module 3(AWS 글로벌 인프라 개요, AWS 서비스 소개)를 정리한다. 서비스 별 기능과 개요에 대해 정리된 문서이므로 해당 서비스를 사용하기 위해선 AWS 가이드를 참조할 것

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## AWS 공동 책임 모델

![그림1](/assets/AWS/Cloud_Foundations4/1.png)

- **AWS의 책임**
  1. **데이터 센터의 물리적 보안**
     - 필요 기반의 제어된 액세스
  2. **하드웨어 및 소프트웨어 인프라**
     - 스토리지 폐기, 호스트 OS 액세스 로깅 및 감사
  3. **네트워크 인프라**
     - 침입 탐지
  4. **가상화 인프라**
     - 인스턴스 격리
- **고객은 클라우드에서 구현하는 모든 application과 data set을 보호할 책임이 있음**
  ![그림2](/assets/AWS/Cloud_Foundations4/2.png)
- 네트워크가 보안을 보장하도록 구성되었는지 확인
- 보안 자격 증명 및 로그인을 안전하게 관리
- 모든 컴퓨터 인스턴스에서 실행되는 OS 및 Application 보안과 firewall 구성을 관리
- AWS에서는 이를 위해 다양한 도구 및 서비스를 제공

### 서비스 특성 및 보안 책임

![그림3](/assets/AWS/Cloud_Foundations4/3.png)

![그림4](/assets/AWS/Cloud_Foundations4/4.png)

- 서비스형 소프트웨어는 즉시 사용 가능한 완전한 소프트웨어 솔루션을 제공하는 서비스

---

## AWS Identity and Access Management(IAM)

- 사용자를 정의하고 사용자에게 허용되는 액세스 유형을 정의할 수 있는 무료 서비스
- 시스템 관리자, 데이터베이스 관리자, 스토리지 관리자, 보안 관리자 등의 운영 그룹을 정의 및 운영 할 수 있음
- IAM 필수 구성 요소
  1. **IAM User**
     - AWS 계정으로 인증할 수 있는 사람 또는 applicatino
  2. **IAM Group**
     - 동일한 권한 부여를 허락받은 IAM User group
  3. **IAM Policy**
     - 액세스할 수 있는 리소스와 각 리소스에 대한 액세스 수준을 정의하는 문서
     - 사용자는 요청한 작업이 명시적으로 허용되고 어디에도 거부 문이 없는 경우에만 작업 수행 가능
  4. **IAM Role**
     - AWS 서비스 요청을 위한 권한 세트를 부여하는 메커니즘
     - 역할을 부여받은 사용자는 필요에 따라 서비스와 관련한 작업을 수행한 후 일반적인 액세스 권한으로 돌아감(linux sudo와 비슷함)

### IAM access 방식

![그림5](/assets/AWS/Cloud_Foundations4/5.png)

- 프로그래밍 방식 액세스는 AWS CLI 또는 SDK를 사용하여 AWS API를 호출할 때 액세스 키 ID와 보안 액세스 키(키 페어)를 제시해야 함
- AWS Management Console 액세스 방식을 사용하는 경우 IAM MFA를 추가로 사용하는 것을 권장(access 시 인증 코드를 입력하는 방식)
- 택 일 또는 두 방법 모두 사용 가능

---

## 새 AWS 계정 보안

- 필요한 경우를 제외하고 AWS 루트 계정을 사용하지 말것을 권장

1. **루트 사용자의 사용을 가능한 빨리 중지(IAM 사용자 생성)**
2. **루트 사용자와 모든 IAM 사용자에게 MFA를 적용**
3. **AWS CloudTrail 적용**
   - CloudTrail은 최근 90간의 계정의 활동을 추적하는 무료 서비스
4. **결제 보고서 활성화**
   - 결제 보고서는 AWS 리소스 사용에 대한 정보와 해당하는 사용에 대한 추정 비용을 제공
   - 보고서는 사용자가 지정한 Amazon S3 버킷으로 전송되며 하루 한 번 이상 업데이트됨

---

## 계정 보안

- **AWS Organizations를 사용하면 여러 AWS 계정을 통합하여 중앙에서 관리 가능**
  - 계정을 OU(조직 단위)로 그룹화하고 각 OU에 다른 액세스 정책을 연결
  - IAM에 대한 통합 및 지원
    - 사용자에 대한 권한은 AWS Organizations를 통해 허용되는 권한과 해당 계정의 IAM을 통해 부여되는 권한의 교차점.
  - 서비스 제어 정책을 사용하여 각 AWS 계정이 액세스할 수 있는 AWS 서비스 및 API 작업에 대한 제어를 설정할 수 있음
- **SCP(Service Control Policy)는 계정에 대한 중앙 집중식 제어를 제공**
  - 조직에 포함된 계정에서 사용할 수 있는 권한을 제한 및 액세스 제어 지침을 준수하는지 확인
  - SCP는 권한을 부여하지 않음. 다만 조직 또는 OU에 대한 최대 권한을 지정하는 JSON 구문 정책
- **AWS Key Management Service(AWS KMS)**
  - 암호화 키를 생성하고 관리
  - AWS 서비스 및 애플리케이션 전체의 암호화 사용을 제어
  - AWS CloudTrail과 통합할 경우 모든 키 사용을 로깅
  - FIPS(Federal Information Processing Standards) 140-2에서 검증한 HSM(Hardware Security Module)을 사용하여 키를 보호
- **Amazon Cognito**
  - 웹 및 모바일 애플리케이션에 사용자 가입, 로그인 및 액세스 제어를 추가
- **AWS Shield**
  - DDoS 방어를 위한 관리형 서비스로 AWS에서 실행되는 application을 보호
  - 상시 탐지 및 자동 인라인 완화 제공
  - AWS Shield Standard는 추가 비용 없이 활성화되며 AWS Shield Advanced는 선택 유료 서비스이다.
    - Advanced는 EC2 Instance Elastic Load Balancer, Amazon CloudFront, Amazon Global Accelerator 및 Amazon Route 53에서 실행되는 application을 목표로 하는 더 정교하고 더 큰 규모의 공격에 대해 추가적인 보호를 제공

---

## AWS 데이터 보안

### 유휴 데이터 암호화

- AWS에서 암호화된 파일 시스템을 생성하면 모든 데이터 및 메타데이터가 AES-256 알고리즘으로 저장 시 암호화됨
- AWS KMS가 지원하는 모든 서비스에 저장된 데이털르 암호화할 수 있음

### 전송 중 데이터의 암호화

- AWS Certificate Manager는 SSL/TLS 인증서를 관리, 배포 및 갱신하는 방법을 제공
- AWS Storage Gateway를 사용하는 방법도 있음

---

## 규정 준수 보장 작업

- AWS는 인증 기관 및 독립 감사자와 협력하여 AWS가 설정하고 운영하는 정책, 프로세스 및 제어에 대한 상세한 정보를 제공
- AWS Config 는 AWS 리소스의 구성을 측정, 감사 및 평가하며 구성을 지속적으로 모니터링 하는 데 사용됨
  - 규정 준수 감사 및 보안 분석을 간소화
  - 리전 별 서비스
