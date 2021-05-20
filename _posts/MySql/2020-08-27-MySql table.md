---
title: MySql 테이블 생성/삭제
tags: MySql
---

MySql 테이블 생성 및 삭제 간단 정리글   

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### 테이블 생성  

```bash
mysql> use database;
```
**테이블 만들 데이터베이스 사용**  

```bash
mysql> create table tablename (열이름 데이터타입);
```
**테이블 생성**  

**옵션**  
not null - 해당 열이 NULL이 아닌 값을 가져야만 함  
primary key - 해당 열에 중복되는 값이 없도록 함  
int auto_increment - 데이터를 삽입할 때 마다 레코드에 자동으로 1씩 증가하는 INT형 기본키  

**문자형 데이터타입**  
char(n) - 고정 길이 데이터 타입(최대 255byte)- 지정된 길이보다 짦은 데이터 입력될 시 나머지 공간 공백으로 채워진다.  
VARCHAR(n) - 가변 길이 데이터 타입(최대 65535byte)- 지정된 길이보다 짦은 데이터 입력될 시 나머지 공간은 채우지 않는다.  
TINYTEXT(n) - 문자열 데이터 타입(최대 255byte)  
TEXT(n) - 문자열 데이터 타입(최대 65535byte)  
MEDIUMTEXT(n) - 문자열 데이터 타입(최대 16777215byte)  
LONGTEXT(n) - 문자열 데이터 타입(최대 4294967295byte)  

**숫자형 데이터타입**  
TINYINT(n) - 정수형 데이터 타입(1byte) -128 ~ +127 또는 0 ~ 255수 표현 가능하다.  
SMALLINT(n) - 정수형 데이터 타입(2byte) -32768 ~ 32767 또는 0 ~ 65536수 표현 가능하다.  
MEDIUMINT(n) - 정수형 데이터 타입(3byte) -8388608 ~ +8388607 또는 0 ~ 16777215수 표현 가능하다.  
INT(n) - 정수형 데이터 타입(4byte) -2147483648 ~ +2147483647 또는 0 ~ 4294967295수 표현 가능하다.  
BIGINT(n) - 정수형 데이터 타입(8byte) - 무제한 수 표현 가능하다.  
FLOAT(길이,소수) - 부동 소수형 데이터 타입(4byte) -고정 소수점을 사용 형태이다.  
DECIMAL(길이,소수) - 고정 소수형 데이터 타입고정(길이+1byte) -소수점을 사용 형태이다.  
DOUBLE(길이,소수) - 부동 소수형 데이터 타입(8byte) -DOUBLE을 문자열로 저장한다.  

**날짜형 데이터타입**  
DATE - 날짜(년도, 월, 일) 형태의 기간 표현 데이터 타입(3byte)  
TIME - 시간(시, 분, 초) 형태의 기간 표현 데이터 타입(3byte)  
DATETIME - 날짜와 시간 형태의 기간 표현 데이터 타입(8byte)  
TIMESTAMP - 날짜와 시간 형태의 기간 표현 데이터 타입(4byte) -시스템 변경 시 자동으로 그 날짜와 시간이 저장된다.  
YEAR - 년도 표현 데이터 타입(1byte)  

**이진 데이터타입**  
BINARY(n) & BYTE(n)	- CHAR의 형태의 이진 데이터 타입 (최대 255byte)  
VARBINARY(n) - VARCHAR의 형태의 이진 데이터 타입 (최대 65535byte)  
TINYBLOB(n)	- 이진 데이터 타입 (최대 255byte)  
BLOB(n)	- 이진 데이터 타입 (최대 65535byte)  
MEDIUMBLOB(n) - 이진 데이터 타입 (최대 16777215byte)  
LONGBLOB(n) - 이진 데이터 타입 (최대 4294967295byte)  

### 테이블 삭제  

```bash
mysql> drop table tablename;
```
**테이블 삭제**   

```bash
mysql> drop database databasename;
```
**데이터베이스 삭제**  

### 테이블 조회  

```bash
mysql> show tables;
```
**테이블 조회**  

```bash
mysql> show table status;
```
**테이블 정보 조회**  

```bash
mysql> show full columns from tablename;
```
**테이블 컬럼(열) 조회**  