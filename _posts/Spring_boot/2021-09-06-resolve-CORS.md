---
title: spring boot cors 처리
tags: Springboot
---

## Summry

본 문서에서는 spring boot에서 cors에러를 해결하는 방법을 정리한다.  

[What is SOP, CORS??](https://limjunho.github.io/2020/11/12/What-is-SOP,-CORS.html)

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## 방법1. Configuration으로 해결

**Configuration file을 만들어 해결하는 방법.**  

**1. config package를 만든다.**  
    * /src/main/java/{project}/config 

**2. WebConfig.java 파일을 생성한다.**  

**3. 아래 코드를 붙여넣는다.**  

```java
package com.thinkingcode.homepage.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedMethods("GET", "PUT", "POST", "DELETE")
                .allowedOrigins("*");
    }
}
```
**위의 코드는 모든 Origins에 대한 모든 HTTP 요청을 허용한다.**  

* **@Configuration Annotation을 통해 설정파일임을 명시**
* **WebMvcConfigurer를 implements**
    * WebMvcConfigurer 인터페이스는 @EnableWebMvc를 통해 자동 설정되는 빈들의 설정자
* **addCorsMappings메소드를 오버라이드**
* **registry.addMapping을 이용해서 CORS를 적용할 URL패턴을 정의할 수 있다.**
* **allowedOrigins**
    * 자원 공유를 허락할 Origin을 지정
    * 한번에 여러 Origin을 설정할 수 있다.
* **allowedMethods**
    * 허용할 HTTP method를 지정
    * 마찬가지로 여러개를 지정할 수 있다.

## 방법2. Controller에서 해결

**Controller 또는 메소드단에서 annotation을 통해 적용하는 방법**  

```java
@RequestMapping("/testPath")
@CrossOrigin(origins = "*", allowedHeaders = "*")
public class TestController {
}
```
* 위처럼 CrossOrigin이라는 어노테이션을 사용하면, 위에서 설정하던것과 같이 허용할 origins이나 methods를 지정할 수 있다.
* origins, methods, maxAge, allowedHeaders를 사용할 수 있다.

```java
@RestController
@RequestMapping("/testPath")
public class TestController {

    @CrossOrigin(origins="*")
    @RequestMapping(value = "/{test}",method = RequestMethod.DELETE)
    public ResponseEntity<String> delete(@PathVariable Long reservationNo) throws Exception{
    }

}
```
* 또한 위처럼 메소드에 적용 할 수 있다.

## Reference

> [[Spring Boot] CORS 설정하기](https://dev-pengun.tistory.com/entry/Spring-Boot-CORS-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)  