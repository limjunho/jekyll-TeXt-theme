---
title: MySQL query 결과에 다시 query 하기
tags: MySql
---

## Summry

본 문서에서는 query 결과에 다시 query하는 방법을 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## SQL

```sql
SELECT * 
    FROM (SELECT * 
        FROM user 
        WHERE age > 19) AS adultuser 
    WHERE sex = 'male'
```

ex) 성별이 남자인 user들 중에서 나이가 20세 이상인 user를 찾는 query 

## Reference

> [[MySQL] query 결과에서 다시 query하기 - 나긋한 개발자](https://sacstory.tistory.com/entry/MySQL-query%EB%AC%B8-%EA%B2%B0%EA%B3%BC%EC%97%90%EC%84%9C-query%ED%95%98%EA%B8%B0)  
