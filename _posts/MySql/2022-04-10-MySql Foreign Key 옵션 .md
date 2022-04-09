---
title: MySQL Foreign Key 옵션
tags: MySql
---

## Summry

본 문서에서는 Foreign Key 설정 방법 및 옵션을 정리한다.  

[MySql 외래키(Foreign Key) 개념](https://limjunho.github.io/2021/01/21/MySql-%EC%99%B8%EB%9E%98%ED%82%A4-%EA%B0%9C%EB%85%90.html)

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## FK 사용 필수조건

* **Foreign Key 를 가지는 부모테이블과 자식 테이블은 Engine 이 InnoDB 여야만 한다.**  
* **Foreign Key 로 설정할 두 테이블의 필드는 같은 데이터 형을 가지고 있어야 한다.**  
* **Foreign Key 로  설정이 불가능한 데이터 형은 BLOB,TEXT 형이다.**  
* **Foreign Key 를 설정 할때 Constraint 는 DB 내에서 유일해야한다. (지정하지 않으면 자동으로 지정됨)**  


## FK option

### ON DELETE

* **Cascade** : 부모 데이터 삭제시 자식 데이터도 동시 삭제.
* **Set null** : 부모 데이터 삭제시 해당되는 자식 데이터의 Columm은  Null 로 처리.
* **Restrict** : 자식 테이블에 데이터가 남아 있는 경우 부모 테이블의 데이터는 삭제 불가.
* **No Action** : 아무일도 일어나지 않음.(삭제 연산은 수행하나 제약을 검사하기 때문에 실제로는 SQL 에러 발생)
  * ON DELETE를 설정하지 않으면 기본값

### ON UPDATE

* **Cascade** : 부모 데이터 수정시 자식 데이터도 동시 수정.
* **Set null** : 부모 데이터 수정시 해당되는 자식 데이터의 Columm은 Null 로 처리.
* **Restrict** : 자식 테이블에 데이터가 남아 있는 경우 부모 테이블의 데이터는 수정 불가.
* **No Action** : 아무일도 일어나지 않음.(업데이트 연산은 수행하나 제약을 검사하기 때문에 실제로는 SQL 에러 발생)
  * ON UPDATE를 설정하지 않으면 기본값

## Reference

> [[MySQL] Foreign Key 설정 방법 및 옵션 설명 - 율토리 촌장](https://congi.tistory.com/entry/Foreign-Key-%EC%84%A4%EC%A0%95-%EB%B0%A9%EB%B2%95-%EB%B0%8F-%EC%98%B5%EC%85%98-%EC%84%A4%EB%AA%85)  