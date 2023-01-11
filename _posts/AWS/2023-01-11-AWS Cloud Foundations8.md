---
title: module 8(데이터베이스)
tags: AWS(Cloud_Foundations)
---

## Summry

본 문서에서는 AWS Academy Cloud Foundations module 8(데이터베이스)를 정리한다.  
서비스 별 기능과 개요에 대해 정리된 문서이므로 해당 서비스를 사용하기 위해선 AWS 가이드를 참조할 것

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### Amazon Relational Database Service

- **직접 관리형 데이터베이스의 문제점**  
  ![그림1](/assets/AWS/Cloud_Foundations8/1.png)

- **Amazon RDS는 지속적인 관리 없이 관계형 데이터베이스를 설정, 운영 및 확장**  
  ![그림2](/assets/AWS/Cloud_Foundations8/2.png)

- **Amazon RDS 아키텍쳐**  
  ![그림3](/assets/AWS/Cloud_Foundations8/3.png)

  - 데이터베이스 인스턴스는 프라이빗 서브넷에서 격리되어 있으며 사용자가 선택한 애플리케이션에서만 직접 액세스할 수 있다.

- **Amazon RDS 사용사례**  
  ![그림4](/assets/AWS/Cloud_Foundations8/4.png)
  ![그림5](/assets/AWS/Cloud_Foundations8/5.png)
  - Amazon RDS가 요구 사항을 해결하기에 적합하지 않다면 Amazon Aurora 또는 Amazon DynamoDB와 같은 다른 데이터베이스 서비스 중 하나를 고려해야 한다.
- **DB 엔진, 크기, 메모리 클래스, 실행 시간, 데이터 전송 등을 추산하여 비용 결정**
  - 예약 인스턴스를 통해 저렴하게 사용 가능
- **다중 AZ 배포를 통한 고가용성을 실현할 수 있음**
- **읽기 작업 수가 많은 애플리케이션이 있는 경우 읽기 전용 복제본을 만들어 사용 가능**

---

### Amazon DynamoDB

- **Amazon DynamoDB는 규모에 관계없이 10밀리초 미만의 일관된 지연 시간이 요구되는 모든 애플리케이션을 위한 빠르고 유연한 NoSQL 데이터베이스 서비스이다.**
  ![그림6](/assets/AWS/Cloud_Foundations8/6.png)
- 관계형 데이터베이스와 비관계형 데이터베이스의 비교
  ![그림7](/assets/AWS/Cloud_Foundations8/7.png)

---

### Amazon Redshift

- **표준 SQL과 기존 비즈니스 인텔리전스 도구를 사용하여 모든 데이터를 간편하고 비용 효율적으로 분석할 수 있는 완전관리형의 고속 데이터 웨어하우스이다.**
  - 정교한 쿼리 최적화, 고성능 로컬 디스크의 열 형식 스토리지 대량 병렬 쿼리 실행 기능을 사용하여 페타바이트 규모의 정형 데이터에 복잡한 분석 쿼리를 실행할 수 있다. 대부분의 결과는 몇 초 내에 반환된다.
- 자동화 및 조정 기능을 포함하고 있으며 저장된 데이터와 전송 중인 데이터에 대해 강력한 암호화를 제공하도록 설계되었다.
- Amazon Redshift 사용 사례
  ![그림8](/assets/AWS/Cloud_Foundations8/8.png)
  ![그림9](/assets/AWS/Cloud_Foundations8/9.png)

---

### Amazon Aurora

- **MySQL 및 PostgreSQL과 호환되는 관계형 데이터베이스로, 고성능 상용 데이터베이스의 속도와 가용성에 오픈 소스 데이터베이스의 간편함과 비용 효율성을 결합했다.**
  - 완전관리형 서비스로 프로비저닝 패치 적용, 백업, 복구, 장애 탐지, 복원 등 시간이 많이 소요되는 작업을 자동화하도록 설계되었다.
  - 또한 Aurora 아키텍처가 기본적으로 고가용성을 제공하므로 고가용성을 위해 구성해야 할 항목이 없다.
  - 여러 가용 영역에 여러 데이터 복사본을 저장하여 고가용성을 실현하며, Amazon S3에 지속적으로 백업된다.
  - 최대 15개의 읽기 전용 복제본을 사용하여 읽기 작업이 많은 사용 사례의 성능을 크게 개선할 수 있다.
- AWS Database Migration Service 및 AWS Schema Conversion Tool과 원할하게 통합된다. 이러한 도구를 활용하면 레거시 관계형 데이터베이스 관리 시스템의 데이터를 Amazon Aurora로 간편하게 이동할 수 있다.
