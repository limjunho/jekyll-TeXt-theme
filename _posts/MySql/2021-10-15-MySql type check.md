---
title: Node.js MySql DATETIME 연동
tags: Node.js
---

## Summry

본 문서에서는 DB의 DATETIME값을 출력하거나 렌더링할 때 ```Mon Feb 24 2020 15:15:53 GMT+0900 (GMT+09:00)```같은 모양을 DB 원본 그대로 ```2020-02-24 15:15:53``` 처럼 바꾸는 방법을 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## 1. SHOW COLUMNS

**전체 field 보기**  
```sql
mysql> SHOW COLUMNS FROM `user_info`;
+--------------+--------------+------+-----+---------+----------------+
| Field        | Type         | Null | Key | Default | Extra          |
+--------------+--------------+------+-----+---------+----------------+
| idx          | int(11)      | NO   | PRI | NULL    | auto_increment |
| device_token | varchar(255) | YES  | UNI | NULL    |                |
+--------------+--------------+------+-----+---------+----------------+
2 rows in set (0.004 sec)
```

**특정 field만 보기**  
```sql
mysql> SHOW COLUMNS FROM `user_info` LIKE 'idx';
+-------+---------+------+-----+---------+----------------+
| Field | Type    | Null | Key | Default | Extra          |
+-------+---------+------+-----+---------+----------------+
| idx   | int(11) | NO   | PRI | NULL    | auto_increment |
+-------+---------+------+-----+---------+----------------+
1 row in set (0.004 sec)
```

## 2. desc

```sql
mysql> desc user_info;
+--------------+--------------+------+-----+---------+----------------+
| Field        | Type         | Null | Key | Default | Extra          |
+--------------+--------------+------+-----+---------+----------------+
| idx          | int(11)      | NO   | PRI | NULL    | auto_increment |
| device_token | varchar(255) | YES  | UNI | NULL    |                |
+--------------+--------------+------+-----+---------+----------------+
2 rows in set (0.006 sec)
```

## 3. information_schema

**전체 field 보기**  
```sql
mysql> SELECT DATA_TYPE FROM information_schema.COLUMNS WHERE TABLE_NAME='user_info';
+-----------+
| DATA_TYPE |
+-----------+
| int       |
| varchar   |
+-----------+
2 rows in set (0.005 sec)
```

**특정 field만 보기**  
```sql
mysql> SELECT DATA_TYPE FROM information_schema.COLUMNS WHERE TABLE_NAME='user_info' AND COLUMN_NAME='device_token';
+-----------+
| DATA_TYPE |
+-----------+
| varchar   |
+-----------+
1 row in set (0.005 sec)
```

---

**전체 field 보기**  
```sql
mysql> SELECT COLUMN_TYPE FROM information_schema.COLUMNS WHERE TABLE_NAME='user_info';
+--------------+
| COLUMN_TYPE  |
+--------------+
| int(11)      |
| varchar(255) |
+--------------+
2 rows in set (0.005 sec)
```

**특정 field만 보기**  
```sql
mysql> SELECT COLUMN_TYPE FROM information_schema.COLUMNS WHERE TABLE_NAME='user_info' AND COLUMN_NAME='device_token';
+--------------+
| COLUMN_TYPE  |
+--------------+
| varchar(255) |
+--------------+
1 row in set (0.005 sec)
```

## Reference

> [MySQL 컬럼 자료형 확인 - ZETAWIKI](https://zetawiki.com/wiki/MySQL_%EC%BB%AC%EB%9F%BC_%EC%9E%90%EB%A3%8C%ED%98%95_%ED%99%95%EC%9D%B8)  
> [Mysql 기본 쿼리 명령어 - 자유코딩](https://fors.tistory.com/195)