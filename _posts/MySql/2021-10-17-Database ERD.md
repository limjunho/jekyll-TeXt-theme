---
title: Database ERD 작성법
tags: MySql
---

## Summry

본 문서에서는 ERD를 작성하는 방법을 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## ERD란?

**ERD(Entity Relationship Diagram)란 개체 속성과 개체 간 관계를 도표로 표현한 것을 의미한다.**  
쉽게말해 데이터들의 관계를 나타낸 도표라고 할 수 있으며 흔히 E-R 다이어그램이라고 부르고 ERD 라고 줄여 부르기도 한다.  

ERD는 관계형 DB에서 주로 널리 사용된다. 엔티티와 속성들을 테이블과 컬럼들로 변환할 수 있는데,    
**테이블들과 관계들을 시각화할 수 있기 때문에 설계 문제점을 파악할 수 있다.**  


## ERD 작성 규칙

![그림1](/assets/mysql/ERD/1.gif)  

ERD를 이해하기 위해서는 부모와 자식의 관계를 알아야 하는데 **A 테이블의 기본키를 B 테이블이 가지고 있다면 A 테이블이 부모 테이블, B 테이블이 자식 테이블을 뜻한다.**  

* 실선은 부모 테이블의 기본키를 자식 테이블이 가지고 있으며 이를 기본키로 사용할 때 사용.
    * 이를 식별 관계라 함
* 점선은 부모 테이블의 기본키를 자식테이블이 가지고 있지만 이를 기본키로 사용하지 않을 때 사용.
    * 이를 비식별 관계라 함

​![그림2](/assets/mysql/ERD/2.PNG)  
**일대 일 (식별 관계)**  
* 상세 주소 테이블은 student_id를 PK로 사용하며 student_id를 통해 student 테이블을 참조한다.  
* **한 명의 학생은 한 개의 상세 주소를 가진다.**  
* 상세 주소는 student_id가 없다면 존재 할 수 없다. (식별 관계)  

​![그림3](/assets/mysql/ERD/3.PNG)  
**일대 다 (비식별 관계)**  
* 학생 한 명은 여러개의 과목을 가질 수 있다.  
* **과목은 반드시 학생에게 소속되어야 한다. (NotNull)**
    * student_id 칼럼에 NotNull 옵션이 있음.
* 한명의 학생에게 여러 과목이 소속 될 수 있다.

​![그림4](/assets/mysql/ERD/4.PNG)  
**일대 다 (비식별 관계)**  
* 학생 한 명은 여러개의 과목을 가질 수 있다.  
* **과목은 학생에게 소속되지 않아도 된다. (Null 허용)**
    * student_id 칼럼에 NotNull 옵션이 없음.
* 한명의 학생에게 소속 된 과목이 없을 수 있다.


## 정리  



## Reference

> [[ Database ] ER 다이어그램 / ERD 기호 및 표기법 - Coding Story](https://mjn5027.tistory.com/43)  
> [[데이터베이스] ERD 표기법 설명 - Bamdule](https://fors.tistory.com/195)  