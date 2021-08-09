---
title: Spring boot structure
tags: Spring boot
---

## Summry  

본 문서에서는 Spring boot project의 구조를 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## Project Structure

![그림1](/assets/Spring_boot/project_structure/1.PNG)  

### src/main/java

**스프링 프로젝트와 마찬가지로 클래스, 인터페이스 등 자바 파일이 위치하는 디렉터리이다.**  

### DemoApplication

**Demo 프로젝트에는 우리가 생성하지 않은 DemoApplication class가 포함되어 있다. 파일을 열어보면 메서드 선언부에는 main 메서드 하나만 선언되어 있는데, main 메서드는 SpringApplication.run 메서드를 호출해서 웹 애플리케이션을 실행하는 역할을 한다.**  

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {

	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class, args);
	}

}
```
**DemoApplication.java**

클래스 선언부에 선언된 @SpringBootAplication 어노테이션은 다음 세 가지로 구성되어 있다.  

* **@EnableAutoConfiguration**
    * 스프링 부트는 개발에 필요한 몇 가지 필수적인 설정들의 처리가 되어 있는데, 해당 애너테이션에 의해 다양한 설정들의 일부가 자동으로 완료된다.
* **@ComponentScan**
    * 기존의 XML 설정 방식의 스프링은 빈(Bean)의 등록 및 스캔을 위해 수동으로 ComponentScan을 여러 개 선언하는 방식을 사용하였다. 스프링 부트는 해당 애너테이션에 의해 자동으로 컴포넌트 클래스를 검색하고,
스프링 애플리케이션 콘텍스트(IoC 컨테이너)에 빈(Bean)으로 등록한다. 쉽게 말해, 의존성 주입 과정이 더욱 간편해졌다고 생각할 수 있다.
* **@Configuration**
    * 해당 애너테이션이 선언된 클래스는 자바 기반의 설정 파일로 인식된다. 스프링 4 버전부터 자바 기반의 설정이 가능하게 되었으며, XML 설정에 어마 무시한 시간을 소모하지 않아도 된다. 물론, XML 기반의 설정을 전혀 사용하지 않는 것은 아니다.

### src/main/resources 디렉터리

기존의 스프링은 프로젝트가 생성되면, 해당 디렉터리에는 log4.xml 파일만 떡하니 생성되었다.  
**스프링 부트는 templates 폴더, static 폴더, application.properties 파일이 기본적으로 생성됩니다.**  

* **templates**
    * 기존의 스프링은 HTML 내에 자바 코드를 삽입하는 방식의 JSP를 사용하였다. 디렉터리의 위치도 웹 디렉터리에 해당하는 src/main/webapp 안에 존재하였다. 이러한 방식은 war 파일로 패키지화되었을 경우에만 정적 리소스를 정상적으로 사용할 수 있다. 그러한 이유로 **스프링 부트는 src/main/resources 디렉터리 내에서 화면과 관련된 파일을 관리하는 것으로 생각할 수 있다.** 스프링 부트는 타임리프(Thymeleaf) 템플릿 엔진의 사용을 권장하는데, 타임리프는 JSP와 마찬가지로 HTML 내에서 데이터를 처리하는 데 사용된다.
* **static**
    * 해당 폴더에는 css, fonts, images, plugin, scripts 등의 정적 리소스 파일이 위치한다.
* **application.properties**
    * 해당 파일은 웹 애플리케이션을 실행하면서 자동으로 로딩되는 파일이다. 예를 들어 톰캣(Tomcat)과 같은 WAS(포트 번호, 콘텍스트 패스 등)의 설정이나,데이터베이스 관련 정보 등 각각으로 분리되어 있는 XML 또는 자바 기반의 설정을해당 파일에 Key-Value 형식으로 지정해서 처리할 수 있다.

### src/test/java 디렉터리

해당 디렉터리의 com.example 패키지에는 DemoApplicationTests 클래스가 생성되어 있는데, **해당 클래스를 이용해서 각각의 개발 단계에 알맞은 테스트를 진행할 수 있다.** 기존의 스프링과 달리, 복잡한 설정 없이 곧바로 테스트가 가능하다.

### build.gradle

[spring boot 개발 환경 세팅](https://limjunho.github.io/2021/08/06/VSC-spring-boot.html)에서 프로젝트를 생성 시 프로젝트의 Type을 Gradle로 선택하였다. 기존의 스프링은 pom.xml에 여러 개의 dependency를 추가해서 라이브러리를 관리하는 방식의 Maven을 사용하였는데, 이는 라이브러리의 버전 문제, 충돌 문제, 종속적인 문제 등 메이븐을 사용하면서 골치 아픈 상황이 종종 발생한다고 한다. **이러한 이유로 요즘에는 Gradle을 선호하는 추세**라고 한다.  
Maven은 하나의 라이브러리를 추가하려면 평균적으로 네 줄 이상의 코드를 작성해야 하는데, 그레이들은 단 한 줄의 코드로 라이브러리를 추가할 수 있다.  

## MVC 패턴

스프링 부트라고 해서 다른 개발 패턴을 사용하지 않는다. 기존의 스프링과 마찬가지로 MVC 패턴으로 개발을 진행한다.  

**MVC 패턴?**  
MVC패턴은 디자인패턴 중 하나로, 디자인 패턴이란 프로그램이나 어떤 특정한 것을 개발하는 중에 발생했던 문제점들을 정리해서 상황에 따라 간편하게 적용해서 쓸 수 있는 것을 정리하여 특정한 "규약"을 통해 쉽게 쓸 수 있는 형태로 만든 것을 말한다.  

|패턴|설명|
|---|----|
|모델, Model - (M)|데이터를 처리하는 영역으로, 흔히 비즈니스 로직을 처리하는 영역이라고 이야기한다. 해당 영역은 데이터베이스와 통신하고, 사용자가 원하는 데이터를 가공하는 역할을 한다.|
|뷰, View - (V)|사용자가 보는 화면을 의미하며, HTML과 타임리프를 사용해서 화면을 처리한다. *(뷰 = 화면 = 사용자)*|
|컨트롤러, Controller - (C)|쉽게 이야기하면, 모델 영역과 뷰 영역의 중간 다리 역할을 하는 영역으로 사용자가 웹에서 어떠한 요청을 하면, 가장 먼저 컨트롤러를 경유한다. **컨트롤러는 사용자의 요청을 처리할 어떠한 로직을 호출하고, 호출한 결과를 사용자에게 전달하는 역할을 한다.** 예를 들어 사용자가 게시판에 게시글을 작성하고 등록을 요청하면, 컨트롤러는 게시글의 제목, 내용, 작성자, 등록일 등에 해당하는 파라미터(이하 데이터)를 전달받아 유효성을 검증한다. 검증이 완료되면, 모델 영역에 데이터의 가공을 요청한다. 가공이 완료되면 데이터베이스에 데이터를 저장하고, 저장의 성공 또는 실패 여부를 컨트롤러로 전달한다. 마지막으로 컨트롤러는 등록 요청에 대한 결과를 뷰로 전달한다.|

## Reference

> [[Spring Boot #2] 스프링 부트 프로젝트 구조 (Spring Boot Project Structure) - 새로비](https://engkimbs.tistory.com/750?category=767865)  
> [스프링 부트(Spring Boot) - 프로젝트 구조 알아보기 [개발을 시작해봐요!] - 배고픈 도뎡이](https://congsong.tistory.com/13)  
> [[개발자 면접준비]#1. MVC패턴이란 - 큰돌](https://m.blog.naver.com/jhc9639/220967034588)
