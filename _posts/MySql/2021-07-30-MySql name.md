---
title: MySql DB, Table, Column name
tags: MySql
---

## Summry  

본 문서에서는 데이터베이스명, 테이블명, 컬럼명은 어떻게 지어야 하는지에 대한 규칙을 간단히 정리한다.  
database naming conventions 즉, 다른사람들이 주로 사용하는 암묵적인 룰을 알아본다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## Nomal

1. SQL 키워드 예약어는 대문자로 작성한다.
2. 테이블명, 컬럼명 등은 소문자로 작성한다.
3. SQL예약어가 아니며 유일한 이름으로 지을 것
4. 빈칸 대신 _(underscore)를 사용할 것
5. 문자로 시작하고 _(underscore)로 끝나지 않도록 할 것
6. _(underscore) 두 번 이상 사용하지 말 것
7. 축양형은 되도록 사용하지 말 것

EX)
```sql
CREATE DATABASE test_db;
USE test_db;
CREATE TABLE test_table (idx INT PRIMARY KEY AUTO_INCREMENT);
```


## Table

1. 접두사는 사용하지 말 것 예를 들어 tbl와 같은
2. 테이블의 컬럼명 중 하나인 것을 테이블 이름으로 사용하지 말것

## Column

1. 항상 단수를 사용할 것
2. 가능하면 테이블 기본키로 id를 사용하지 말 것
3. 고유명사가 아닌 경우 항상 소문자를 사용할 것
4. 테이블의 이름을 컬럼명으로 쓰지 말 것

## 정리

* 예약어(SELECT)는 대문자로 사용
* 테이블명은 복수형보다는 단수형
* 컬럼명은 단수형
* 데이터베이스, 테이블, 컬럼명은 소문자와 _(underscore)만 사용
* 접두사 사용 자제

## Reference

> [MySQL 데이터베이스명, 테이블명, 컬럼명은 어떻게 지어야 할까? - Tap to restart](https://taptorestart.tistory.com/entry/MySQL-%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4%EB%AA%85-%ED%85%8C%EC%9D%B4%EB%B8%94%EB%AA%85-%EC%BB%AC%EB%9F%BC%EB%AA%85%EC%9D%80-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%A7%80%EC%96%B4%EC%95%BC-%ED%95%A0%EA%B9%8C)  
> [SQL Style Guide](https://www.sqlstyle.guide/)