---
title: Spring boot application.yml vs application.properties
tags: Springboot
---

## Summry

본 문서에서는 yml을 properties와 비교정리한다.  

**yml yaml?**  
최근 스프링이나 쿠버네티스 같은 프로그램의 '설정 파일' 저장 양식으로 많이 쓰이는데, yaml파일의 확장자가 .yml이 된다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## YAML(YML Ain't Markup Language)

* XML, C, Python, Perl, RFC2822에서 정의된 e-mail 양식에서 개념을 얻어 만들어진 '사람이 쉽게 읽을 수 있는' 데이터 직렬화 양식.
* YAML은 모든 데이터를 리스트, 해쉬, 스칼라 데이터의 조합으로 적절히 표현할 수 있다는 믿음을 가지고 만들어졌다.
* YAML은 모든 데이터를 리스트, 해쉬, 스칼라 데이터의 조합으로 적절히 표현할 수 있다는 믿음을 가지고 만들어졌다.
* 문법은 상대적으로 이해하기 쉽고, 가독성이 좋도록 디자인되었으며, 고급 컴퓨터 언어에 적합하다.
* JSON은 yaml의 일종이다.

### 장점

1. **가독성이 좋다**  
  * 계층구조로 표현하여 가독성이 좋다. 또한 불필요한 소스의 중복도 제거 할 수 있다.
  * 들여쓰기, 띄어쓰기로 구분하여 보기 편하다.  

```properties
spring.datasource.driverClassName=net.sf.log4jdbc.sql.jdbcapi.DriverSpy
spring.datasource.url=jdbc:log4jdbc:mariadb://localhost:3306/test?characterEncoding=UTF-8&serverTimezone=UTC
spring.datasource.hikari.username=root
spring.datasource.hikari.password=qlalfqjsgh
spring.datasource.hikari.maximum-pool-size=10
```

```yml
spring:
  datasource:
    driverClassName: net.sf.log4jdbc.sql.jdbcapi.DriverSpy
    url: jdbc:log4jdbc:mariadb://localhost:3306/test?characterEncoding=UTF-8&serverTimezone=UTC
    hikari:
      username: root
      password: qlalfqjsgh
      maximum-pool-size: 10
```

2. **리스트 표현**  
  * 여러 줄에 쓸 때에는 하이픈(-)으로 시작하는 한 줄에 하나의 요소를 표현한다.
  * 한 줄에 모아 쓸 때에는 대괄호([])를 이용하며 쉼표로 각 요소를 구분한다.  

```properties
my.servers[0]=dev.example.com
my.servers[1]=another.example.com
```

```yaml
my:
  servers:
    - dev.example.com
    - another.example.com
```
또는

```yaml
my:
  servers: [dev.example.com, another.example.com]
```

3. **주석**  

```yaml
spring:
  datasource: #db 접속 정보
    driverClassName: net.sf.log4jdbc.sql.jdbcapi.DriverSpy
    url: jdbc:log4jdbc:mariadb://localhost:3306/test?characterEncoding=UTF-8&serverTimezone=UTC
    hikari: #hikari 설정 정보
      username: root
      password: qlalfqjsgh
      maximum-pool-size: 10
```

4. **SpringBoot Profile 적용이 용이하다.**  
 * 한 파일 내에서 여러 파일을 사용하는 것처럼 분리가 가능하다.
 * application.yml 파일 하나로 여러개의 yml을 생성 한것과 같이 처리 가능

### 단점

1. **@PropertySource 어노테이션으로 프로퍼티 값을 불러 올 수 없다.**  
  * 해당 어노테이션을 사용 하려면 프로퍼티 파일을 사용해야된다.  

### 기타

**.properties나 .yml 파일은 Kebob case를 권장 한다.**  

**Kebab case란?**  
* camelCase 와 달리 모두 소문자로 표현하며 단어와 단어 사이를 대시(-)를 이용하여 구분합니다. 스프링의 yml파일이나 url주소에서 사용한다.
  * 변환 전 : My Visitor Count
  * 변환 후 : my-visitor-count


## Reference

> [[스프링부트 (11)] SpringBoot YAML 적용하기(properties vs yaml) - 갓대희](https://goddaehee.tistory.com/213)  
> [Camel case, Kebab case, Snake case 그리고 Pascal case - 양's World](https://yangbox.tistory.com/65)  