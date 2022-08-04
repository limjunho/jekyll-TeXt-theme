---
title: MySQL 데이터 나눠서 select
tags: MySql
---

## Summry

mysql의 limit 키워드를 사용하면 많은 데이터 중 일부분만을 가져올 수 있다.  
DB에서 값을 일부분만을 가져오면 굳이 서버단인 백엔드에서의 작업이 줄어들고 데이터를 요청하고 가져오는 양이 줄어들기 때문에 성능향상에 도움이 되기도 한다.  

본 문서에서는 limit 키워드를 이용해 데이터 일부분을 select하는 방법을 정리한다.

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## 기본 문법

```sql
SELECT *
  FROM 테이블명
LIMIT {offset}, {가져울 데이터 수(ROW_COUNT)}
```

## offset

**offset은 행의 위치를 말하는 것이며 0부터 시작한다.**  
아래의 쿼리는 어떤 테이블에서 offset이 5인 지점에서 데이터 5개를 조회한다.  

```sql
SELECT *
  FROM 테이블명
LIMIT 5, 5
```

## example

```/api/postlist?page=1``` 1페이지의 게시글 정보를 가져오는 요청을 한다면 offset은 0(첫 번째 데이터부터 가져올 거니까), 가져올 데이터 수는 프론트와 약속한 게시글 개수일 것이다.  

아래의 쿼리는 게시판에서 limit을 이용해 페이지별로 게시글 정보를 가져온다.  

```sql
SELECT *
  FROM postlist
LIMIT (page - 1), 10
```

## Reference
>[게시판 페이징 mysql 5.x limit 를 이용한 게시물 나눠서 가져오기 - 4L1FE](https://lngnat.tistory.com/entry/%EA%B2%8C%EC%8B%9C%ED%8C%90-%ED%8E%98%EC%9D%B4%EC%A7%95-mysql-5x-limit)