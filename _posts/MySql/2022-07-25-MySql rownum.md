---
title: MySQL join 결과에 rownum 붙이기
tags: MySql
---

## Summry

본 문서에서는 mysql에서 rownum을 사용하는 방법을 정리한다.

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

table join을 진행하고 나서 각 row에 number(index)를 붙이기 위해 아래의 방법을 사용한다.  

## SET 구문을 사용하여 초기화

SET 구문을 사용하여 조회하기 전에 rownum을 초기화 하고, 데이터를 조회  
```sql
SET @rownum:=0;

SELECT @rownum:=@rownum+1, column1
FROM 테이블명;
```

* **초기화 구문을 넣지않고 @rownum:=@rownum+1 만으로 데이터를 조회하게 된다면, <span style = "color : red">조회할 때마다 rownum값이 매번 바뀌게 된다.</span>**
## 

## WHERE절에서 초기화

WHERE 절에 rownum을 초기화하는 조건을 붙여서 조회

```sql
SELECT @rownum:=@rownum+1
FROM 테이블명
WHERE (@rownum:=0)=0;
```

## join + rownum 사용 예제

**table1**  
|table1_index|column1|
|------------|-------|
|1|A1|
|2|A2|

**table2**  
|table2_index|column2|
|------------|-------|
|1|B1|
|2|B2|

### sql

```sql
SELECT @rownum:=@rownum+1 AS idx, a.column1, b.column2
FROM table1 AS a
    JOIN table2 AS b
WHERE (@rownum:=0)=0;
```

**결과**  

* rownum 미사용  

|column1|column2|
|-------|-------|
|A1|B1|
|A1|B2|
|A2|B1|
|A2|B2|

* rownum 사용  

|idx|column1|column2|
|---|-------|-------|
|1|A1|B1|
|2|A1|B2|
|3|A2|B1|
|4|A2|B2|

## Reference
> [[MySQL] rownum을 사용하여 조회된 결과에 번호붙이기 - 양석규](https://sukkyu.tistory.com/67)  
