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
   - Amazon EC2는 용량을 사용할 수 있게 되면 인스턴스를 재개. 즉, 사용자가 hibernation mode를 컨트롤 할 수 없다.
3. **인스턴스 스토어 볼륨과 다른 탄력적 IP 주소가 아닌 퍼블릭 IP 주소는 유지되지 않는다.**

## 제한 사항

1. **인스턴스 RAM 크기는 100GB 미만이어야 한다.**
2. **EBS 루트 볼륨 크기는 최대 절전 모드 동안 인스턴스 메모리(RAM)를 저장할 수 있을 만큼 충분히 커야한다.**
3. **지원되는 인스턴스 패밀리**
   * C3 - 이전 세대 인스턴스
   * C4, C5 - 컴퓨팅 집약적 워크로드에 최적화
   * M4 - 컴퓨팅, 메모리 및 네트워크 리소스를 균형 있게 제공하며 다양한 애플리케이션에 적합
   * M5 - 인텔 제온 플래티넘 8175M 프로세서로 구동되는 최신 세대 범용 인스턴스로 이 인스턴스 패밀리는 컴퓨팅, 메모리 및 네트워크 리소스를 균형 있게 제공하며 다양한 애플리케이션에 적합
   * R4 - 메모리를 집중적으로 사용하는 애플리케이션에 최적화되어 있으며 RAM의 GiB당 가격이 R3보다 더 저렴

4. **EBS 볼륨을 사용해야 한다.(EC2 인스턴스 스토어 볼륨은 인스턴스에 연결된 상태로 유지되지만 볼륨의 데이터는 손실)**

5. **에이전트가 설치되어 있지 않거나 기반 운영 체제가 최대 절전 모드를 지원하지 않거나 인스턴스 메모리를 저장하기 충분한 볼륨 공간이 없는 경우에는 최대 절전 모드로의 전환이 실패하고 대신에 Amazon EC2가 인스턴스를 중지한다.**  
   * 지원되는 Linux AMI
   * Amazon Linux 2 2019.08.29 이상
   * Amazon Linux AMI 2017.09.1 이상
   * Ubuntu Xenial 16.04 20171121 이상
      * 이전 버전의 Ubuntu Xenial AMI를 사용하려면 AWS가 조정한 4.4.0-1041 이상의 Ubuntu 커널(linux-aws)이 있어야 한다.



---

## Reference

[Overview of hibernation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-hibernate-overview.html)

[Instance lifecycle](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-lifecycle.html#lifecycle-differences)

[Hibernate interrupted Spot Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/hibernate-spot-instances.html)

[Amazon EC2 인스턴스 유형 - Amazon Web Services](https://aws.amazon.com/ko/ec2/instance-types/)

[Amazon EC2 이전 세대 인스턴스 요금 | AWS](https://aws.amazon.com/ko/ec2/previous-generation/)
