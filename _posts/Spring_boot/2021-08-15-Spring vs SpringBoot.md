---
title: Spring vs Spring Boot
tags: Springboot
---

## Summry

본 문서에서는 Spring 과 Spring Boot의 차이에 대해 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## Spring

**Spring은 Java 진영에서 가장 대중적인 동적 웹 개발 프레임워크이다.**  

Spring은 아래 세 가지  
* 의존성 주입(DI, Dependency Injection),
* 제어의 역전(IOC, Inversion Of Control),
* 관점 지향 프로그래밍(AOP, Aspect-Oriented Programming)
특성을 이용해 결합도를 낮추는 방식으로 Application을 개발할 수 있다.  

## Spring Boot 

Spring의 환경 설정은 복잡하다.  
Transaction Manager, Hibernate Datasource, Entity Manager, Session Factory ...  
**위 같은 설정에 초기 프로젝트 구축시 많은 어려움이 있다.**  

**Spring Boot는 최소한의 기능으로 Spring MVC를 사용하여 손 쉽게 사용하기 위해 등장했다.**  

* spring-boot-starter Dependency만 추가해주면 바로 API를 정의
* 내장 Tomcat으로 Web Application Server를 실행
* 스프링 홈페이지의 Spring Initializr.를 사용하면 바로 실행 가능한 프로젝트를 생성
* 실행환경이나 의존성 관리 등의 인프라 관련 등은 신경쓸 필요 없이 바로 개발이 가능

**위처럼 Spring의 환경설정이 어려운 사용자들을 위해 다양한 기능을 지원한다.**  

## 차이점

* **Spring Boot 내부에 Tomcat이 포함**
    * 따로 Tomcat을 설치하거나 매번 버전을 관리해 주어야 하는 수고로움을 덜어준다.
* **starter을 통한 dependency 자동화**
    * 과거 Spring framework에서는 각각의 dependency들의 호환되는 버전을 일일이 맞추어 주어야 했고, 때문에 하나의 버전을 올리고자 하면 다른 dependeny에 까지 영향을 미쳐 version관리에 어려움이 많았다. 
    * 하지만, 이제 starter가 대부분의 dependency를 관리해주기 때문에 이러한 걱정을 많이 덜게 되었다.
* **XML설정을 하지 않아도 된다.**
* **jar file을 이용해 자바 옵션만으로 손쉽게 배포가 가능하다.**
    * Spring Actuaor를 이용한 애플리케이션의 모니터링과 관리를 제공한다.

## Reference

> [Spring vs Spring Boot - gillog](https://velog.io/@gillog/Spring-vs-Spring-Boot)  
> [Spring Triangle (스프링의 핵심 3요소) - 1. IoC Container - jbb9229](https://velog.io/@jbb9229/springioccontainer)
