---
title: JAVA DAO, DTO, VO
tags: JAVA
---

## Summry  

본 문서에서는 DAO, DTO, VO의 개념과 차이를 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## DAO

**DAO(Data Access Object)는 실질적으로 DB에 접근하는 객체를 말한다.**  
DAO의 사용 이유는 효율적인 커넥션 관리와 보안성 그리고 DataBase 접근을 하기 위한 로직과 비지니스 로직을 분리하기 위해 사용한다.  

DAO의 경우는 DB와 연결할 Connection 까지 설정되어 있는 경우가 많기 때문에 현재 많이 쓰이는 Mybatis 등을 사용할 경우 커넥션풀까지 제공한다. 따라서 DAO를 별도로 만드는 경우는 드물다.  

**DAO Class example**
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class TestDao {

    public void add(TestDto dto) throws ClassNotFoundException, SQLException {
        Class.forName("com.mysql.jdbc.Driver");
        Connection connection = DriverManager.getConnection("jdbc:mysql://localhost/test", "root", "root");

        PreparedStatement preparedStatement = connection.prepareStatement("insert into users(id,name,password) value(?,?,?)");


        preparedStatement.setString(1, dto.getName());
        preparedStatement.setInt(2, dto.getValue());
        preparedStatement.setString(3, dto.getData());
        preparedStatement.executeUpdate();
        preparedStatement.close();
        
        connection.close();

    }
}
```

웹서버는 DB와 연결하기 위해서 매번 커넥션 객체를 생성하는데, 이것을 해결하기 위해 나온것이 커넥션 풀이다. **ConnectionPool 이란 connection 객체를 미리 만들어 놓고 그것을 가져다 쓰는것**이다. 또 다쓰고 난 후에는 반환해 놓는 것. 하지만 유저 한명이 접속해서 한번에 하나의 커넥션만 일으키지 않고 게시판 하나만 봐도 목록볼때 한번, 글읽을때마다 한번, 글쓸때 한번 등등... 엄청나게 많은 커넥션이 일어나기에, 커넥션풀은 커넥션을 또 만드는 오버헤드를 효율적으로 하기 위해 DB에 접속하는 객체를 전용으로 하나만 만들고, 모든 페이지에서 그 객체를 호출해다 사용한다. 이렇게 커넥션을 하나만 가져오고 그 커넥션을 가져온 객체가 모든 DB와의 연결을 하는것이 바로 DAO객체이다.  

**사용이유?**  

사용자는 자신이 필요한 Interface를 DAO에게 던지고 DAO는 이 Interface를 구현한 객체를 사용자에게 편리하게 사용 할 수 있도록 반환해준다.  

DB에 대한 접근을 DAO가 담당하도록 하여 데이터베이스 엑세스를 DAO에서만 하게 되면 다수의 원격호출을 통한 오버헤드를 VO나 DTO를 통해 줄일 수 있고 다수의 DB 호출문제를 해결할 수 있다.  

## DTO

**DTO(Data Transfer Object) 는 계층간 데이터 교환을 위한 자바빈즈를 의미한다.**  
여기서 말하는 계층간의 의미는 Controller, View, Business Layer, Persistent Layer 등을 말하며 각 계층간 데이터 교환을 위한 객체를 의미한다. DTO는 로직을 가지지 않는 순수한 데이터 객체이고 getter, setter 메소드만 가진 클래스를 의미한다.  

**DTO Class example**
```java
public class PersonDTO {

    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

**위의 클래스를 보면 getter/setter 가 존재하는데 여기서 중요한건 Property(프로퍼티) 개념이다.**  
자바는 Property가 문법적으로 제공되지 않는다. 따라서 자바에서 프로퍼티라는 개념을 사용하기 위해 지켜야 할 약속이 있다.  
**setter/getter 에서 set과 get 이후에 나오는 단어가 property라고 약속하는 것인데, 그래서 위 클래스에서 프로퍼티는 name과 age 이다.**  
중요한 것은 프로퍼티가 멤버변수 name, age로 결정되는 것이 아닌 getter/setter에서의 name과 age임을 명심해야 하며 멤버변수는 아무렇게 지어도 영향이 없고 getter/setter 로 프로퍼티(데이터)를 표현한다는 것이다.  

자바는 다양한 프레임워크에서 데이터 자동화처리를 위해 리플렉션 기법 *(리플렉션이란 객체를 통해 클래스의 정보를 분석해 내는 프로그램 기법)* 을 사용하는데, 데이터 자동화 처리에서 제일 중요한 것은 표준규격이다. 예를들어 윗 클래스 DTO에서 property가 name, age라면 name, age의 키값으로 들어온 데이터는 리플렉션 기법으로 setter를 실행시켜 데이터를 넣을 수 있다.  
중요한 것은, 우리가 setter를 요청하는 것이 아닌 프레임워크 내부에서 setter가 실행된다는 점이다.(눈에 보이지않음) 

그래서 layer간(특히 서버 -> View로 이동 등)에 데이터를 넘길때에는 DTO를 쓰면 편하다는 것이 이런이유 때문이다. View에 있는 form에서 name 필드 값을 프로퍼티에 맞춰 넘겼을 때, 받아야 하는 곳에서는 일일히 처리하는 것이 아니라 name속성의 이름이랑 매칭되는 프로퍼티에 자동적으로 DTO가 인스턴스화 되어 PersonDTO를 자료형으로 값을 받을 수 있다. 따라서 key-value 로 존재하는 데이터는 자동화 처리된 DTO로 변환되어 쉽게 데이터가 셋팅된 오브젝트를 받을 수 있다.

**대표적인 DTO로는 폼데이터빈, 데이터베이스 테이블빈 등이 있으며, 각 폼요소나, 데이터베이스 레코드의 데이터를 매핑하기 위한 데이터 객체를 말한다.**  
즉 폼 필드들의 이름을 그대로 가지고 있는 자바빈 객체를 폼 필드와 그대로 매핑하여 비즈니스 계층으로 보낼 때 사용한다. 이런 객체를 DTO라고 부르며 VO(Value Object) 패턴이라고도 한다. VO 패턴은 데이터 전달을 위한 가장 효율적인 방법이지만, 클래스 선언을 위해 많은 코드가 필요하다는 단점이 있다.  

일반적인 DTO는 로직을 갖고 있지 않다. 순수한 데이터 객체이며 속성과 그 속성에 접근하기 위한 getter, setter 메소드만 가진 클래스를 말한다. 여기에 추가적으로 toString(), equals(), 등의 Object 클래스 메소드를 작성할 수 있다.  

## VO

VO(Value Object) 는 DTO와 혼용해서 쓰이긴 하지만 미묘한 차이가 있다.  
**VO는 값 오브젝트로써 값을 위해 쓰인다. 자바는 값 타입을 표현하기 위해 불변 클래스를 만들어서 사용하는데, 불변이라는 것은 readOnly 특징을 가집니다.**  

이러한 클래스는 중간에 값을 바꿀 수 없고 새로 만들어야 한다는 특징이 있다. 예를들어 Color클래스를 보면 Red를 표현하기 위해 Color.RED 등을 사용하며 getter기능만이 존재합니다.  

DTO와 VO의 공통점은 넣어진 데이터를 getter를 통해 사용하므로 주 목적은 같다. 그러나 **DTO의 경우는 가변의 성격을 가진 클래스 입니다(setter 활용). 그에반해 VO는 불변의 성격을 가졌기에 차이점이 있다.** 

## Reference

> [DAO / VO / DTO란? - GENESIS8](https://genesis8.tistory.com/214)  
> [[JAVA] DAO, DTO, VO 차이 - demonic_](https://lemontia.tistory.com/591)  