---
title: MySql 위도 경도 값으로 반경(거리) 구하기
tags: MySql
---

## Summry

본 문서에서는 MySql에서 GPS를 이용한 거리 및 반경을 구하는 방법을 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## SQL
```sql
SELECT *,
  (6371*acos(cos(radians(검색할 Lat))*cos(radians(latitude가 담긴 컬럼명))*cos(radians(longitude가 담긴 컬럼명) 
  -radians(검색할 Lon))+sin(radians(검색할 Lat))*sin(radians(latitude가 담긴 컬럼명)))) 
  AS distance 
  FROM user 
  HAVING distance <= 범위(1Km == 1000) 
  ORDER BY distance 
  LIMIT 0,300
```

검색할 위도, 경도를 기준으로 범위내에 속한 유저들을 모두 찾는 sql문이다.  

## Reference

> [[MySQL] 위도 경도 값으로 반경(거리) 구하기](https://pnot.tistory.com/8)  
