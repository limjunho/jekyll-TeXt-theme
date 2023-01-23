---
title: Spot hibernation mode
tags: AWS
---

## Summry

본 문서에서는 Spot Instance에서 hibernation mode(최대 절전 모드)에 대해 정리한다.

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## 주요 기능

1. **최대 절전 모드를 시작하면 인스턴스가 stopping상태로 이동. Amazon EC2는 최대 절전 모드(디스크 일시 중단)를 수행하도록 운영 체제에 신호를 보낸다.**
2. **최대 절전 모드는 모든 프로세스를 중지하고 RAM의 내용을 EBS 루트 볼륨에 저장한 다음 정기적으로 종료한다.**
   - 모든 EBS 볼륨은 인스턴스에 연결된 상태로 유지되며 RAM의 저장된 콘텐츠를 포함하여 데이터가 지속된다.
3. **인스턴스가 최대 절전 모드인 동안에는 EBS 볼륨에 대해서만 요금이 부과된다.**
4. **애플리케이션이 중지된 부분부터 다시 시작된다.(마치 노트북 덮개를 여닫는 것처럼)**

## 고려 사항

1. **Amazon EC2만 스팟 인스턴스를 최대 절전 모드로 전환 및 재개할 수 있다.**
2. **스팟 인스턴스를 수동으로 최대 절전 모드로 전환 및 재개할 수 없다.**
   - Amazon EC2는 용량을 사용할 수 있게 되면 인스턴스를 재개

즉, 사용자가 hibernation mode를 컨트롤 할 수 없다.

## 제한 사항

1. **인스턴스 RAM 크기는 100GB 미만이어야 한다.**
2. **EBS 루트 볼륨 크기는 최대 절전 모드 동안 인스턴스 메모리(RAM)를 저장할 수 있을 만큼 충분히 커야한다.**
3. **지원되는 인스턴스 패밀리**

- C3, C4, C5, M4, M5, R3, R4

4. **EBS 볼륨을 사용해야 한다.(EC2 인스턴스 스토어 볼륨은 인스턴스에 연결된 상태로 유지되지만 볼륨의 데이터는 손실)**

---

## Reference

[Overview of hibernation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-hibernate-overview.html)

[Instance lifecycle](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-lifecycle.html#lifecycle-differences)

[Hibernate interrupted Spot Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/hibernate-spot-instances.html)
