---
title: Spring boot mysql 연동(mybatis)
tags: Springboot JAVA
---

## Summry

본 문서에서는 Spring Boot 프로젝트에서 MyBatis Framework를 이용한 MySQL 연동 방법을 정리한다.

- 본 문서에서 제공하는 예시는 portfolio 테이블의 내용을 get request를 통해 확인해보는 코드이다.
- 본 문서는 MySQL이 설치되어 있다고 가정하고 내용을 기술한다.
- 본 문서에서는 gradle 프로젝트를 예제로 삼는다.

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## Mybatis 개념,

**개발자가 지정한 SQL, 저장 프로시저 그리고 몇 가지 고급 매핑을 지원하는 SQL Mapper이다.**  
* JDBC로 처리하는 상당 부분의 코드와 파라미터 설정 및 결과 매핑을 대신해준다.
    * 기존에 JDBC를 사용할 때는 DB와 관련된 여러 복잡한 설정(Connection)들을 다루어야 했지만 SQL Mapper는 자바 객체를 실제 SQL문에 연결함으로써, 빠른 개발과 편리한 테스트 환경을 제공한다.
* 데이터베이스 record에 원시 타입과 Map 인터페이스 그리고 자바 POJO를 설정해서 매핑하기 위해 xml과 Annotation을 사용할 수 있다.
* MyBatis는 원래 Apache Foundation의 iBatis였으나, 생산성, 개발 프로세스, 커뮤니티 등의 이유로 Google Code로 이전되면서 이름이 바뀌었다.
    * iBatis와 바뀐 차이점은 아래와 같다.
        * JDK 1.5, Annotation
        * Dynatic SQL, XML Element

**장점**  
* SQL에 대한 모든 컨트롤을 하고자 할때 매우 적합하다.
* SQL쿼리들이 매우 잘 최적화되어 있을 때에 유용하다.  

**단점**  
* 애플리케이션과 데이터베이스 간의 설계에 대한 모든 조작을 하고자 할 때는 적합하지 않다.
    * 애플리케이션과 데이터베이스 간에 서로 잘 구조화되도록 많은 설정이 바뀌어야 하기 때문이다.

[JDBC, JPA, Mybatis](https://limjunho.github.io/2021/08/10/JDBC-JPA-Mybatis.html) 등 개념 정리

## Spring boot 기본 개념

Spring boot 에서 중요한 개념인 **Controller, Service, DAO, Mapper에 대해서 정리하고 넘어간다.**

- **Spring boot 는 MVC pattern를 따른다.**
- 아래는 Spring boot에서 DataBase에 접근하는 과정이다.

![그림1](/assets/Spring_boot/mysql/1.PNG)

### Controller

- Controller는 웹 브라우저의 요청을 전담하여 처리한다.
- Controller가 Service를 호출 한다.

### Service

- **Service는 비지니스 로직을 수행 한다.**
  - 유저의 요청이 들어왔을 때, Controller에서 모든 요청을 수용한다. 이때, Controller에서는 때로는 복잡한 작업, 때로는 DB와 통신하는 작업 등 수많은 업무에 대한 요청을 받게 된다. 그 모든 것을 Controller가 홀로 감당하는 형태로 프로그래밍을 하는 경우는 아마도 거의 없을 것이다.
  - 따라서, 보통은 다른 클래스에서 그 일을 대신하게 해 주는데 Service 클래스가 하는 일이 그것이다.
- **데이터베이스에 접근하는 DAO를 이용해서 결과값을 받아 온다.**

### DAO

- DAO는 데이터베이스에 접속하여 비즈니스 로직 실행에 필요한 쿼리를 호출한다.
  [JAVA DAO, DTO, VO](https://limjunho.github.io/2021/07/30/JAVA-DAO-DTO-VO.html) 자세한 설명 참조.

### DTO

- Data Transfer Object의 약자로 각 계층이 데이터를 주고 받을 때 사용하는 객체를 말한다.

### Mapper

- **마이바티스(Mybais)는 Mapper 인터페이스를 제공한다.**
- **DAO대신 mapper를 사용하면 DAO를 만들지 않고 interface만을 이용해서 간편히 개발할 수 있다.**
- **mybatis는 java code와 sql문을 분리하여 편리하게 관리하도록 한다.**
- **SQL문은 \*.xml 형식으로 저장한다.**
- **DB에 질의할 쿼리문을 관리하는 Mapper파일에 요청한다.**

## Spring boot mysql 연동

![그림2](/assets/Spring_boot/mysql/2.PNG)

**본 문서에서 제공하는 예시의 패키지(디렉터리) 구조이다.**

### 의존성 설정

- **build.gradle 파일에 위의 의존성을 작성한다.**
  - spring boot 프로젝트 생성 시 dependency에 mybatis framework를 추가했다면 따로 의존성을 추가할 필요는 없다.
  - 위의 방법이 아닌 경우 dependencies {...}에 `implementation 'org.mybatis.spring.boot:mybatis-spring-boot-starter:2.2.0` 를 추가하고 Refresh Gradle Project를 해준다.

### DB 연결 설정 및 mybatis 설정

**application.priperties 파일을 열고 아래와 같이 설정 정보를 작성한다.**

```properties
# database
spring.datasource.url: jdbc:mysql://localhost:3306/tk_web?characterEncoding=utf8
spring.datasource.username: root
spring.datasource.password: george7790
spring.datasource.driver-class-name: com.mysql.cj.jdbc.Driver

# mybatis
mybatis.config=mybatis-config.xml
mybatis.type-aliases-package: com.example.demo.model
mybatis.mapper-locations: mybatis/mapper/*.xml
```

### DTO 작성

**데이터를 주고 받을때 사용하는 객체인 DTO를 만들어준다.**

```java
package com.example.demo.model.aws.dto;

import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;

@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
public class portfolio {
    private String idx;

    private String start_date;

    private String deadline;

    private String header;

    private String description;

    private String image_path;
}
```

### mapper.xml 작성

**mapper.xml은 DB 쿼리문을 관리하며 DB와 상호작용하는 파일이다.**  
쉽게말해 쿼리문을 모아둔 파일이다.

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.example.demo.model.aws.dao.TestMapper">

  <select id="getAllDataList" resultType="com.example.demo.model.aws.dto.portfolio">
    SELECT * FROM portfolio;
  </select>

</mapper>
```

**TestMappler.xml**

- portfolio 테이블의 모든 정보를 select 하는 간단한 쿼리문을 작성한다.
- mapper 태그의 namespace는 아직은 생성하지 않은 Mapper 인터페이스를 의미한다.

### Mapper Interface 작성

```java
package com.example.demo.model.aws.dao;

import java.util.List;

import com.example.demo.model.aws.dto.portfolio;

import org.apache.ibatis.annotations.Mapper;
import org.springframework.stereotype.Repository;

@Repository
@Mapper
public interface TestMapper {
    List<portfolio> getAllDataList();
}
```

### Service class 작성

**Service는 두가지를 구현해야한다.**

1. service interface
2. service interface를 구현한 class 만들기 (service interface 구현체)

```java
package com.example.demo.service;

import java.util.List;

import com.example.demo.model.aws.dto.portfolio;

public interface TestService {
    public List<portfolio> getAllDataList();
}
```

**TestService.java**

```java
package com.example.demo.service;

import java.util.List;

import com.example.demo.model.aws.dao.TestMapper;
import com.example.demo.model.aws.dto.portfolio;

import org.springframework.stereotype.Service;

import lombok.RequiredArgsConstructor;

@Service
@RequiredArgsConstructor
public class TestServiceImpl implements TestService {
    private final TestMapper testMapper;

    @Override
    public List<portfolio> getAllDataList() {
        return testMapper.getAllDataList();
    }
}
```

**TestServiceImpl.java**

- @Service Annotation을 붙여 이 클래스가 서비스 클래스라는 것을 알려준다.
- **우리는 DAO를 사용하지않고 매퍼를 사용할 것이다.**
- Service가 mapper를 호출하는 부분은 `return testMapper.getAllDataList();`이다.

### Controller 작성

```java
package com.example.demo.controller;

import java.util.List;

import com.example.demo.model.aws.dto.portfolio;
import com.example.demo.service.TestService;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import lombok.RequiredArgsConstructor;

@RequiredArgsConstructor
@RestController
public class TestController {
  private final TestService testService;

  @GetMapping("/hello")
  public String hello() {
    return "Hello World";
  }

  @GetMapping("/test")
  public List<portfolio> test() {

    return testService.getAllDataList();
  }
}
```

**TestController.java**

## Reference

> [[Spring] Spring Boot 시작하기 (4) - Mybatis, MySQL 연동 - \_overload](https://dev-overload.tistory.com/27)  
> [Spring Boot + Gradle + Mybatis + MySQL REST API 서버 만들기 (2) - Day0404](https://day0404.tistory.com/30?category=815939)  
> [Spring boot - Controller, Service, DAO, Mapper - dalpaeng](https://dalpaeng00.tistory.com/83)
