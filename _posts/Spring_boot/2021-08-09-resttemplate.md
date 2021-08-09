---
title: Spring boot RestTemplate
tags: Springboot
---

## Summry  

본 문서에서는 RestTemplate 정의, 특징 등을 정리한다.  

**Spring 기반 프로젝트를 진행하면 컴포넌트 내부에서 URL을 요청해야하는 경우가 생긴다.** Spring에서는 Http 요청을 간단하게 이용할 수 있도록 **Blocking I/O 기반의 RestTemplate, Non-Blocking I/O 기반의 WebClient** 모듈을 제공하고 있다. 

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## RestTemplate이란?

spring 3.0 부터 지원한다. 스프링에서 제공하는 http 통신에 유용하게 쓸 수 있는 템플릿이며, HTTP 서버와의 통신을 단순화하고 RESTful 원칙을 지킨다. jdbcTemplate 처럼 RestTemplate 도 기계적이고 반복적인 코드들을 깔끔하게 정리해준다.    

**특징**  
* 기계적이고 반복적인 코드를 최대한 줄여줌
* RESTful형식에 맞춤
* json, xml 를 쉽게 응답받음

### RestTemplate 동작

![그림1](/assets/Spring_boot/resttemplate/1.png)  

1. 어플리케이션이 RestTemplate를 생성하고, URI, HTTP메소드 등의 헤더를 담아 요청한다.
2. RestTemplate 는 HttpMessageConverter 를 사용하여 requestEntity 를 요청메세지로 변환한다.
3. RestTemplate 는 ClientHttpRequestFactory 로 부터 ClientHttpRequest 를 가져와서 요청을 보낸다.
4. ClientHttpRequest 는 요청메세지를 만들어 HTTP 프로토콜을 통해 서버와 통신한다.
5. RestTemplate 는 ResponseErrorHandler 로 오류를 확인하고 있다면 처리로직을 태운다.
6. ResponseErrorHandler 는 오류가 있다면 ClientHttpResponse 에서 응답데이터를 가져와서 처리한다.
7. RestTemplate 는 HttpMessageConverter 를 이용해서 응답메세지를 java object(Class responseType) 로 변환한다.
8. 어플리케이션에 반환된다.

## HTTP 서버와의 다양한 통신방법

### URLConnection

jdk 1.2 부터 내장되어 있으며, java.net 패키지에 있다. **URL의 내용을 읽어오거나, URL 주소에 GET, POST로 데이터를 전달 할 때 사용한다.** 또한 http 프로토콜 이외에도 가능하다.(file 등) 보통 아래와 같이 사용한다.  

1. new URL("http:// ....")
2. openConnection()
3. URLConnection
4. getInputStream, getOutputStream
5. InputStream, OutputStream 처리

**문제점**  
* 응답코드가 4xx 거나 5xx 면 IOException 이 터진다.
* 타임아웃을 설정할 수 없다.
* 쿠키 제어가 불가

### HttpClient

3.x일 땐 apache commons 의 프로젝트였다가 승급해서, 아파치 탑 프로젝트가 되었다. 4.x부터는 Apache HttpComponents 로 불린다. maven dependency 를 설정하거나, http://hc.apache.org/downloads.cgi 에서 다운로드할 수 있다. org.apache.http 패키지에 있다.보통 아래처럼 사용한다.  

1. CloseableHttpClient httpclient = HttpClients.createDefault();
2. 메소드에 따라 new HttpGet("http:// ....");
3. CloseableHttpResponse response = httpclient.execute(httpget);
4. HttpEntity entity = response.getEntity();
5. Stream으로 entity.getContent() 처리 등

**URLConnection 와 비교하였을 때 장점**  
* 모든 응답코드를 읽을 수 있다. httpResponse.getStatusLine().getStatusCode()
* 타임아웃 설정 가능
* 쿠키 제어가 가능

**문제점**  
* URLConnection 을 이용한 방식보다 코드가 간결해졌지만, 여전히 반복적이고 코드들이 길다.
* 스트림 처리 로직을 별도로 짜야한다. (EntityUtils 를 쓰면 되는거 같긴하지만)
* 응답의 컨텐츠타입에 따라 별도 로직이 필요하다. (RestTemplate 가 이때 유용!!)


## RestTemplate 사용하기

```java
@SpringBootApplication
public class WebclientApplication {

    public static void main(String[] args) {
        SpringApplication.run(WebclientApplication.class, args);
    }

}
@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello() throws InterruptedException {
        Thread.sleep(5000);
        return "hello";
    }

    @GetMapping("/world")
    public String world() throws InterruptedException {
        Thread.sleep(3000);
        return "world";
    }
}
```
* **/hello, /world 요청이 왔을 때 각각 5초, 3초 후 값을 리턴하는 메서드들을 작성.**  

```java
@Component
public class RestRunner implements ApplicationRunner {

    @Autowired
    RestTemplateBuilder restTemplateBuilder;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        RestTemplate restTemplate = restTemplateBuilder.build();

        StopWatch stopWatch = new StopWatch();
        stopWatch.start();

        String helloResult = restTemplate.getForObject("http://localhost:8080/hello", String.class);
        System.out.println(helloResult);

        String worldResult = restTemplate.getForObject("http://localhost:8080/world", String.class);
        System.out.println(worldResult);

        stopWatch.stop();
        System.out.println(stopWatch.prettyPrint());
    }
}
```
* restTemplate를 통해 스프링 어플리케이션 컴포넌트 내에서 http 요청을 보낸다. **RestTemplate는 Blocking I/O 기반이기 때문에 대략 8초 정도 지나서 모든 요청을 끝마치게 된다.**

### 주요 메소드

|RestTemplate Method|HTTP Method|Description|
|-------------------|-----------|-----------|
|execute|Any||
|exchange|Any|헤더세팅해서 HTTP Method로 요청보내고 ResponseEntity로 반환받음|
|getForObject|GET|get 요청을 보내고 java object로 매핑받아서 반환받음|
|getForEntity|GET|get 요청을 보내고 ResponseEntity로 반환받음|
|postForLocation|POST|post 요청을 보내고 java.net.URI 로 반환받음|
|postForObject|POST|post 요청을 보내고 ResponseEntity로 반환받음|
|put|PUT||
|delete|DELETE||
|headForHeaders|HEAD||
|optionsForAllow|OPTIONS||


### src/main/resources 디렉터리

기존의 스프링은 프로젝트가 생성되면, 해당 디렉터리에는 log4.xml 파일만 생성되었다.  
**스프링 부트는 templates 폴더, static 폴더, application.properties 파일이 기본적으로 생성된다.**  

* **templates**
    * 기존의 스프링은 HTML 내에 자바 코드를 삽입하는 방식의 JSP를 사용하였다. 디렉터리의 위치도 웹 디렉터리에 해당하는 src/main/webapp 안에 존재하였다. 이러한 방식은 war 파일로 패키지화되었을 경우에만 정적 리소스를 정상적으로 사용할 수 있다. 그러한 이유로 **스프링 부트는 src/main/resources 디렉터리 내에서 화면과 관련된 파일을 관리하는 것으로 생각할 수 있다.** 스프링 부트는 타임리프(Thymeleaf) 템플릿 엔진의 사용을 권장하는데, 타임리프는 JSP와 마찬가지로 HTML 내에서 데이터를 처리하는 데 사용된다.
* **static**
    * 해당 폴더에는 css, fonts, images, plugin, scripts 등의 정적 리소스 파일이 위치한다.
* **application.properties**
    * 해당 파일은 웹 애플리케이션을 실행하면서 자동으로 로딩되는 파일이다. 예를 들어 톰캣(Tomcat)과 같은 WAS(포트 번호, 콘텍스트 패스 등)의 설정이나, 데이터베이스 관련 정보 등 각각으로 분리되어 있는 XML 또는 자바 기반의 설정을해당 파일에 Key-Value 형식으로 지정해서 처리할 수 있다.

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

> [RestTemplate (정의, 특징, URLConnection, HttpClient, 동작원리, 사용법, connection pool 적용)](https://sjh836.tistory.com/141)  
> [[Spring Boot #31] 스프링 부트 RestTemplate, WebClient - 새로비](https://engkimbs.tistory.com/808?category=767865)  
> [스프링 RestTemplate 정리(요청 함) - soosungp33](https://velog.io/@soosungp33/%EC%8A%A4%ED%94%84%EB%A7%81-RestTemplate-%EC%A0%95%EB%A6%AC%EC%9A%94%EC%B2%AD-%ED%95%A8)