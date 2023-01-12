---
title: module 10(자동 조정 및 모니터링)
tags: AWS(Cloud_Foundations)
---

## Summry

본 문서에서는 AWS Academy Cloud Foundations module 10(자동 조정 및 모니터링)를 정리한다.  
서비스 별 기능과 개요에 대해 정리된 문서이므로 해당 서비스를 사용하기 위해선 AWS 가이드를 참조할 것

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## Elastic Load Balancing

- **수신되는 애플리케이션 또는 네트워크 트래픽을 단일 가용 영역 또는 여러 가용 영역의 여러 대상에 분산.**
    - 애플리케이션에 대한 트래픽이 시간이 지남에 따라 변화하면 로드 밸런서의 크기 조정할 것
- **로드 밸런서 타입**
    
    ![그림1](/assets/AWS/Cloud_Foundations10/1.png)
    
- **Elastic Load Balancing 작동 방식**
    
    ![그림2](/assets/AWS/Cloud_Foundations10/2.png)
    
- **Elastic Load Balancing 사용 사례**
    
    ![그림3](/assets/AWS/Cloud_Foundations10/3.png)
    
    1. 고가용성 및 내결함성 - Elastic Load Balancing은 여러 가용 영역의 정상적인 대상 간에 트래픽을 밸런싱한다. 
    단일 가용 영역에 있는 하나 이상의 대상이 정상 상태가 아닐 경우, Elastic Load Balancing은 다른 가용 영역에 있는 정상 상태인 대상으로 대상 트래픽을 라우팅한다. 대상이 정상 상태로 복구되면 대상에 대한 트래픽을 자동으로 재개
    2. 향상된 컨테이너 - 로드 밸런서를 사용하여 컨테이너화된 애플리케이션을 동일한 EC2 인스턴스의 여러 포트에 걸쳐 자동으로 밸런싱할 수 있다.
    완전관리형 컨테이너 상품을 제공하는 Amazon Elastic Container Service와의 완벽한 통합도 활용할 수 있다. ex) 서비스를 로드 밸런서에 등록하기만 하면 Amazon ECS가 Docker 컨테이너의 등록 및 등록 취소를 투명하게 관리한다.
    3. 자세한 사용 사례는 AWS 웹 사이트 참조
- **로드 밸런서 모니터링**
    
    ![그림4](/assets/AWS/Cloud_Foundations10/4.png)
    
    - 모니터링 활용 예 1 - AWS CloudWatch 경보를 생성하여 지정된 지표를 모니터링할 수 있으며 지표가 허용 범위를 벗어난다고 간주되는 경우 이메일 주소로 알림을 전송하는 등의 작업을 시작하고 해당 메시지 전송
    액세스 로그를 사용하여 로드 밸런서에 보낸 요청에 대한 자세한 정보를 캡처하고 Amazon S3에 로그 파일로 저장할 수 있다.
    - 모니터링 활용 예 2 - AWS CloudTrail을 사용하여 Elastic Load Balancing API에 보낸 호출에 대한 자세한 정보를 캡처하고 이를 Amazon S3에 로그 파일로 저장할 수 있다.
    이 로그를 사용하여 누가 호출했는지, 언제 어떤 호출이 이루어졌는지 어떤 소스 IP 주소에서 호출했는지 등을 확인할 수 있다.

---

## Amazon CloudWatch

- **AWS를 효율적으로 사용하기 위해서는 AWS 리소스에 대한 통찰력이 필요하다. 아래와 같은 정보를 얻기 위해 Amazon CloudWatch를 사용하여 정보를 캡처한다.**
    - 더 많은 Amazon EC2 인스턴스를 시작해야 하는 경우 용량이 충분하지 않아서 애플리케이션의 성능 또는 가용성이 영향을 받고 있는 지, 실제로 사용되고 있는 인프라의 양 등을 알아야 할 수 있다.
    
        ![그림5](/assets/AWS/Cloud_Foundations10/5.png)
    
    - 예를 들어 EC2 인스턴스 CPU 사용률 elastic Load Balancing 요청 지연 시간, Amazon DynamoDB 테이블 처리량, Amazon Simple Queue Service 대기열 길이 또는 AWS 청구서의 요금에 대한 경보를 생성할 수 있다. 뿐만 아니라 사용자 지정 지표에 대한 경보도 생성할 수 있다.
- **Amazon CloudWatch를 사용하면 시스템 전체의 리소스 사용률, 애플리케이션 성능 및 운영 상태를 파악할 수 있다. 사전 약정이나 최소 비용은 없으며 사용한 만큼만 비용을 지불한다.**

---

## Amazon EC2 Auto Scaling

- Scaling(조정) 이란?
    - **Scaling은 애플리케이션의 컴퓨팅 용량을 늘리거나 줄이는 기능이다.**
        
        ![그림6](/assets/AWS/Cloud_Foundations10/6.png)
        
        - 수요일에 발생하는 가장 높은 수요를 충족하도록 용량을 프로비저닝하면 대부분의 요일에 활용도가 낮은 상태로 리소스를 실행하게 되며 이는 비용최적화와 거리가 멀다.
        반대로 더 적은 용량을 할당하여 비용을 줄이면 특정 요일에 용량이 부족하게 된다.
        - Auto Scaling은 이처럼 유동적인 서비스 수요를 지원하는 데 필요하다.

</br>

- **Amazon EC2 Auto Scaling은 애플리케이션 가용성을 유지하면서 사용자가 정의한 조건에 따라 EC2 인스턴스를 자동으로 추가 또는 제거할 수 있는 AWS 서비스이다.**
    
    ![그림7](/assets/AWS/Cloud_Foundations10/7.png)
    
- **Auto Scaling Group**
    
    ![그림8](/assets/AWS/Cloud_Foundations10/8.png)
    
    - Auto Scaling Group의 크기는 사용자가 원하는 용량으로 구성한 인스턴스 수에 따라 달라진다.

- **Amazon EC2 Auto Scaling의 작동 방식**
    
    ![그림9](/assets/AWS/Cloud_Foundations10/9.png)
    
    1. **현재 수치 유지** - Auto Scaling Group에서 실행 중인 인스턴스에 대해 주기적으로 상태 확인 및 비정상 인스턴스 발견 시 새 인스턴스로 교체
    2. **수동 조정** - Auto Scaling Group의 최대 용량과 최소 용량 또는 원하는 용량의 변동만 지정
    3. **예약된 조정** - 날짜 및 시간에 따라 조정 작업이 자동으로 수행
        - 하루 중, 주중 또는 월중에 그룹의 인스턴스 수를 언제 늘리거나 줄여야 할지 정확히 알 수 있는 예측 가능한 워크로드를 지원하는 데 유용
    4. **동적 조정** - 조정 정책을 사용하여 조청 프로세스를 제어하는 파라미터를 정의할 수 있다.
        - 온디맨드 조정을 위한 조정 이벤트에서는 Amazon CloudWatch를 사용하여 CPU 사용률을 확인하고 사전 정의된 CPU 사용률 임계값을 위반한 경우에만 조정할 수 있다.
        - 동적 구성 예
            1. EC2 인스턴스 또는 로드 밸런서에서 수집된 성능 정보를 기반으로 하는 CloudWatch 경보를 생성
            2. 성능 임계값을 초과하면 CloudWatch 경보가 Auto Scaling Group의 EC2 인스턴스에서 확장 또는 축소되는 Auto Scaling 이벤트를 트리거
    5. **예측 조정** - Amazon EC2 Auto Scaling과 AWS Scaling을 함께 사용하여 예측 조정을 구현할 수 있다. 이 경우 예측된 수요에 따라 용량이 조정된다.
        - 예측 조정에서는 실제 EC2 사용량에서 수집한 데이터를 사용하며, AWS의 관찰 결과에서 가져온 수십억 개의 데이터 포인트가 추가로 참조된다.
        - 그 다음 AWS는 잘 학습된 기계 학습 모델을 사용하여 일일 및 주간 패턴을 비롯한 예상 트래픽 및 EC2 사용량을 예측한다.
        모델이 예측을 시작하려면 최소한 하루 동안의 기록 데이터가 필요하며 24시간마다 다시 평가하여 다음 48시간에 대한 예측을 생성한다.