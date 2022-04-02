---
title: MySQL 조건부 INSERT
tags: MySql
---

## Summry

본 문서에서는 데이터를 삽입할 때 조건비교 후 삽입하는 방법을 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## 데이터가 없으면 INSERT, 있다면 처리하지 않는 방법

```sql
INSERT INTO table_name (column1, column2)  
SELECT 'data1', 'data2'
FROM DUAL WHERE NOT EXISTS(SELECT * FROM table_name WHERE column1 = 'data1');
```

table_name 이라는 테이블에 column1과 column2에 각각 data1, data2를 추가하고 싶은데 table_name 테이블에서 column1의 값이 data1인 데이터가 없을 때 삽입한다.  

## 데이터가 있을 경우 INSERT, 없다면 처리하지 않는 방법

```sql
INSERT INTO table_name (column1, column2)  
SELECT 'data1', 'data2'
FROM DUAL WHERE EXISTS(SELECT * FROM table_name WHERE column1 = 'data1');
```

데이터가 있는 경우를 조건으로 한다면 NOT EXISTS 대신 EXISTS로 입력하면 된다.  
위의 예시는 table_name 이라는 테이블에 column1과 column2에 각각 data1, data2를 추가하고 싶은데 table_name 테이블에서 column1의 값이 data1인 데이터가 있을 때 삽입한다.  

## Reference

> [MySQL :: 데이터 존재 여부 또는 조건에 따라 Insert, update - 투효니](https://blog.naver.com/PostView.nhn?isHttpsRedirect=true&blogId=smilennv&logNo=221474444049)  
> [[mysql] insert구문 조건비교 후 넣기 - dual,exists - 개구리뒷다리](https://frog-hindleg.tistory.com/223)  