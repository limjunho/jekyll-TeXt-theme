---
title: MySql Database normalization
tags: MySql
---

## Summry  

본 문서에서는 Database normalization(데이터베이스 정규화)를 정리한다.  



[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## 정의와 목적

**Database normalization(데이터베이스 정규화)란 데이터베이스의 설계를 재구성하는 테크닉으로 불필요한 데이터를 없애고 insert, update, delete등의 작업 시 발생할 수 있는 문제들을 해결할 수 있다.**  

정규화의 목적은 주로 두 가지이다.  
1. 데이터 중복(data redundancy)를 제거  
2. 테이블의 구성을 논리적이고 직관적으로 구성  

먼저 정규화를 진행하지 않았을 경우 문제점을 살펴본다.  

|idx|name|address|subject|
|:-:|----|-------|-------|
|1|Adam|Noida|Bio|
|2|Alex|Panipat|Maths|
|3|Stuart|Jammu|Maths|
|4|Adam|Noida|Physics|

위처럼 정규화를 하지 않은 테이블의 경우 데이터 핸들링 시 다양한 문제가 발생할 수 있다.  
*Adam이라는 학생이 두 번 들어가 있음*  

1. Update : Adam의 Address가 변경되었을 때, 여러줄의 데이터를 갱신해야합니다. 이로인해 데이터의 불일치(inconsistency)가 발생할 수 있다.  
2. Insert : 만약 학생이 아무 과목도 수강하지 않는다고 하면, subject 컬럼에는 NULL이 들어갈 것이다.  
3. Delete : 만약 Alex 학생이 과목 수강을 취소한다면 Alex의 레코드가 아예 테이블에서 지워진다.  

**이러한 문제는 테이블의 구성을 논리적으로 변경(정규화)하여 해결한다.**  

*정규화의 법칙(Normalization Rule)은 1차정규화, 2차정규화, 3차정규화, BCNF, 4차정규화, 5차정규화로 나눌 수 있는데, 실무적으로 4차, 5차 정규화까지 하는 경우는 많지 않다고 합다. 따라서 본 문서에서는 BCNF까지만 정리한다.*  

## 1차 정규화

**1차 정규형은 각 로우마다 컬럼의 값이 1개씩만 있어야 한다.**  
이를 컬럼이 원자값(Atomic Value)를 갖는다고 하며 아래와 같은 경우 Adam의 Subject가 Biology와 Maths 두 개 이기 때문에 1차 정규형을 만족하지 못한다.  

|student|age|subject|
|:-:|----|-------|
|Adam|15|Biology, Maths|
|Alex|14|Maths|
|Stuart|17|Maths|

위의 정보를 표현하고 싶은 경우 아래처럼 한 개의 로우를 더 만든다. 결과적으로 1차 정규화를 함으로써 데이터 redundancy는 더 증가하였지만 데이터의 논리적 구성을 위해 이 부분을 희생하는 것으로 볼 수 있다.  

|student|age|subject|
|:-:|----|-------|
|Adam|15|Biology|
|Adam|15|Maths|
|Alex|14|Maths|
|Stuart|17|Maths|

## 2차 정규화

**2차 정규형은 테이블의 모든 컬럼이 완전 함수적 종속을 만족해야 한다.**  
풀어 설명하면 기본키중에 특정 컬럼에만 종속된 컬럼(부분적 종속)이 없어야 한다.  
위 테이블의 경우 기본키는 (Student, Subject) 두 개로 볼 수 있다. 이 두 개가 합쳐져야 한 로우를 구분할 수가 있다. Age의 경우 이 기본키중에 Student에만 종속되어 있기 때문에 Student 컬럼의 값을 알면 Age의 값을 알 수 있다. 따라서 Age가 두 번 들어가는 것은 불필요하다.  

**student_table**  

|student|age|
|:-:|----|
|Adam|15|
|Alex|14|
|Stuart|17|

**subject_table**  

|student|subject|
|:-:|-------|
|Adam|Biology|
|Adam|Maths|
|Alex|Maths|
|Stuart|Maths|

이를 해결하기 위한 방법은 위처럼 테이블을 쪼개는 것이다. 그러면 두 테이블 모두 2차 정규형을 만족하게 되는데 위 테이블의 경우 삽입/갱신/삭제 이상을 겪지 않게된다. 하지만 조금 더 복잡한 테이블의 경우, 갱신 이상을 겪기도하는데 이를 해결하는 것이 바로 3차 정규화이다.  

## 3차 정규화

|idx|name|birthday|street|city|state|ZIP_code|
|:-:|----|--------|------|----|-----|--------|
||||||||

*ZIP code는 ZIP 코드는 미국 우편 공사 (USPS)가 사용하는 우편 번호 제도이다. ZIP은 Zone Improvement Plan(지역 개선 계획)의 약어이며, 공식적으로 대문자로 작성한다.*  

위와 같은 데이터 구성을 예로 들어본다.  
Student_id가 기본키이고, 기본키가 하나이므로 2차 정규형은 만족하는 것으로 볼 수 있다. 하지만 이 데이터의 zip 컬럼을 알면 street, city, state를 결정할 수 있다. 또한 여러명의 학생들이 같은 Zip코드를 갖는 경우에 Zip코드만 알면 Street, City, State가 결정되기 때문이 이 컬럼들에는 중복된 데이터가 생길 가능성이 있다.  
정리하면 3차 정규형은 기본키를 제외한 속성들 간의 이행적 함수 종속이 없는 것으로 풀어서 말하자면, **기본키 이외의 다른 컬럼이 그외 다른 컬럼을 결정할 수 없는 것**이다.

3차 정규화는 2차정규화와 마찬가지로 테이블을 분리함으로써 해결할 수 있는데, 아래처럼 두 개의 테이블로 나눔으로써 3차 정규형을 만족할 수 있다. 이를 통해 데이터가 논리적인 단위(학생, 주소)로 분리될 수 있고, 데이터의 redundancy도 줄었음을 알 수 있다.  

**student_detail_table**  

|idx|name|birthday|ZIP_code|
|:-:|----|--------|--------|
|||||

**address_table**  

|ZIP_code|street|city|state|
|--------|------|----|-----|
|||||

## BCNF

BCNF는 (Boyce and Codd Normal Form) 3차 정규형을 조금 더 강화한 버전으로 볼 수 있다. 이는 3차 정규형으로 해결할 수 없는 이상현상을 해결할 수 있다.  
**BCNF란 3차정규형을 만족하면서 모든 결정자가 후보키 집합에 속한 정규형이다.**  
아래와 같은 경우를 생각해보면, 후보키는 슈퍼키중에서 최소성을 만족하는 건데, 이 경우 (학생, 과목)이다. (학생, 과목)은 그 로우를 유일하게 구분할 수 있다.  
이 테이블의 경우 교수가 결정자다. (교수가 한 과목만 강의할 수 있다고 가정) 즉, 교수가 정해지면 과목이 결정된다. 하지만 교수는 후보키가 아니다. 따라서 이 경우에 BCNF를 만족하지 못한다고 하며 3차 정규형을 만족하면서 BCNF는 만족하지 않는 경우가 된다. 즉, 일반 컬럼이 후보키를 결정하는 경우이다.

|학생|과목|교수|학점|
|:--:|:--:|:--:|:--:|
|1|network|Mr.Kim|A|
|2|data structure|Mr.Lim|A|
|3|data structure|Mr.Lim|A|

위와 같이 테이블이 구성된 경우에 데이터가 중복되고, 갱신 이상이 발생힌다. 예를 들어 Mr.Lim이 강의하는 과목명이 바뀌었다면 두 개의 로우를 갱신해야 하므로 이를 해결하기 위해 마찬가지로 테이블을 분리힌다.

**교수 테이블**  

|교수|과목|
|:-:|:---|
|Mr.Kim|network|
|Mr.Lim|data structure|

**수강 테이블**  

|학생|과목|학점|
|:--:|:--:|:-:|
|1|network|A|
|2|data structure|A|
|3|data structure|A|

## Reference

> [데이터베이스 정규화 1NF, 2NF, 3NF, BCNF - Hard skills](https://3months.tistory.com/193)  
> [[SQL] 1,2,3차 정규화 (feat. ERwin Data Modeler) - 고막고막](https://dog-foot-story.tistory.com/61)  
> [[database] 키의 개념 및 종류 - 슈퍼키, 후보키, 기본키, 대체키, 외래키 - moonibot](https://moonibot.tistory.com/61)  