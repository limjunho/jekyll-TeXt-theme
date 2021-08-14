---
title: Spring Boot 동작 원리
tags: Springboot
---

## Summry

본 문서에서는 Spring Boot의 동작 순서를 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## 기본

### Apache Tomcat

**Spring Boot는 내장 Tomcat을 가지고 있다.**  
웹서버는 자바 코드가 추가된 파일 요청이 오면 응답하지 못하기 때문에 톰캣이 필요하다. 웹서버가 이해하지 못하는 파일에 대한 요청이 오면 제어권을 톰캣에게 넘긴다.  

* 톰캣은 파일안에 있는 모든 자바코드를 컴파일하고 컴파일이 끝나면 컴파일된 데이터를 html 문서로 만들고 웹서버에게 넘겨준다.

### Servlet container

**서블릿 컨테이너는 웹 애플리케이션 서버중에서 HTTP 요청을 받아 처리하는 기초 역할을 맡는다.**  

대부분의 웹 프레임워크가 제공하는 기능은 서블릿 컨테이너 위에서 동작하는 서블릿, 필터, 이벤트 리스너등을 적절하게 구현한 것이다. 따라서 사용자가 웹 프레임워크로 작성한 웹애플리케이션은 결국 서블릿 컨테이너 위에서 동작한다. 현재 많이 사용중인 서블릿컨테이너의 종류로는 아파치 톰켓, 제티등이 있다.  

**Servlet?**  
* Servlet은 서블릿 프로그램을 개발할 때 반드시 구현해야 하는 메서드를 선언하고 있는 인터페이스이다. 이 표준을 구현해야 서블릿 컨테이너가 해당 서블릿을 실행할 수 있다.  
* 자바를 사용하여 웹을 만들기 위해 필요한 기술로 클라이언트의 요청을 처리하고, 그 결과를 반환하는 자바 웹 프로그래밍 기술이다.

동시에 수백,수만명의 요청이 발생할때마다 스레드와 서블릿 객체 생성된다. 이는 정해놓은 스레드 만큼 만들어지고 만들어진 스레드는 재사용된다. 즉, 속도 향상을 기대할 수 있다.

* **서블릿 컨테이너가  파일을 읽어서 구동될 때, ContextLoaderListener가 자동으로 메모리에 생성된다. (Pre-Loading)**
    * ContextLoaderListener 클래스는 ServletContextListener 인터페이스를 구현하고 있으며, ApplicationContext를 생성하는 역할을 수행한다.
    * ContextLoaderListener 클래스는 Servlet의 생명주기를 관리해준다.


### web.xml

**web.xml은 웹 어플리케이션 배포시 어플리케이션의 환경을 설정하는 역할을 한다.** 
쉽게말해 각종 설정을 위한 파일이며 서버가 처음 로딩될 때 읽혀서 해당 환경설정을 Tomcat에 전달한다.  
web.xml의 내용은 다음과 같다.  

1. ServletContext의 초기 파라미터 설정
2. Session의 유효시간 설정
3. Servlet/JSP의 정의 및 매핑
4. Error Pages 처리
5. 리스너/필터 설정
6. 보안

### Spring Container

**Container 는 개발자를 대신하여, Bean 을 생성 / 관리 / 제거하는 역할을 한다.**  
때문에 개발자는 모듈 간에 의존 및 결합으로 인해 발생하는 문제로부터 자유롭다.

[Spring Container](https://limjunho.github.io/2021/08/15/Spring-container.html) 참고.

### DispatcherServlet

DispatcherServlet은 FrontController + RequestDispatcher이다.  
**디스패처 서블릿은 가장 앞단에서 HTTP 프로토콜로 들어오는 모든 요청을 가장 먼저 받아 적합한 컨트롤러에 위임해준다.**  

클라이언트로부터 어떠한 요청이 오면 Tomcat(톰캣)과 같은 서블릿 컨테이너가 요청을 받게 된다. 그리고 이 모든 요청을 먼저 프론트 컨트롤러인 디스패처 서블릿이 받는다.  
그러면 디스패처 서블릿은 공통적인 작업을 먼저 처리한 후에 해당 요청을 처리해야 하는 세부 컨트롤러를 getBean()으로 가져오고, 정해진 메소드를 실행시켜 작업을 위임한다. 예를 들어 예외가 발생하였을 때 일관된 방식으로 처리하는 것도 프론트 컨트롤러인 디스패처 서블릿이 담당하고 있다.

**Front Controller(프론트 컨트롤러)?**  
Front Controller는 주로 서블릿 컨테이너의 제일 앞에서 서버로 들어오는 클라이언트의 모든 요청을 받아서 처리해주는 컨트롤러로써, MVC 구조에서 함께 사용되는 디자인 패턴이다.


## Spring Boot 동작 순서

**동작 순서 정리**  
1. Request
2. DispatcherServlet
3. HandlerMapping
4. business logic(Controller -> Service -> DAO -> DB -> DAO -> Service -> Controller)
5. DispatcherServlet
6. ViewResolver
7. View
8. DispatcherServlet
9. Response  

![그림1](/assets/Spring_boot/machanism/1.png)  
> https://devpad.tistory.com/24

1. **클라이언트가 Request 요청을 하면, DispatcherServlet이 요청을 가로챈다. 이때 DispatcherServlet이 모든 요청을 가로채는 건 아니고 web.xml에 등록된 내용만 가로챈다.**
    * 최초의 web.xml 에서는 \<url-pattern\>이 '/'와 같이 해당 애플리케이션의 모든 URL로 등록돼있기 때문에, 만약 *. do와 같이 특정 URL만 적용하고 싶다면 \<url-pattern\>의 내용을 바꿔주어 범위를 변경하면 된다.
2. **DispatcherServlet이 가로챈 요청을 HandlerMapping에게 보내 해당 요청을 처리할 수 있는 Controller를 찾는다.**
3. **실제 로직 처리 (Controller -> Service -> DAO -> DB -> DAO -> Service -> Controller)**
4. **로직 처리 후 ViewResolver를 통해 view 화면을 찾는다.**
5. **찾은 view 화면을 View에 보내면 이 결과를 다시 DispatcherServlet에 보내고, DispatcherServlet는 최종 클라이언트에게 전송한다.**

## Reference

> [Spring MVC 프로젝트의 기본 구조와 실행 순서 - 개발새발](https://devpad.tistory.com/24)  
> [스프링 부트 동작원리 - nrudev](https://velog.io/@nrudev/3.-%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B6%80%ED%8A%B8-%EB%8F%99%EC%9E%91%EC%9B%90%EB%A6%AC)  
> [[Spring]Dispatcher-Servlet(디스패처 서블릿)이란? - 망나니개발자](https://mangkyu.tistory.com/18)
