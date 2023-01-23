---
title: AWS ALB(Application Load Balancer) 세팅 및 SSL 인증서 적용
tags: AWS Springboot
---

## Summry

본 문서에서는 ACM(Amazon Certificate Manager)에서 SSL인증서를 발급받고 ALB(Application Load Balancer)에 적용시켜 https를 구현하는 방법을 정리한다.  
본 문서는 Spring boot back-end 서버를 기준으로 한다.

작성자의 서버는 Let's Encrypt 인증서를 사용하였으나 갱신마다 인증서를 포함해 다시 빌드하여 재배포 해야하는 번거로움이 있었고 인건비 절감과 ALB 구성을 통한 서버 안정성, ALB와 연계되는 AWS의 보안 서비스들을 적용하고자 본 문서의 작업을 진행하였다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## ACM(Amazon Certificate Manager)

**AWS Certificate Manager(ACM)를 사용하면 AWS 서비스 및 연결된 내부 리소스에 사용할 공인 및 사설 SSL/TLS 인증서를 프로비저닝, 관리 및 배포할 수 있다.**  
ACM은 SSL/TLS 인증서를 구매, 업로드 및 갱신하는 데 드는 시간 소모적인 수동 프로세스를 대신 처리해준다.

### 기능 및 비용

* **ACM 통합 서비스를 위한 무료 퍼블릭 인증서**

**AWS Certificate Manager를 사용하면 Elastic Load Balancing 및 API Gateway와 같은 ACM 통합 서비스와 함께 사용하는 퍼블릭 또는 프라이빗 SSL/TLS 인증서를 프로비저닝하는 데 추가 비용이 들지 않는다.** 사용자는 애플리케이션을 실행하기 위해 생성한 AWS 리소스에 대한 비용을 지불한다.  
프라이빗 인증서의 경우 AWS Private CA에는 사용한 서비스와 생성한 인증서 요금을 매월 지불하는 기능이 있다. 프라이빗 인증서를 더 많이 생성하면 인증서당 지급 비용이 줄어든다.

* **관리형 인증서 갱신**

AWS Certificate Manager는 ACM가 관리하고, Elastic Load Balancing 및 API Gateway와 같은 ACM 통합 서비스와 함께 사용되는 인증서의 갱신 프로세스를 관리한다. ACM은 이러한 인증서의 갱신 및 배포를 자동화할 수 있다. AWS Private CA API를 포함하여 ACM을 통해 온프레미스 리소스, EC2 인스턴스 및 IoT 디바이스에 대한 프라이빗 인증서의 생성 및 갱신을 자동화할 수 있다.

* **손쉽게 인증서 받기**

AWS Certificate Manager는 웹 사이트 또는 애플리케이션를 위한 SSL/TLS 인증서를 획득할 때 시간이 소모되고 오류가 발생하기 쉬운 여러 단계를 제거한다. 키 페어 또는 인증서 서명 요청(CSR)을 생성하거나, CSR을 인증 기관에 제출하거나, 인증서를 받은 후 업로드 및 설치할 필요가 없습니다. AWS 관리 콘솔에서 몇 번만 클릭하면 AWS가 발행하는 신뢰할 수 있는 SSL/TLS 인증서를 요청할 수 있다. 인증서가 생성되면 AWS Certificate Manager는 웹 사이트 또는 애플리케이션에 대한 SSL/TLS를 활성화하도록 인증서 배포를 처리한다.

## ACM 인증서 발급

### 1. 인증서 요청

![그림1](/assets/AWS/ACM_ALB/1.png)

### 2. 인증서 유형

![그림2](/assets/AWS/ACM_ALB/2.png)

### 3. 도메인 입력 및 검증 설정

* **본인이 구입한 도메인을 입력한다.**  
    * AWS에서 구매하지 않은 도메인도 가능

![그림3](/assets/AWS/ACM_ALB/3.png)

### 4. DNS 검증

* **DNS 검증을 진행하면 검증 대기 중 상태가 발급 상태로 바뀐다.**

![그림4](/assets/AWS/ACM_ALB/4.png)

### 5. R53 레코드 생성(DNS 검증용)

* **검증 대기 중 인증서를 선택하여 R53 레코드 생성을 클릭한다.**  

![그림5](/assets/AWS/ACM_ALB/5.png)

* **자동으로 값을 채워주므로 레코드 생성만 클릭하면 된다.** 

![그림6](/assets/AWS/ACM_ALB/6.png)

### 6. ACM 인증서 발급 완료

* **R53 DNS 레코드 생성 후 보통 30분 내로 인증서 검증이 완료된다.**

![그림7](/assets/AWS/ACM_ALB/7.png)

---

## ALB (Application Load Balancer)

* **ALB는 Application Load Balancer라는 뜻으로 어플리케이션에서 사용되는 여러 대의 서버들의 트래픽을 분산시켜주는 역할을 수행한다.**  

정말 단순한 서비스라고 한다면 한 대의 서버만 scale-up하여 사용해도 문제없이 운영할 수 있지만 사용자의 접속이 많고 그에 따라 트래픽이 증가하게 되면 한 대의 서버를 계속해서 scale-up을 하는 것은 점점 더 많은 비용 발생과, 트래픽 처리를 위해서도 좋지 않은 방향이다. 그렇기 때문에 트래픽 처리를 하나의 서버가 모두 부담하지 않고 여러 대의 서버들을 생성하여 분산시키는 scale-out 방식을 많이 채택한다.  

## 비용

* 2023-01-17 서울 리전을 기준으로 한 가격이며 LCU 세부 정보 등 더 자세한 내용은 [AWS 공식 문서](https://aws.amazon.com/ko/elasticloadbalancing/pricing/)를 참고  

![그림8](/assets/AWS/ACM_ALB/8.png)

## ALB 생성 및 설정

### 1. 로드 밸런서 보안 그룹 생성

* **각자의 환경에 맞게 세팅하면 된다.**
    * 작성자는 80, 443 포트에서만 트래픽을 수신하려고 함

![그림9](/assets/AWS/ACM_ALB/9.png)

### 2. EC2 Instance 가용 영역 확인

* **ALB 설정을 위해 EC2의 가용 영역을 미리 확인해둔다.**

![그림10](/assets/AWS/ACM_ALB/10.png)

### 3. 로드 밸런서 생성 및 타입 선택

* **Load Balancer 탭에 접속해 Create load balancer를 클릭한다.**

![그림11](/assets/AWS/ACM_ALB/11.png)

* **여러 종류의 로드 밸런서가 있지만 본 문서에서는 ALB를 생성한다.**

![그림12](/assets/AWS/ACM_ALB/12.png)

### 4. Basic Configuration

* **로드 밸런서 이름을 지정하고 아래와 같이 설정한다.**

![그림13](/assets/AWS/ACM_ALB/13.png)

### 5. Network Mapping

* **2단계에서 확인했던 가용영역을 포함해 최소 2개 지정한다.**

![그림14](/assets/AWS/ACM_ALB/14.png)

### 6. Security Groups

* **1단계에서 생성한 보안 그룹을 지정한다.**

![그림15](/assets/AWS/ACM_ALB/15.png)

### 7. Listener and Routing

* **먼저 target group을 생성한다.**

![그림16](/assets/AWS/ACM_ALB/16.png)

* **Target group Basic Configuration**
    * EC2 Instance가 타겟이므로 Instance를 선택

![그림17](/assets/AWS/ACM_ALB/17.png)

* **Health Checks**
    * health check는 로드 밸런서가 인스턴스에 주기적으로 ping을 보내, 정상 상태인지 확인하는 과정이다. 로드밸런서는 인스턴스가 설정한 횟수 이상 미응답(timeout)한 경우 해당 인스턴스는 로드 밸런싱 대상에서 제외한다. 장애가 발생했던 인스턴스가 정상화되어 설정한 횟수 이상 응답(200)한 경우 다시 로드 밸런싱 대상에 추가한다.
    * **본 문서에서는 Spring boot back-end가 EC2에 프로비저닝된 것을 기준으로 하기 때문에 health-check api의 경로를 넣어준다.**  

![그림18](/assets/AWS/ACM_ALB/18.png)

* **Register target**
    * spring boot server의 포트를 지정하여 target을 등록한다.

![그림19](/assets/AWS/ACM_ALB/19.png)

* **target으로 트래픽을 forward할 리스너 등록**
    * 생성한 타겟 그룹을 지정해서 http와 https 리스너를 등록한다.

![그림20](/assets/AWS/ACM_ALB/20.png)

* **ACM SSL 인증서 등록**
    * ACM에서 발급받은 인증서를 지정한다.

![그림21](/assets/AWS/ACM_ALB/21.png)

### 8. Redirect

* **1~7의 단계를 완료하였으면 로드 밸런서를 생성하고 80포트를 443포트로 리다이렉트하도록 설정한다.**
    * https만 사용할 것이기 때문

![그림22](/assets/AWS/ACM_ALB/22.png)

### 9. Route 53 설정

* **R53에 접속해 본인이 등록한 DNS에 A 레코드를 추가한다.**
    * 로드 밸런서 도메인이 아닌 구매한 도메인으로 요청을 보내기 위해 해당 도메인으로 요청이 왔을 때 ALB가 처리하도록 설정
    * 기존 레코드가 있다면 지워준다.(ALB를 통해서만 인스턴스에 트래픽을 보내기 위함)

![그림23](/assets/AWS/ACM_ALB/23.png)

### 10. EC2 보안 그룹 생성

* **EC2에서 사용할 보안규칙 설정**
    * SSH(본인 IP)와 로드 밸런서(dns)를 지정한다.
    * 로드밸런서가 타겟 그룹으로 트래픽을 포워딩 하므로 로드 밸런서만 서버 포트(8888)를 허용한다.

![그림24](/assets/AWS/ACM_ALB/24.png)

---

## Health-Check api

* **ALB 생성 및 설정 7단계 health checks에서 ALB가 spring에 보내는 요청을 처리하기 위한 api**
    * 해당 api를 추가해 배포해야 health checks가 정상적으로 가능하다.

```java
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import lombok.RequiredArgsConstructor;

@RequiredArgsConstructor
@RequestMapping("/aws")
@RestController
public class HealthController {
  @GetMapping(value = "/health-check")
  public ResponseEntity<String> healthCheck() {
    return ResponseEntity.ok().build();
  }
}
```

## 트래픽 경로

* **https로 api 요청**
    * Domain(443) -> ALB(443) -> EC2(8888)
* **http로 api 요청**
    * Domain(80) -> ALB(80) -> ALB(443, redirect) -> EC2(8888)

## Reference

[[AWS] ALB(Application Load Balancer) 사용하기 (Feat, SSL 설정)](https://jforj.tistory.com/278)

[AWS의 Certificate Manager 로 SSL 인증서 발급 받기](https://jojoldu.tistory.com/434)

[How to Use AWS Load Balancer to Deploy Spring Boot Applications](https://enlear.academy/how-to-use-aws-load-balancer-to-deploy-spring-boot-applications-c41877fc8682)

[ACM/SSL 인증서를 로드 밸런서에 연결](https://aws.amazon.com/ko/premiumsupport/knowledge-center/associate-acm-certificate-alb-nlb/)

[네트워크 트래픽 분산-Elastic Load Balancing 요금 -Amazon Web Services](https://aws.amazon.com/ko/elasticloadbalancing/pricing/)

[인증서 관리자 - AWS Certificate Manager - Amazon Web Services](https://aws.amazon.com/ko/certificate-manager/)