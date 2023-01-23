---
title: Spot Instance inturruption handling
tags: AWS
---

## Summry

본 문서에서는 작성자가 조사한 Spot Instance 중단 및 복구 방법을 정리한다.

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## 중단 대비(데이터 백업)

1. **스팟 인스턴스에 연결된 모든 볼륨의 스냅샷을 정기적으로 생성**
   - 정기적으로 생성된 스냅샷을 S3등 저렴한 스토리지에 보관하다가 새 스팟 인스턴스를 실행하거나 하게 되면 스냅샷을 프로비저닝하여 사용
   - **고려 사항**
     1. 루트 디바이스 역할을 하는 EBS 볼륨의 스냅샷을 생성할 때는 인스턴스를 중지한 후 스냅샷을 생성
     2. 크기가 큰 최초의 스냅샷이나 변경된 블록이 많은 후속 스냅샷의 경우 몇 시간씩 시간이 걸릴 수 있다.
     3. 스냅샷은 snapshot 명령을 실행할 때 Amazon EBS 볼륨에 기록된 데이터만 캡처합니다. 이때 애플리케이션이나 운영 체제에 의해 캐시된 데이터가 제외될 수 있다.
2. **인스턴스가 종료된 경우 Amazon EBS 볼륨을 종료하도록 스팟 인스턴스를 구성**
   - 인스턴스가 종료된 경우 Amazon EBS 볼륨을 삭제가 아닌 종료하도록 스팟 인스턴스를 구성하여 볼륨을 다른 인스턴스에 연결하고 볼륨에 저장된 데이터를 복구할 수 있다.
3. **EFS + Spot instance 구성**
   - Amazon EFS(Elastic File System)은 네트워크 파일 시스템 버전 4 (NFSv4.1 및 NFSv4.0) 프로토콜을 지원하므로 현재 사용하는 애플리케이션 및 도구가 EFS와 원활하게 연동된다.
   - Amazon EC2, Amazon ECS 및AWS Lambda 를 비롯한 여러 컴퓨팅 인스턴스가 Amazon EFS 파일 시스템에 동시에 액세스할 수 있다.

## 중단 대처

1. **Auto Scaling Group을 사용하여 스팟이 중단되면 대체 인스턴스 시작**
2. **로드 밸런서에서 등록 해제**
   - 다른 인스턴스로 트래픽 분산
3. **Spot hibernation mode**
   - 최대 절전모드로 대기, 추후 중지된 부분부터 시작(EC2만 사용 가능)

- Auto Scaling Group, Load Balancer를 사용하여 중단에 대처하는 사례는 Reference를 참고

웹 애플리케이션처럼 트래픽을 수용해야 하는 작업이 아니더라도 중지돼서는 안되는 작업이라면 위의 방법을 사용하여 중단에 대처할 수 있다.

## 복구

- 새 인스턴스 생성 시 중단 대비에서 수행한 작업의 결과물을 토대로 복구 작업 수행

**Jupyter Notebook과 같은 인터프리터를 이용한 작업은 스팟 재배치 시 리소스 손실을 피하기 어려울 듯 함.**

> 큰 작업들에 대해서는 코드를 분리하여 컴파일하고 Jupyter Note에서는 그 실행파일들을 실행하도록 하면 어느정도 대응이 가능할 듯 하다. 다만 코드를 분리하고 컴파일하고 이런 작업들은 개발자가 직접 수행해야 하므로 자동화가 어려운 점이 아쉽다. (DNN 학습 및 추론 작업이라면 AWS SageMaker 사용을 고려할 것, 스팟 중단 관리를 제공함)

---

## 작성자의 생각

AWS Lambda SnapStart 기능을 고려해보는 것도 좋은 방법일 듯 싶다.

최근에 발표된 기능으로(22.11) 아직 많은 기능이 제한적이다. **기존 lambda의 cold start를 사실 상 완전히 제거하는 방향으로 출시되었다.**  
기존의 lambda는 downloading the function’s code, starting the runtime and any external dependencies, and running the function’s initialization code와 같은 작업을 초기화(init) 단계에서 수행했고 이 작업이 cold start 문제점이었다. 따라서 개발자들은 특정 주기마다 lambda 함수를 호출하여 warm 상태를 유지하도록 유도 하였다.

Lmbda SnapStart는 이 과정을 제거하기 위해 최초 초기화 시 메모리 및 디스크에 대한 스냅샷을 생성한다. 메모리까지 스냅샷에 포함하기 때문에 고유성은 떨어진다고 한다.(예를 들어 난수 생성이 매번 동일) 이것을 해결하는 방법은 여러가지가 있고 작성자는 메모리 스냅샷에 주목했다. 스팟 재배치 시의 리소스 손실을 어느정도 해결 가능할 것이라 생각한다.

다만 출시 후 아직 2개월이 채 안된 기능인지라 사용 가능한 리전과 언어가 제한되어 있다. Lambda의 cold start를 개선하고자 출시된 기능이기 때문에 경과를 지켜보면 더 많은 언어와 프레임워크를 지원할 것이다.

- 사용 가능 리전 - 미국 동부(오하이오, 버지니아 북부), 미국 서부(오레곤), 아시아 태평양(싱가포르, 시드니, 도쿄) 및 유럽(프랑크푸르트, 아일랜드, 스톡홀름) 리전
- 사용 가능 언어 - JAVA

## Reference

[종료된 스팟 인스턴스 복구](https://aws.amazon.com/ko/premiumsupport/knowledge-center/spot-instance-terminate/)

[Amazon Elastic File System 이란 무엇입니까?](https://docs.aws.amazon.com/ko_kr/efs/latest/ug/whatisefs.html)

[Amazon EC2 스팟 인스턴스로 확장성 높은 웹 애플리케이션 운영하기 - Amazon Web Services](https://aws.amazon.com/ko/blogs/korea/running-high-scale-web-on-spot-instances/)

[Amazon EBS 스냅샷](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/EBSSnapshots.html)

[Amazon EBS 스냅샷 생성](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/ebs-creating-snapshot.html)

---

### AWS Lambda SnapStart

[AWS Lambda SnapStart : 함수 실행 속도 및 성능 가속화 기능 - Amazon Web Services](https://aws.amazon.com/ko/blogs/korea/new-accelerate-your-lambda-functions-with-lambda-snapstart/)

[Starting up faster with AWS Lambda SnapStart - Amazon Web Services](https://aws.amazon.com/ko/blogs/compute/starting-up-faster-with-aws-lambda-snapstart/)

[AWS re:Invent 2022 - AWS Lambda SnapStart: Fast cold starts for your Java functions (SVS320)](https://www.youtube.com/watch?v=ZbnAithBNYY&ab_channel=AWSEvents)

[Lambda SnapStart: What we know from re:Invent 2022](https://acloudguru.com/blog/business/lambda-snapstart-reinvent-2022)
