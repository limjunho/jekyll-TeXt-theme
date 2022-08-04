---
title: Spring boot Exception 처리
tags: JAVA Springboot
---

## Summry

Backend 개발자에게는 클라이언트에게 500Error를 보여주지 않을 의무가 있다. **개발한 API에는 기본적으로 대부분의 상황을 테스트하여 여러 에러들을 올바른 status code와 에러 메시지를 반환하도록 예외처리를 수행해야 한다.**  

본 문서에서 JAVA의 표준 예외를 이용해 Exception을 처리하는 방법을 정리한다.

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## @ControllerAdvice, @RestControllerAdvice

* **@ControllerAdvice는 Spring에서 제공하는 어노테이션이다.**

* **@Controller나 @RestController 에서 발생하는 예외를 한 곳에서 관리하고 처리할 수 있게 하는 어노테이션이다.**
    * 설정을 통해 범위 지정이 가능하며, Default 값으로 모든 Controller에 대해 예외 처리를 관리한다.

* **예외 발생 시 json의 형태를 결과를 반환하기 위해서는 @RestControllerAdvice를 사용하면 됨**

## @ExceptionHandler

* **예외 처리 상황이 발생하면 해당 Handler로 처리하겠다고 명시하는 어노테이션이다.**

* **어노테이션 뒤에 괄호를 붙여 어떤 ExceptionClass를 처리할 지 설정할 수 있다.**
    * @ExceptionHandler(exampleException.class)

* **Exception.class는 최상위 클래스로, 하위 세부 예외 처리 클래스로 설정한 핸들러가 존재하면 그 핸들러가 우선처리하게 되며 처리 되지 못하는 예외 처리에 대해 ExceptionCalss에서 핸들링한다.**
    * @ExceptionHandler(Exception.class) 보다 @ExceptionHandler(NullPointerException.class)가 우선순위가 높다.

* **@ControllerAdivice로 설정된 클래스 내에서 메소드를 정의할 수 있지만 각 Controller 안에도 가능**
    * 각 controller에서 정의한 @ExceptionHandler는 @ControllerAdivice로 설정된 클래스 내에서 정의한 @ExceptionHandler 보다 높은 우선순위를 가진다.

* **전역 설정(@ControllerAdvice)보다 지역 설정(Controller)으로 정의한 Handler가 우선순위를 가진다.**

## 예외 처리하기

### ExceptionHandler 작성

```java
package com.arot.gradle.vss_backend.exception;

import java.nio.file.AccessDeniedException;
import java.util.HashMap;
import java.util.Map;

import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.MissingServletRequestParameterException;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

@RestControllerAdvice
public class exceptionHandler {

    // 400
    @ExceptionHandler(value = MissingServletRequestParameterException.class)
    public ResponseEntity<Map<String, String>> ExceptionHandler(final MissingServletRequestParameterException  e) {
        HttpHeaders responseHeaders = new HttpHeaders();
        HttpStatus httpStatus = HttpStatus.BAD_REQUEST;

        Map<String, String> map = new HashMap<>();
        map.put("error type", httpStatus.getReasonPhrase());
        map.put("code", "400");
        map.put("message", "잘못된 요청, 필수 파라미터가 누락되었습니다.");
        
        return new ResponseEntity<>(map, responseHeaders, httpStatus);
    }

    @ExceptionHandler(value = NumberFormatException.class)
    public ResponseEntity<Map<String, String>> ExceptionHandler(final NumberFormatException  e) {
        HttpHeaders responseHeaders = new HttpHeaders();
        HttpStatus httpStatus = HttpStatus.BAD_REQUEST;

        Map<String, String> map = new HashMap<>();
        map.put("error type", httpStatus.getReasonPhrase());
        map.put("code", "400");
        map.put("message", "잘못된 요청, 숫자 형식 오류입니다.");
        
        return new ResponseEntity<>(map, responseHeaders, httpStatus);
    }

    // 401
    @ExceptionHandler(AccessDeniedException.class)
    protected ResponseEntity<Map<String, String>> handleAccessDeniedException(AccessDeniedException e) {
        HttpHeaders responseHeaders = new HttpHeaders();
        HttpStatus httpStatus = HttpStatus.UNAUTHORIZED;

        Map<String, String> map = new HashMap<>();
        map.put("error type", httpStatus.getReasonPhrase());
        map.put("code", "401");
        map.put("message", "인증 실패, 로그인하지 않은 사용자 또는 권한이 없는 사용자입니다.");
        
        return new ResponseEntity<>(map, responseHeaders, httpStatus);
    }
}
```

json으로 return하기 위해 @RestControllerAdvice를 사용했으며 위와 같은 방식으로 메시지를 예외사항에 맞게 재정의해준다.  
**서버는 잘못된 요청으로 인해 400을 반환하는 경우에도 단순히 에러 메시지로 BadRequest를 반환하는것이 아닌 자세한 에러 사유를 반환하는것이 좋다.** 따라서 JAVA의 표준 예외를 활용해 예외의 발생 원인을 에러메시지로 재정의한다.  

### Controller 내의 ExceptionHandler

```java
@RestController
public class HelloController {

    @RequestMapping("/hello")
    public String hello() {
        return "Hello World!";
    }

    @PostMapping("/exception")
    public void exceptionTest() throws Exception {
        throw new Exception();
    }

    @ExceptionHandler(value = Exception.class)
    public ResponseEntity<Map<String, String>> ExceptionHandler(Exception e) {
        HttpHeaders responseHeaders = new HttpHeaders();
        HttpStatus httpStatus = HttpStatus.BAD_REQUEST;

        Map<String, String> map = new HashMap<>();
        map.put("error type", httpStatus.getReasonPhrase());
        map.put("code", "400");
        map.put("message", "에러 발생");

        return new ResponseEntity<>(map, responseHeaders, httpStatus);
    }
}
```

위처럼 컨트롤러 내에 ExceptionHandler를 작성하는 경우 전역인 설정인 @ControllerAdvise보다 우선순위가 높아 컨트롤러 내에 ExceptionHandler가 Exception을 처리한다.

## spring에서 자동적으로 매핑하는 예외

훌륭하게도 스프링은 자동적으로 15개의 예외를 적당한 상태 코드로 매핑시킨다.

[spring framework handler mapping code](https://github.com/spring-projects/spring-framework/blob/e17fc8d6077aa6085d216e443173869e7d224280/spring-webmvc/src/main/java/org/springframework/web/servlet/mvc/method/annotation/ResponseEntityExceptionHandler.java#L124)

|Exception|HttpStatus|
|--|--|
|HttpRequestMethodNotSupportedException|405 - METHOD_NOT_ALLOWED|
|HttpMediaTypeNotSupportedException|415 - UNSUPPORTED_MEDIA_TYPE|
|HttpMediaTypeNotAcceptableException|406 - NOT_ACCEPTABLE|
|MissingPathVariableException|500 - INTERNAL_SERVER_ERROR|
|MissingServletRequestParameterException|400 - BAD_REQUEST|
|ServletRequestBindingException|400 - BAD_REQUEST|
|ConversionNotSupportedException|500 - INTERNAL_SERVER_ERROR|
|TypeMismatchException|400 - BAD_REQUEST|
|HttpMessageNotReadableException|400 - BAD_REQUEST|
|HttpMessageNotWritableException|500 - INTERNAL_SERVER_ERROR|
|MethodArgumentNotValidException|400 - BAD_REQUEST|
|MissingServletRequestPartException|400 - BAD_REQUEST|
|BindException|400 - BAD_REQUEST|
|NoHandlerFoundException|404 - NOT_FOUND|
|AsyncRequestTimeoutException|503 - SERVICE_UNAVAILABLE|



## Reference

> [스프링 부트에서의 예외 처리 (Exception) | @ControllerAdvice, @ExceptionHandler [스프링 부트 (Spring Boot) - 어라운드허브 스튜디오]](https://www.youtube.com/watch?v=nyN4o9eXqm0&list=LL&index=4&t=562s&ab_channel=%EC%96%B4%EB%9D%BC%EC%9A%B4%EB%93%9C%ED%97%88%EB%B8%8C%EC%8A%A4%ED%8A%9C%EB%94%94%EC%98%A4-AroundHubStudio)  