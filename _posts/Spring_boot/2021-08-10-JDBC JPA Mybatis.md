---
title: JDBC, JPA, Mybatis
tags: Springboot JAVA
---

## Summry  

**Spring은 DB에 접근하기 위해 자바의 API를 사용한다.** 웹 서비스에 필요한 기능들이 추상화돼서 Spring이 만들어졌듯이, DB에 접근하는 기술들도 일종의 추상화 과정을 거치며 진화해 나갔다. 이 부분을 처음 공부하는 사람 입장에서는, 비슷해 보이면서 다른 단어들 때문에 개념이 굉장히 헷갈린다. JPA를 검색했는데 뜬금없이 Hibernate에 관한 포스팅만 쏟아지기도 한다. 따라서 이해가 쉽도록 **본 문서에서는 영속성, JDBC, JPA, Mybatis등 간단히 주요 단어들과 개념들을 정리한다.**  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## 영속성(Persistence)

* **데이터를 생성한 프로그램이 종료되더라도 사리지지 않는 데이터의 특성을 말한다.**
* 영속성을 갖지 않는 데이터는 단지 메모리에서만 존재하기 때문에 프로그램이 종료되면 모두 잃어버리게 된다. 때문에 파일 시스템, 관계형 데이터베이스 혹은 객체 데이터베이스 등을 활용하여 데이터를 영구적으로 저장하여 영속성을 부여한다.

**Persistence Layer**
* **프로그램의 아키텍처에서 데이터에 영속성을 부여해주는 계층을 말한다.**
* **JDBC를 이용하여 직접 구현할 수 있지만 Persistence Framework를 이용한 개발이 많다.**
* **계층 참고**
    * 프레젠테이션 계층 (Presentation layer) - UI 계층 (UI layer) 이라고도 함
    * 애플리케이션 계층 (Application layer) - 서비스 계층 (Service layer) 이라고도 함
    * 비즈니스 논리 계층 (Business logic layer) - 도메인 계층 (Domain layer) 이라고도 함
    * 데이터 접근 계층 (Data access layer) - 영속 계층 (Persistence layer) 이라고도 함
* **Persistence Framework**
    * JDBC 프로그래밍의 복잡함이나 번거로움 없이 간단한 작업만으로 데이터베이스와 연동되는 시스템을 빠르게 개발할 수 있으며 안정적인 구동을 보장한다.
    * Persistence Framework는 SQL Mapper와 ORM으로 나눌 수 있다.
        * Ex) JPA, Hibernate, Mybatis 등

## SQL Mapper와 ORM

**Persistence Framework는 SQL Mapper와 ORM으로 나눌 수 있다.**  

* ORM은 데이터베이스 객체를 자바 객체로 매핑함으로써 객체 간의 관계를 바탕으로 SQL을 자동으로 생성해주지만 SQL Mapper는 SQL을 명시해줘야 한다.
* ORM은 관계형 데이터베이스의 ‘관계’를 Object에 반영하자는 것이 목적이라면, SQL Mapper는 단순히 필드를 매핑시키는 것이 목적이라는 점에서 지향점의 차이가 있다.

### SQL Mapper

* **SQL문장으로 직접 데이터베이스를 다룬다.**
* SQL Mapper는 SQL을 명시해 줘야 한다.
* MyBatis, jdbcTemplates등

### ORM

* **객체를 통해 간접적으로 데이터베이스를 다룬다.**
* 객체와 관계형 데이터베이스의 데이터를 자동으로 매핑(연결)해주는 것을 말한다.
* Persistence API라고도 할 수 있다.
* Ex) JPA, Hibernate 등

## JDBC

**JDBC는 DB에 접근하고, SQL을 날릴 수 있게 해주는 자바의 표준 API다.** 자바 진영에서 DB에 접근하는 기술들의 근간이 된다. DriverManager를 사용하여 각 드라이버들을 로딩, 해제한다.  
* **모든 Persistence Framework는 내부적으로 JDBC API를 이용한다.**  

```java
// JDBC를 사용한 Java Application(DAO)
public class CarDao {
    public Connection getConnection() {
        Connection con = null;
        String server = "localhost:3306"; // MySQL 서버 주소
        String database = "DB_NAME"; // MySQL DATABASE 이름
        String option = "?useSSL=false&serverTimezone=UTC";
        String userName = "USER_ID"; //  MySQL 서버 아이디
        String password = "USER_PASSWORD"; // MySQL 서버 비밀번호

        // 드라이버 로딩
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
        } catch (ClassNotFoundException e) {
            System.err.println(" !! JDBC Driver load 오류: " + e.getMessage());
        }

        // 드라이버 연결
        try {
            con = DriverManager.getConnection("jdbc:mysql://" + server + "/" + database + option, userName, password);
            System.out.println("정상적으로 연결되었습니다.");
        } catch (SQLException e) {
            System.err.println("연결 오류:" + e.getMessage());
        }
        return con;
    }

    // 드라이버 연결해제
    public void closeConnection(Connection con) {
        try {
            if (con != null)
                con.close();
        } catch (SQLException e) {
            System.err.println("con 오류:" + e.getMessage());
        }
    }

    // CRUD
    public void addCar(Car car) throws SQLException {
        String query = "INSERT INTO car (car_id, car_brand, car_created) VALUES (?, ?, ?)";
        PreparedStatement pstmt = getConnection().prepareStatement(query);
        pstmt.setInt(1, car.getId());
        pstmt.setString(2, car.getBrand());
        pstmt.setString(3, car.getCreated());
        pstmt.executeUpdate();
    }
}
```

## JPA

**JPA는 자바 진영 ORM의 API 표준 명세이다.** ORM을 간단하게 설명하면, 직접적인 SQL 문을 사용하지 않고 자바 코드를 사용해서 DB에 접근, 조작할 수 있는 기술이다. **JPA 역시 내부적으로 JDBC를 사용한다.**  

* JPA는 ORM을 사용하기 위한 표준 인터페이스를 모아둔 것이다.

## Spring JDBC(SQL Mapper -> MyBatis)

**Spring JDBC는 JDBC에서 DriveManager가 하는 일들을 JdbcTemplate에게 맡긴다.** 따라서 개발자는 쿼리문으로 질의할 수 있다. 이 때, JdbcTemplate은 SQL Mapper 중 하나이다(참고로 MyBatis 역시 SQL Mapper 중의 하나다).

```java
public class CarDao {
    private String driver = "com.mysql.cj.jdbc.Driver";
    private String url = "localhost:3306";
    private String userid = "USER_ID";
    private String userpw = "USER_PASSWORD";

    private DriverManagerDataSource dataSource;
    private JdbcTemplate template;

    public CarDao() {
        dataSource = new DriverManagerDataSource();
        dataSource.setDriverClass(driver);
        dataSource.setJdbcUrl(url);
        dataSource.setUser(userid);
        dataSource.setPassword(userpw);

        template = new JdbcTemplate();
        template.setDataSource(dataSource);
    }

    // CRUD
    public int carInsert(Car car) {
        String query = "INSERT INTO car (car_id, car_brand, car_created) VALUES (?, ?, ?)";
        int result = template.update(query, car.getId(), car.getBrand(), car.getCreated());

        return result;
    }
 }
```

## Spring Data JDBC

Spring data는 Spring 진영에서 DB를 쉽게 다루기 위해 시작한 프로젝트이다. 그 중 하나인 **Spring Data JDBC는 기본적인 드라이버 설정 기능부터 CRUD 기능을 제공한다.** [공식 문서](https://spring.io/projects/spring-data-jdbc)를 보면 Spring Data JDBC를 간단하고 선택적인 ORM이라고 소개하고 있다. 선택적 ORM이라는 표현을 사용한 이유는, ORM이 제공하는 기본적인 기능을 제공할 뿐만 아니라, 사용자가 직접 SQL문을 질의하는 기능 역시 제공하기 때문이다(@Query 어노테이션을 사용하면 된다). Data source에 대한 설정은 application.properties 파일에서 가능하다.  

## Hibernate

**Hibernate는 JPA를 구현한 프레임워크이다.** 앞서 JPA는 자바 ORM의 API 표준 명세라는 설명이 있었다. 즉, JPA는 이러이러한 기능이 존재해야 한다라고 정의한 규약이고, 실제로 개발자가 사용하는 것은 그 구현체들이다. 이 구현체 중 하나가 Hibernate다. 그리고 Spring은 기본 JPA vendor로 Hibernate를 사용하고 있다. 이 때문에 Spring JPA 코드 구현에 관한 많은 질문들에서 JPA와 Hibernate란 단어가 혼용된다.

**장점**  
* 객체지향적으로 데이터를 관리할 수 있기 때문에 비즈니스 로직에 집중 할 수 있으며, 객체지향 개발이 가능하다.
* 테이블 생성, 변경, 관리가 쉽다. (JPA를 잘 이해하고 있는 경우)
* 로직을 쿼리에 집중하기 보다는 객체자체에 집중 할 수 있다.
* 빠른 개발이 가능하다.  
**단점**  
* 어렵다. (많은 내용이 감싸져 있기 때문에 알아야 할 것이 많다.)
* 잘 이해하고 사용하지 않으면 데이터 손실이 있을 수 있다. (persistence context)
* 성능상 문제가 있을 수 있다. (이 문제 또한 잘 이해해야 해결이 가능하다.)

## Mybatis

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

## 정리

SQL Mapper와 ORM 중에 어느 것을 사용해야될지 궁금증이 생길 수도 있다. ORM은 SQL 문을 배우지 않고도 사용할 수 있다는 장점이 있다. 하지만 ORM이 생성해주는 SQL 구문을 보고 성능을 예측할 수 없다면, 나중에 끔찍한 일이 벌어질 수도 있을 것이다. 이 부분은 나중에 다른 글로 다뤄보면 좋을 것 같다.


## Reference

> [[JDBC] JDBC, JPA/Hibernate, Mybatis의 차이 - gmlwjd9405](https://gmlwjd9405.github.io/2018/12/25/difference-jdbc-jpa-mybatis.html)  
> [JPA vs JDBC, JPA vs Mybatis, JPA vs Spring Data JPA의 차이점과 Hibernate - 삽질 저장소](https://skyblue300a.tistory.com/7)  
> [JDBC, JPA, Mybatis 차이점](https://thefif19wlsvy.tistory.com/249)