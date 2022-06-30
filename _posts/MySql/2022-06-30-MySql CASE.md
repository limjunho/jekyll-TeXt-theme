---
title: MySQL CASE 조건문
tags: MySql
---

## Summry

본 문서에서는 mysql에서 CASE 조건문을 사용하는 방법을 정리한다.

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## CASE

MySQL을 사용하다 보면 Query 구문에서 Switch 혹은 IF문과 같은 조건문을 사용하여 결과값을 내야 하는 경우가 있다.  
이러한 경우를 위해 MySQL에서는 CASE라는 Function(함수)을 제공하고 있으며, **해당 Function은 MySQL 4.0 이상부터 사용이 가능하다.**  

### case 사용법

* CASE 문은 조건을 통과하고 첫 번째 조건이 충족되면 값을 반환한다. 
* 조건에 따라 True(참)이면 읽기를 중지하고 결과를 반환하고, 조건이 True(참)가 아니면 ELSE 절의 값을 반환한다.
* ELSE를 사용하지 않는다면, 조건이 거짓인 경우 NULL을 반환합니다.

```sql
CASE
	WHEN 조건1 THEN 결과값1
	WHEN 조건2 THEN 결과값2
	WHEN 조건N THEN 결과값N
	ELSE 결과값
END
```

### example code

```sql
SELECT CASE WHEN age > 20 THEN 1 ELSE 0 END AS isAdult FROM user;
```
위의 코드는 user table에서 age column을 추출하는데, age가 20을 초과하면 1 그렇지 않다면 0을 반환하여 column name은 isAdult로 한다.
									
## Reference
> [[MySQL] CASE 문법 사용 하기 - IT School by :: Teacher](https://info-lab.tistory.com/305)  