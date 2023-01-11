---
title: Springboot 404 NotFound Exception Handling
tags: Springboot
---

## Summry

본 문서에서는 404 NotFound의 Exception Handling 방법을 정리한다.

[Spring boot Exception 처리](https://limjunho.github.io/2022/08/04/spring-boot-exception.html)

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## 기존 예외 처리 방법

Spring에서는 아래와 같이 ControllerAdvice로 @ExceptionHandler를 사용해 Error들을 Handling 할 수 있는데, <span style = "color : red">404는 다른 예외처리와 다르게 application.properties에 별도의 설정이 필요하다.</sapn>

```java
@RestControllerAdvice
@Slf4j
public class VssExceptionHandler {
  // 400
  @ExceptionHandler(value = MissingServletRequestParameterException.class)
  public ResponseEntity<Map<String, String>> missingServletRequestParameterException(final MissingServletRequestParameterException  e, HttpServletRequest req) {
      HttpHeaders responseHeaders = new HttpHeaders();
      HttpStatus httpStatus = HttpStatus.BAD_REQUEST;

      log.error("400 error, missingServletRequestParameterException")

      Map<String, String> map = new HashMap<>();
      map.put("error type", httpStatus.getReasonPhrase());
      map.put("code", "400");
      map.put("message", "잘못된 요청, 필수 파라미터가 누락되었습니다.");

      return new ResponseEntity<>(map, responseHeaders, httpStatus);
  }
}
```

## application.properties 설정

```properties
spring.mvc.throw-exception-if-no-handler-found=true

// 이걸 쓰면 include File을 적용한 파일에 css가 오류가 생김
// 하지만 참고 포스팅에선 이것도 설정에 넣어줌.
spring.web.resources.add-mappings=false
```

### throw-exception-if-no-handler-found

```java
public class DispatcherServlet {
  protected void doDispatch(...) throws Exception {
      // Determine handler for the current request.
      mappedHandler = getHandler(processedRequest);
      if (mappedHandler == null) {
          noHandlerFound(processedRequest, response);
          return;
      }
  }

  protected void noHandlerFound(HttpServletRequest request, HttpServletResponse response) throws Exception {
    if (this.throwExceptionIfNoHandlerFound) {
      throw new NoHandlerFoundException(request.getMethod(), getRequestUri(request),
        new ServletServerHttpRequest(request).getHeaders());
    }
    else {
      response.sendError(HttpServletResponse.SC_NOT_FOUND);
    }
  }
}
```

DispatcherServlet의 doDispatch 메소드를 확인해보면, mappedHandler가 null인 경우 noHandlerFound 메소드가 실행된다. 메소드 내부를 보면 **throwExceptionIfNoHandlerFound에 값에 따라, Exception을 throw하는 지의 여부가 결정**된다. 기본 설정은 false 이기 때문에 프론트 404의 응답만을 보냈다.

- 이 옵션을 ture로 설정하면 해결 가능

### add-mappings

add-mappings를 true로 설정하면 두 번의 요청(잘못된 URL 요청, '/error' 요청)이 각각 SimpleUrlHandlerMapping, RequestMappingHandlerMapping에 의해 처리되기 때문에 throwExceptionIfNoHandlerFound를 true로 설정하더라도 NoHandlerFoundException예외가 발생하지 않는다.

- 여기서 /error 요청은 존재하지않는 API에 대한 요청을 정적자원에 대한 요청으로 보기 때문에 파일이 없다면 Spring 내부적으로 발생시키는 '/error' URL을 뜻한다.

반면에 add-mappings을 false로 설정하면 스프링에서 기본적으로 제공하는 정적자원요청에 대한 매핑을 사용하지 않기 때문에 잘못된 URL로 요청하더라도 SimpleUrlHandlerMapping가 해당 요청을 받지 않고 정상적으로 NoHandlerFoundException을 발생시킵니다.

## ExceptionHandler 생성

```java
@RestControllerAdvice
@Slf4j
public class VssExceptionHandler {
  // 404
  @ExceptionHandler(NoHandlerFoundException.class)
  protected ResponseEntity<Map<String, String>> noHandlerFoundException(NoHandlerFoundException e, HttpServletRequest req) {
    HttpHeaders responseHeaders = new HttpHeaders();
    HttpStatus httpStatus = HttpStatus.NOT_FOUND;

    log.error("400 error, NoHandlerFoundException")

    Map<String, String> map = new HashMap<>();
    map.put("error type", httpStatus.getReasonPhrase());
    map.put("code", "404");
    map.put("message", "잘못된 요청, 요청한 URI가 존재하지 않습니다.");

    return new ResponseEntity<>(map, responseHeaders, httpStatus);
  }
}
```

## 결과

![그림1](/assets/Spring_boot/404Exception/1.png)

404 Exception을 잘 핸들링 하는것을 볼 수 있다.

## Reference

> [Spring REST API 404 응답 커스텀 - ydh6226](https://velog.io/@ydh6226/Spring-404-NotFound-Response-Custom)  
> [[Spring] 404 Error Custom하기 (@ExceptionHandler사용, NoHandlerFoundException Throw 안될때) - gillog](https://velog.io/@gillog/Spring-404-Error-Custom%ED%95%98%EA%B8%B0-ExceptionHandler%EC%82%AC%EC%9A%A9-NoHandlerFoundException-Throw-%EC%95%88%EB%90%A0%EB%95%8C)  
> [exceptionhandler를 이용한 404페오류페이지 처리 - ggujunhee](https://velog.io/@ggujunhee/exceptionhandler%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-404%ED%8E%98%EC%98%A4%EB%A5%98%ED%8E%98%EC%9D%B4%EC%A7%80-%EC%B2%98%EB%A6%AC)
