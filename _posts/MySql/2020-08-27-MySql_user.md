---
title: MySql 사용자 생성/삭제
tags: MySql
---

MySql 사용자 생성 및 삭제 간단 정리글   

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### 사용자 생성  

```bash
mysql> use mysql;

mysql> select user, host from user;
```
**user와 host 확인**  

```bash
mysql> create user 'username'@'localhost' identified by 'password';
mysql> create user 'username'@'%' identified by 'password';
```
**내부 접속만 가능한 사용자 - localhost**  
**외부 접속이 가능한 사용자 - %**  

### 사용자 삭제  

```bash
mysql> use mysql;

mysql> select user, host from user;
```
**user와 host 확인**  

```bash
mysql> drop user 'username';
mysql> delete from user where user='username';
```
**둘 중 하나의 방법으로 삭제**  

### 적용

```bash
mysql> flush privileges;
```