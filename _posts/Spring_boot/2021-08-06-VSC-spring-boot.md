---
title: Visual Studio Code - Spring boot 개발 환경 세팅
tags: Spring boot
---

## Summry  

본 문서에서는 VSC(Visual Studio Code)에서 Spring boot 개발 환경을 구성하는 방법을 정리한다.  

많은 사람들이 spring boot project에서 Intellij나 Eclipse를 IDE로 사용하지만 VSC가 익숙하고 또 VSC를 이용해 개발하고자 하는 사람들을 위해 VSCode에서 spring boot 기반 java application을 개발할 수 있는 환경을 구축하는 방법을 정리한다.  

**본 문서는 VSC IDE의 install 방법은 설명하지 않는다.**  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## plugin install

**VSCode 내 확장 마켓플레이스 검색: ```Ctrl + Shift + X``` 를 눌러 VSCode ‘Extension Marketplace’ 를 패널을 띄운다.**  

### Java Extension Pack

![그림1](/assets/Spring_boot/VSC-spring-boot/1.PNG)  

**Java Extension Pack은 Visual Studio Code에서 Java 응용 프로그램을 작성, 테스트 및 디버그하는 데 도움이 될 수 있는 널리 사용되는 확장팩.**  

### Spring Boot Extenstion Pack

![그림2](/assets/Spring_boot/VSC-spring-boot/2.PNG)  

**Spring Boot 애플리케이션 개발 및 배포를 위한 확장팩.**

### Lombok Annotations Support for VS Code

![그림3](/assets/Spring_boot/VSC-spring-boot/3.PNG)  

**Java 라이브러리로 반복되는 getter, setter, toString .. 등의 반복 메서드 작성 코드를 줄여주는 코드 다이어트 라이브러리.**  

[Lombok Library 정리글](https://limjunho.github.io/2021/08/01/JAVA-LOMBOK.html)

### Spring Initializr Java Support

![그림4](/assets/Spring_boot/VSC-spring-boot/4.PNG)  

**Spring Initializr Java Support’ 확장팩으로 VSCode 내에서 Spring Initialzr(https://start.spring.io/) API를 이용하여 스프링 부트 프로젝트를 구성할 수 있다.**

### Spring Boot Dashboard

![그림5](/assets/Spring_boot/VSC-spring-boot/5.PNG)  

**피보탈이 이클립스를 통해 제공하는 대시보드와 유사하게 작동한다. 등록된 스프링 부트 애플리케이션 조회, 실행, 중단, 디브그 및 실행중인 스프링 부트 앱을 브라우저에서 열어볼 수 있다.**

## create project

## Reference

> [\[spring-boot\] Visual Studio Code 에서 스프링 부트 프로젝트 개발하기 - honeymon](http://honeymon.io/tech/2021/01/06/use-vs-code-for-spring-boot.html)  
