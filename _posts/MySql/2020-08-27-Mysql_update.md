---
title: MySql 수정(UPDATE)
tags: MySql
---

MySql 데이터 수정(UPDATE) 간단 정리글   

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### 데이터 수정  

```bash
mysql> update tablename set coulmn="data" where number >= 4;
```
**number가 4 이상인 레코드 중 coulmn에 해당하는 값을 data로 수정**  

```bash
mysql> update tablename set coulmn1="data1", coulmn2="data2" where number <= 3;
```
**number가 3이하인 레코드 중 coulmn1은 data1로 coulmn2는 data2로 수정**  

```bash
mysql> update tablename set number = number + 5;
```
**모든 레코드의 number값을 +5한 값으로  변경**  
where(조건)이 없는 경우 모든 레코드 수정  

[Python DB에 데이터 삽입 예제](https://limjunho.github.io/2020/08/26/MySql_insert_example.html)  
[Python DB 데이터 읽어오는 예제](https://limjunho.github.io/2020/08/26/MySql_Select_example.html)  
