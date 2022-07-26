---
title: AWS Spot Instance
tags: AWS
---

## Summry

본 문서에서는 Spot Instance는 무엇인가에 대해 정리한다.

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

**AWS 스팟 인스턴스는 사전 약정 없이 On-demand 요금보다 70~90% 절감된 비용으로 사용할 수 있는 EC2 Instance(VM)이다.**  

## 원리

![그림1](/assets/AWS/Spot_instance/1.png)  

**스팟 인스턴스는 사용자 제시 가격(입찰가격)을 정해놓고 저렴할 때 이용할 수 있다.**

최고 가격 > 스팟 가격: 인스턴스 시작  
최고 가격 < 스팟 가격: 인스턴스 종료  

**스팟 인스턴스의 중단 이유는 아래와 같다.**  
* **가격:** 스팟 가격이 최고 가격보다 크다.
* **용량:** 미사용 EC2 인스턴스가 스팟 인스턴스의 수요를 충족할 만큼 충분하지 않으면 Amazon EC2에서 스팟 인스턴스를 중단한다. 인스턴스가 중단되는 순서는 Amazon EC2에서 결정된다.
* **제약 조건:** 요청에 시작 그룹 또는 가용 영역 그룹과 같은 조건이 더 이상 충족할 수 없으면 중단된다.

### 특징

* 사용자가 제시한 가격보다 인스턴스 시장 가격이 높아지게 되면 인스턴스가 종료된다.
* 시장 가격은 인스턴스 패밀리, 인스턴스 크기, 가용 영역(AZ), 리전(Region) 등에 따라 변화하고 또한, 수요와 공급량에 따라 가격이 변화한다.
* 종료 되는 시점을 알 수는 없다.
* 여유 자원에 대한 경매 방식으로 온디맨드 대비 80~90% 저렴하다.

### 장점

* 1~6시간 이내 짧은 워크로드를 다루거나, 갑작스런 피크 타임에 해당하는 컴퓨팅 리소스를 확보해야 할 때 좋다. 즉, 단기적으로 수요가 많을 때 유리하다.
* 주로 Batch Job 등 용도로 사용하는 서버에 잘 어울린다.

## Reference
> [[AWS EC2] Spot Instance(스팟 인스턴스)란? - 우노](https://wooono.tistory.com/86)  
> [Spot Instances - aws](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/using-spot-instances.html)  
> [AWS Spot Instance 이해하기 - leedoing](https://blog.leedoing.com/178)  
