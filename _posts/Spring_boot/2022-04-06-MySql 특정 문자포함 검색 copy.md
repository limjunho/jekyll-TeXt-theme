---
title: Mybatis LIKE 검색
tags: MySql Springboot
---

## Summry

본 문서에서는 Mybatis에서 LIKE를 사용하는 방법을 정리한다.  

[MySql 특정 문자포함 검색](limjunho.github.io/2022/04/06/MySql) 와 같이 사용하면 mybatis에서는 검색결과 없음을 마주하게 될 수 있다.  
따라서 Mybatis에 맞게 sql문을 수정하여야 한다.


[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## 사용법

### 1. MySQL
```sql
title like CONCAT('%',#{keyword},'%')
```

### 2. Oracle
```sql
title like '%' ||  #{keyword} || '%'
```


### 3. MSSQL
```sql
title like '%' + #{keyword} + '%'
```

## Reference

> [MyBatis Like 검색시 처리방법 - 과일가게 개발자](https://fruitdev.tistory.com/60)  