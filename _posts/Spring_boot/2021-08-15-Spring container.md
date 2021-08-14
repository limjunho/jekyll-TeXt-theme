---
title: Spring Container
tags: Spring
---

## Summry

본 문서에서는 Spring Container에 대해 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## Spring Container

Container 는 Spring 의 핵심이다. 
* Container 는 개발자를 대신하여, Bean 을 생성 / 관리 / 제거한다. 
    * 때문에 개발자는 모듈 간에 의존 및 결합으로 인해 발생하는 문제로부터 자유롭다.  

아래와 같이 독립적인 코드를 작성해서 Annotaion 만 남겨주면 Container 가 개발자가 원하는 상황에 코드를 실행한다. 따라서 개발자는 메서드가 언제, 어디서 호출되어야 하는지 그리고 메서드를 호출하기 위해 필요한 매개 변수를 준비해서 전달하지 않는다. Container 가 개발자 대신 알아서 호출한다.  

```java
@GetMapping("/greeting")
public Greeting greeting(@RequestParam(value = "name", defaultValue = "World") String name) {
    return new Greeting(counter.incrementAndGet(), String.format(template, name));
}
```

**Container 가 개발자를 대신하여 메서드가 호출될 때와 메서드가 필요한 자원을 전달하는 설계 구조를 Inversion of Control (IOC) 이라 한다.**  
* IOC 는 메서드가 필요로 하는 자원을 코드가 실행되는 타임에 전달하는데, 이를 Dependency Injection (DI) 이라 한다.  
    * 예를 들어 Container가 알아서 greeting 메서드가 필요로 하는 name 매개변수를 전달하는 과정과 run 메서드가 필요로 하는 RestTemplate 매개변수를 전달하는 과정 모두 Dependency Injection (DI) 이다.

만약 Container를 직접 조작하여 Bean 의 생성 / 관리 / 제거에 관여하고 싶다면, Container 의 설정 정보가 담긴 XML 문서를 변경하면 되고, 런타임에 조작하고 싶다면 ApplicationContext 객체를 사용하면 된다.  
* IoC 컨테이너는 주로 Application Cotext를 사용하는데 그 이유는 BeanFactory를 사용해도 되지만 Application Context가 BeanFactory를 상속받고 있으며, 더 다양한 기능을 가지고 있기 때문이다.

* 객체 중에서 Spring IoC Container가 관리하는 객체를 Bean이라고 한다.
* [Spring Bean](https://limjunho.github.io/2021/08/14/Spring-Bean.html) 참고.

## Reference

> [[Spring] 컨테이너 Container - 지람](https://ibocon.tistory.com/122)  
> [Spring Triangle (스프링의 핵심 3요소) - 1. IoC Container - jbb9229](https://velog.io/@jbb9229/springioccontainer)
