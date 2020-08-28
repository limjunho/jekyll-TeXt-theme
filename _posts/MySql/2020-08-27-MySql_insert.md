---
title: MySql 삽입(INSERT)/ 삭제(DELETE)
tags: MySql
---

MySql 데이터 삽입/삭제 간단 정리글   

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### 데이터 삽입  

```bash
mysql> insert into tablename values(데이터타입 값, );
```
**데이터 삽입**  

```bash
mysql> insert ignore into tablename values(데이터타입 값, );
```
**오류를 무시하고 삽입(중복값 삽입은 불가능)**  

### 데이터 삭제  

```bash
mysql> delete from tablename where number <=2;
```
**number가 2 이하인 레코드 삭제**   

```bash
mysql> delete from tablename where data = "test";
```
**data 칼럼의 값이 "test"인 레코드를 삭제**  

```bash
mysql> delete from tablename where number <=2 and data = "test";
```
**number가 2 이하이면서 data가 "test"인 레코드를 삭제**  

```bash
mysql> delete from tablename where number <=2 or data = "test";
```
**number가 2 이하이거나 data가 "test"인 레코드를 삭제**  

```bash
mysql> delete from tablename;
```
**전체 레코드를 삭제**  

[Python DB에 데이터 삽입 예제](https://limjunho.github.io/2020/08/26/MySql_insert_example.html)  
[Python DB 데이터 읽어오는 예제](https://limjunho.github.io/2020/08/26/MySql_Select_example.html)  