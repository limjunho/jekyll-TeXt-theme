---
title: MySQL SELECT 중복 제거
tags: MySql
---

## Summry

본 문서에서는 mysql에서 SELECT문을 실행하다 보면 발생하는 중복 데이터를 제거하는 방법을 정리한다.

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## DISTINCT

### example code

```sql
SELECT DISTINCT age FROM user;
```

## GROUP BY

### example code
```sql
SELECT age FROM user GROUP BY age;
```

## 두 명령 차이

* DISTINCT : 중복을 없애주지만 정렬을 하지는 않는다.
* GROUP BY : 중복을 없애주고 정렬도 해준다.

**GROUP BY 가 하는 일이 더 많기 때문에 속도면에서 불리할 수 있으므로 상황에 맞게 사용하면 될 것이다.**  

## Reference
> [[MySQL] 중복 제거문 DISTINCT, GROUP BY - 야곰야곰's S/W 공부](https://stormpy.tistory.com/82)  