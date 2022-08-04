---
title: Spring boot custom Exception
tags: JAVA Springboot
---

## Summry

Backend 개발자에게는 클라이언트에게 500Error를 보여주지 않을 의무가 있다. **개발한 API에는 기본적으로 대부분의 상황을 테스트하여 여러 에러들을 올바른 status code와 에러 메시지를 반환하도록 예외처리를 수행해야 한다.**  

표준예외로 처리할 수 없는 경우가 있는데 이러한 경우 custom exception을 만들어 처리한다.  
예를 들어 파라미터의 범위를 1~100까지 설정하는 경우 101을 요청해도 int형 파라미터의 범위를 초과하지는 않으므로 예외가 발생하지 않는다. 이 경우 개발자가 임의로 값을 검사하고 1~100 범위 밖의 데이터가 들어온 경우 custom exception 강제로 발생시켜 처리한다.  


본 문서에서 JAVA의 custom exception class를 만들어 Exception을 처리하는 방법을 정리한다.

[Java Checked, Unchecked Exception 정리](https://limjunho.github.io/2022/08/04/JAVA-Exception.html)  
[Spring boot Exception 처리](https://limjunho.github.io/2022/08/04/spring-boot-exception.html)  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## constants 생성

```java
package com.arot.gradle.vss_backend.exception;

public class constants {

  public enum ExceptionClass {

    ParameterRangeException("ParameterRangeException");

    private String exceptionClass;

    ExceptionClass(String exceptionClass) {
      this.exceptionClass = exceptionClass;
    }

    public String getExceptionClass() {
      return exceptionClass;
    }
  }
}
```

enum class를 만들어 예외를 상수로 정의한다.  

## custom exception class 생성

```java
package com.arot.gradle.vss_backend.exception;

import org.springframework.http.HttpStatus;

public class customException extends Exception {

    // 직렬화를 위한 시리얼UID
    private static final long serialVersionUID = 4663380430591151694L;

    private constants.ExceptionClass exceptionClass;
    private HttpStatus httpStatus;

    // 상수로 정의한 예외, HttpStatus, 에러메시지를 입력받는 생성자 오버로딩
    public customException(constants.ExceptionClass exceptionClass, HttpStatus httpStatus,
        String message) {
        super(message);
        this.exceptionClass = exceptionClass;
        this.httpStatus = httpStatus;
    }

    public constants.ExceptionClass getExceptionClass() {
        return exceptionClass;
    }

    public int getHttpStatusCode() {
        return httpStatus.value();
    }

    public String getHttpStatusType() {
        return httpStatus.getReasonPhrase();
    }

    public HttpStatus getHttpStatus() {
        return httpStatus;
    }
}
```

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
    // custom exception
    @ExceptionHandler(value = customException.class)
    public ResponseEntity<Map<String, String>> ExceptionHandler(customException e) {
        HttpHeaders responseHeaders = new HttpHeaders();

        Map<String, String> map = new HashMap<>();
        map.put("error type", e.getHttpStatusType());
        map.put("code",
            Integer.toString(e.getHttpStatusCode()));
        map.put("message", e.getMessage());

        return new ResponseEntity<>(map, responseHeaders, e.getHttpStatus());
    }
}
```

### controller에서 custom exception 발생시키기

```java
@Operation(summary = "test API")
  @GetMapping("/test")
  public List<test_DTO> getTest(@RequestParam Integer test, HttpServletResponse res) throws customException{
    List<test_DTO> result = testService.getTest(test);
    
    // 값 검사 후 custom exception throw
    if(test < 0 || test > 100){
        throw new customException(ExceptionClass.ParameterRangeException, HttpStatus.BAD_REQUEST, "잘못된 요청, 허용된 파라미터의 범위를 초과하였습니다.");
    }

    if (!result.isEmpty()) {
      res.setStatus(StatusCode.OK);
      return result;
    } else {
      res.setStatus(StatusCode.NoContent);
      return null;
    }
  }
```

controller에서 파라미터의 값을 검사한 후 지정한 범위를 초과하면 custom exception이 발생하도록 throw한다.  

```java
@Operation(summary = "test API")
  @GetMapping("/test")
  public List<test_DTO> getTest(@RequestParam Integer test, HttpServletResponse res) throws customException{
    List<test_DTO> result = testService.getTest(test);
    
    try {
      result = testService.getTest(test);
    } catch (Exception e) {
      // Check page range
      if(test < 0 || test > 268435456){
        throw new customException(ExceptionClass.ParameterRangeException, HttpStatus.BAD_REQUEST, "잘못된 요청, 허용된 페이지 범위를 벗어난 요청입니다.");
      }
      return null;
    }

    if (!result.isEmpty()) {
      res.setStatus(StatusCode.OK);
      return result;
    } else {
      res.setStatus(StatusCode.NoContent);
      return null;
    }
  }
```
위 코드는 게시글을 8개씩 불러오는 API이다.  
0~268435456 범위에 없는 경우 DB에서 정상 결과를 가져오지 못하거나 int값 범위 초과로 인해 sqlSyntaxException이 발생한다. 이러한 경우 try/catch를 이용해 exception이 발생하면 파라미터 값 검사를 통해 어떤 exception인지 파악하고 해당하는 customException을 발생시킨다.  

## Reference

> [서비스 특성에 맞춘 예외 처리 방법 Custom Exception [스프링 부트 (Spring Boot)]  - 어라운드허브 스튜디오](https://www.youtube.com/watch?v=5XHhAhN-9po&list=PLlTylS8uB2fBOi6uzvMpojFrNe7sRmlzU&index=20&ab_channel=%EC%96%B4%EB%9D%BC%EC%9A%B4%EB%93%9C%ED%97%88%EB%B8%8C%EC%8A%A4%ED%8A%9C%EB%94%94%EC%98%A4-AroundHubStudio)  