---
title: Mysql SELECT 결과 합치기(UNION)
tags: MySql
---

## Summry

본 문서에서는 MySQL에서 복수의 select문을 합치는 방법을 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## UNION

**두개의 SELECT문을 서로 합치고 싶은데 그 중에서도 두개의 조회문의 중복되는 데이터를 한번만 출력할 때 사용한다.**  

* UNION은 합집합을 의미하며 사용하게되면 두개의 SELECT문을 서로 합친 결과를 출력합니다. (중복데이터는 한번만 조회)  

**usertable1**  
|idx|name|age|
|---|----|---|
|1|user1|20|
|2|user2|22|
|3|user3|24|

**usertable2**  
|idx|name|age|
|---|----|---|
|1|user4|21|
|2|user5|23|
|3|user6|25|

위와 같은 데이터를 가진 테이블이 두 개 있을 때 두 테이블의 모든 이름을 조회하고 싶을 경우 아래처럼 쿼리문을 구성한다.  

```mysql
(SELECT name FROM usertable1) --첫번째 SELECT문
UNION
(SELECT name FROM usertable2) --두번째 SELECT문
```

**하나의 테이블에서도 SELECT문을 합칠 수 있다.**  

usertable1에서 나이가 20살인 사람을 조회하고 동시에 가장 마지막 행을 조회하고 싶다면 아래와 같이 쿼리문을 구성한다.  

```mysql
(SELECT * FROM usertable1 WHERE age = 20)
UNION
(SELECT * FROM calendar ORDER BY idx DESC limit 1);
```

### UNION ALL 

**두개의 SELECT문을 서로 합치되, 중복을 무시하고 싶을 때 사용한다.**  

* UNION 대신 UNION ALL 키워드를 사용한다.

```mysql
(SELECT name FROM usertable1) --첫번째 SELECT문
UNION ALL
(SELECT name FROM usertable2) --두번째 SELECT문
```
**example**  



## Reference

> [[Oracle] SELECT문 합치기 유니온(UNION, UNION ALL) 합집합 - 코딩팩토리](https://coding-factory.tistory.com/447)  