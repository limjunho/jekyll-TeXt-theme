---
title: Arduino TTGO-LoRa32-V2.0
tags: Arduino
---

## Summry

본 문서에서는 TTGO-LoRa32-V2.0를 사용하여 통신을 하는 방법에 대해 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## LoRa

LoRa는 Long Range의 약자로 로라 장치 개발자 가이드 문서에 따르면, 14km까지 통신이 가능하다고 한다. 또한 LoRa는 BLE처럼 Low Energy로 동작한다.  

### LoRa 특징

**1. Low Energy 저전력**
    * 환경에 따라 다르겠지만 배터리 하나로 10년정도를 버틸 수 있다고 한다.
**2. Long Range 장거리**
    * 10마일 이상이라고 되어 있다.(대략 16km 이상), 개발자 가이드에도 14km까지 소개하고 있다.
**3. 다중센서 가능**
    * 통신을 해주는 단말을 Node라고 표현하는데(센서가 Node가 될 수도 있다), 이 Node에 여러개의 센서를 연결할 수 있다.
**4. 암호화 (보안, AES128)**
    * LoRa에서는 기본적으로 AES128을 따르는 보안기능을 제공한다.

## LoRa 통신 예제

[Github](https://github.com/limjunho/LoRa32_example)  

![그림](/assets/Arduino/LoRa/1.png)  

위의 프로젝트는 수집한 센서값을 LoRa통신으로 인터넷에 연결된 Slave LoRa에 전송하고 Slave LoRa는 Firebase와 상호작용한다.  

* LoRa Setting방법 등이 README에 정리되어 있음.

## Reference

> [[IoT] 로라(LoRa) 통신이란? - IoT 통신 기술(RF) - 오푸스](https://m.blog.naver.com/PostView.nhn?isHttpsRedirect=true&blogId=opusk&logNo=220984482677&proxyReferer=)  
