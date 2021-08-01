---
title: JAVA Lombok Library
tags: JAVA
---

## Summry  

본 문서에서는 Java의 Lombok Library 를 정리한다.

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## Lombok?

**Lombok 이란 Java 라이브러리로 반복되는 getter, setter, toString .. 등의 반복 메서드 작성 코드를 줄여주는 코드 다이어트 라이브러리이다.**  
Java에서 보통 model 클래스나 Entity 같은 도메인 클래스 등에는 수많은 멤버변수가 있고 이에 대응되는 getter, setter 와 toString() 메서드, 때에따라 멤버변수에 따른 여러개의 생성자를 만들어주게 되는데 거의 대부분 이클립스나 인텔리제이같은 IDE에서 자동생성 기능이 있지만 이 역시도 번거로운 작업이 아닐 수 없다. 뿐만 아니라 코드 자체가 반복 소스코드로 인해 복잡해지게 된다.  

**Lombok은 여러가지 @Annotation을 제공하고 이를 기반으로 반복 소스코드를 컴파일 과정에서 생성해주는 방식으로 동작하는 라이브러리이다.**  
즉, 코딩 과정에서 Lombok과 관련된 어노테이션만 보이고 getter,setter 등의 생략되지만 실제로 컴파일된 결과물 .class 파일에는 코드가 생성되어 있다.  

### 장점

* 어노테이션 기반의 코드 자동생성을 통한 생산성 향상
* 반복코드 다이어트를 통해 가독성 및 유지보수성 향상
* Getter/Setter 외 빌더 패턴이나 로그생성 등 다양한 방면으로 활용 가능

### 단점 및 주의사항

롬복 라이브러리는 개발자마다 호불호가 나뉠 수 있다. 일부 개발자들은 코드가 직접 눈에 보이는 직관성을 유지하는 것이 좋다고 보는 의견도 있는 만큼 자신의 프로젝트나 성향에 따라 사용하면 좋을 것이다.  
또한 API설명과 내부동작을 어느정도 숙지하고 사용해야 한다. **예를 들어 Lombok의 @Data Annotation이나 @toString 어노케이션으로 자동 생성되는 toString()메서드는 순환 참조 또는 무한재귀호출 문제로 인해 StackOverflowError가 발생할 수도 있다.**  

**물론 이 문제를 인지한 Lombok에서 해결할 수 있는 속성을 제공하지만 Lombok이 편리하다는 이유만으로 마구 사용한다면 여러가지 예외문제가 발생할 수 있음을 인지해야 한다.**  

## 일반 Java vs Lombok

**BoardCategory class**
```java
public class BoardCategory {

    private String category_id;

    private String category_name;

    private Date regdate;

    private int board_cnt;

    private int seq;

    public String getCategory_id() {
        return category_id;
    }

    public void setCategory_id(String category_id) {
        this.category_id = category_id;
    }

    public String getCategory_name() {
        return category_name;
    }

    public void setCategory_name(String category_name) {
        this.category_name = category_name;
    }

    public Date getRegdate() {
        return regdate;
    }

    public void setRegdate(Date regdate) {
        this.regdate = regdate;
    }

    public int getBoard_cnt() {
        return board_cnt;
    }

    public void setBoard_cnt(int board_cnt) {
        this.board_cnt = board_cnt;
    }

    public int getSeq() {
        return seq;
    }

    public void setSeq(int seq) {
        this.seq = seq;
    }

    @Override
    public String toString() {
        final StringBuffer sb = new StringBuffer("BoardCategoryBean{");
        sb.append("category_id='").append(category_id).append('\'');
        sb.append(", category='").append(category_name).append('\'');
        sb.append(", regdate=").append(regdate);
        sb.append(", board_cnt=").append(board_cnt);
        sb.append(", seq=").append(seq);
        sb.append('}');
        return sb.toString();
    }
}
```
위 코드를 Lombok을 이용하면 다음과 같이 축소할 수 있다.  
클래스에 @Getter, @ToString 등의 어노테이션을 붙여주기만 하면 된다.  

```java
import lombok.*;

@Getter
@Setter
@ToString
@NoArgsConstructor
@AllArgsConstructor

public class BoardCategoryLombok {

    private String category_id;

    private String category_name;

    private Date regdate;

    private int board_cnt;

    private int seq;

}
```
또는 이 다섯개의 어노테이션을 @Data 어노케이션 하나만 붙여 사용할 수도 있다.  
@Data 어노테이션을 활용하면 @ToString, @EqualsAndHashCode, @Getter, @Setter, @RequiredArgsConstructor를 자동완성 시켜준다.  

```java
import lombok.*;
 
@Data
public class BoardCategoryLombok {

    private String category_id;

    private String category_name;

    private Date regdate;

    private int board_cnt;

    private int seq;

}
```

## Reference

> [[JAVA] LOMBOK 롬복이란? - 코드 다이어트](https://cheershennah.tistory.com/183)  
> [[Java] Lombok이란? 및 Lombok 활용법](https://mangkyu.tistory.com/78)  