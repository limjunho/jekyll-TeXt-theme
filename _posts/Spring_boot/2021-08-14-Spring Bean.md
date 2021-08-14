---
title: Spring Bean
tags: Springboot
---

## Summry

본 문서에서는 Spring Bean에 대해 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## Spring bean

** Spring에서는 Spring IoC Container 라는 개념을 사용한다.**  
[Spring Container](https://limjunho.github.io/2021/08/15/Spring-container.html) 참고.

* Container는 인스턴스의 생명주기를 관리하며, 생성된 인스턴스들에게 추가적인 기능을 제공하도록 하는 것이다. 즉 Container는 개발자가 작성한 코드의 처리과정을 위임받은 독립적인 존재라고 생각하면 된다.  

* Container는 적절한 설정만 되어있다면 누구의 도움 없이도 작성한 코드를 스스로 참조한 뒤 알아서 객체의 생성과 소멸을 컨트롤해준다.  
    * Spring Container는 Spring Framework의 핵심부에 위치하며, 종속 객체 주입을 이용하여 Application을 구성하는 Component들을 관리한다.  

**이때 Spring Container에서 생성되는 객체를 Bean이라고 한다.**  

Spring IoC가 관리하는 객체라함은 스프링에 의해 생성되고, 라이프 사이클을 수행하고, 의존성 주입이 일어나는 객체들을 말한다. 즉, 개발자가 관리하는 객체가 아닌 스프링에게 제어권을 넘긴 객체를 스프링에서 Bean이라고 부른다.  

## Spring bean 등록

spring bean을 IoC 컨테이너에 등록하는 방법은 크게 2가지로 나뉜다.

**1. Component Scan을 통한 등록 ( annotation을 이용한 방법 )**
**2. Bean 설정파일에 직접 등록**  

### Component Scanning

**@ComponentScan 어노테이션과 @Component 어노테이션을 사용해서 빈을 등록하도록 하는 방법이다.**  
* @Component 어노테이션이 붙어 있는 클래스, 인터페이스 등은 자동으로 IoC컨테이너에 등록되게 된다.
* @Controller, @Service, @Entity 등도 클래스 파일을 열어보면 내부적으로 @Component 어노테이션을 사용한다.  

**Spring IoC Container가 IoC Container를 만들고 그 안에 Bean을 등록할때 사용하는 Interface들을 Life Cycle Callback이라고 부른다.**  
Life Cycle Callback 중에는 @Component이 붙어있는 모든 Class의 Instance를 생성해 Bean으로 등록하는 작업을 수행하는 Annotation Processor가 등록 되어있다.  

### Bean 설정파일에 직접 등록

빈 설정파일은 XML과 자바 설정파일로 작성할 수 있는데 최근 추세는 자바 설정파일을 좀 더 많이 사용한다.  
* 자바 설정파일은 자바 클래스를 생성해서 작성할 수 있으며 일반적으로 xxxxConfiguration와 같이 명명한다.  
* 그리고 클래스에 @Configuration dj노테이션을 붙인다. 그 안에 @Bean dj노테이션을 사용해 직접 빈을 정의한다.

```java
@Configuration
public class SampleConfiguration {
    @Bean
    public SampleController sampleController() {
        return new SampleController;
    }
}
```
**example**  
sampleController()에서 리턴되는 객체가 IoC 컨테이너 안에 Bean으로 등록된다.  

이렇게 Bean을 직접 정의해서 등록하면 @Component 어노테이션을 붙이지 않아도 된다.  
@Configuration 어노테이션을 보면 이 어노테이션도 @Component를 사용하기 때문에 @ComponentScan의 스캔 대상이 되고 그에 따라 Bean 설정파일이 읽힐때 그 안에 정의한 Bean들이 IoC 컨테이너에 등록되는 것이다.

## JavaBean SpringBean

**먼저 spring bean은 Spring Ioc 컨테이너에 등록되어 관리되고 있는 객체라고 말 할 수 있다.**  
Spring의 Bean은 XML/Annotation에 정의된 설정을 읽고 Spring이 생성하게 된다.


---

**java bean은 jsp에서 주로 사용되는 것으로 자바 코드로 작성된 class이며, 주로 데이터 전달을 위해 사용된다.**  
어떻게 보면 DTO나 VO와 비슷한 역할을 한다고 생각해도 될 것 같다. 또한 하나의 컴포넌트라고도 말 할 수 있다.  
[JavaBean](https://limjunho.github.io/2021/08/12/JAVA-Bean.html) 참고

## Reference

> [[Spring] Bean 정리 - gillog](https://velog.io/@gillog/Spring-Bean-%EC%A0%95%EB%A6%AC)  
> [[Spring] 스프링 빈(Bean)의 개념과 생성 원리 - 버터필드](https://atoz-develop.tistory.com/entry/Spring-%EC%8A%A4%ED%94%84%EB%A7%81-%EB%B9%88Bean%EC%9D%98-%EA%B0%9C%EB%85%90%EA%B3%BC-%EC%83%9D%EC%84%B1-%EC%9B%90%EB%A6%AC)  
> [Java Bean VS Spring Bean - 짱호](https://jjingho.tistory.com/10)  
> [[Spring] Java Bean, Spring Bean - damiano1027](https://velog.io/@damiano1027/Spring-Java-Bean-Spring-Bean)  
> [[스프링] spring bean 과 java bean의 차이점! - ckdgus](https://ckdgus.tistory.com/71)  

