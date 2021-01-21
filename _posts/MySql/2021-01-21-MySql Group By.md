---
title: MySql Group By
tags: MySql
---

[함수 (Functions) 글](https://limjunho.github.io/2021/01/21/MySql-%ED%95%A8%EC%88%98.html) SQL 에서 제공하는 함수를 이용해서 통계값을 구하는 방법  
함수를 통해 통계값은 구할 수 있었지만, 한번 SQL 요청을 통해 오직 하나의 결과값만을 얻을 수 있었다.  
COUNT 를 이용해 개수를 세든, AVG 를 통해 평균값을 구하든 결과는 언제나 하나의 값이였다.  

<br/>
사용자 테이블에서 남자가 몇명이고 여자는 몇명인지 구하고 싶다면 WHERE 에 성별 조건을 바꿔가면서 2번 SQL 문을 실행하는 방법도 있겠지만 **SQL에서 제공하는 데이터베이스에서 선택된 데이터를 그룹지어 통계값을 구할 수 있는 기능인 GROUP BY 를 활용하면 문제를 해결할 수 있다.**  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### Group By 

GROUP BY는 데이터를 선택해서 보여줄 때, 어떻게 묶어서 보여줄지 설정하는 기능이며 GROUP BY 를 사용하는 문법은 아래와 같다.  
**GROUP BY를 사용하기 위해서는 반드시 SELECT 문에 함수를 적용**

```bash
mysql> select column1, FUNCTION(column2) FROM table_name GROUP BY column1
```
INSERT, UPDATE, DELETE 와는 관계가 없는 기능으로 위와 같이 SELECT ~ FROM 뒤에 GROUP BY 키워드를 입력하고 이어서 뒤에 그룹으로 묶고 싶은 열의 이름을 적는다.  

**예제1**  
사용자 테이블에서 남자와 여자의 키 평균값을(한번의 SQL 실행으로) 구한다.  

```bash
mysql> select gender, AVG(height) FROM users GROUP BY gender;
```

|gender|AVG(height)|
|:------:|:---|
|male|176.5|
|female|163|

대략 위와같은 결과가 출력될 것이다.  

**예제2(WHERE)**  
사용자 테이블에서 나이가 **20살 이상**인 사용자 중 남자와 여자의 평균 키를 구한다.  

```bash
mysql> select gender, AVG(height) FROM users WHERE age >= 20 GROUP BY gender;
```

**예제3(ORDER BY)**  
사용자 테이블에서 나이가 20살 이상인 사용자 중 남자와 여자의 평균 키를 구하는데 이를 성별 이름의 오름차순으로 구한다.  

```bash
mysql> select gender, AVG(height) FROM users WHERE age >= 20 GROUP BY gender ORDER BY gender ASC;
```

ORDER BY 역시 GROUP BY와 함께 사용할 수 있다. ORDER BY는 결과를 구한 후 어떤 순서로 배열할지를 결정하기 때문에 GROUP BY보다 더 뒤에 ORDER BY 키워드가 위치한다.  

**예제4(LIMIT)**  
사용자 테이블에서 남자를 대상으로 각 연령별 평균 키를 구하는데 나이가 제일많은 5개 그룹만 구한다.  

```bash
mysql> select gender, AVG(height) FROM users WHERE gender = 'male' GROUP BY age ORDER BY age DESC LIMIT 5 ;
```