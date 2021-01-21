---
title: MySql DataType
tags: MySql
---

MySql DataType 정리글   

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### 자주 사용하는 자료형 

|자료형|설명|
|:------:|:---|
|int|4byte 이며, 정수형이다.|
|varchar|최대 65535byte 이며 가변길이이다. 짧은 데이터 입력시 나머지 공간은 채우지 않는다.|
|date|3byte 이며 년, 월, 일 자료형이다.|
|time|3byte 이며 시, 분, 초 자료형이다.|
|datetime|8byte 이며 날짜와 시간(기간) 자료형이다. now() 함수를 써주면 현재시간으로 데이터가 입력된다.|


### 정수, 실수형 자료형

|자료형|설명|
|:------:|:---|
|tinyint|1byte 이며 -128 ~ 127 또는 0 ~ 255 까지 표현 가능하다.|
|smallint|2byte 이며 -32768 ~ 32767 또는 0 ~ 65536 까지 표현 가능하다.|
|mediumint|3byte 이며 -8388608 ~ 8388607 또는 0 ~ 16777215 까지 표현 가능하다.|
|bigint|8byte 이며 무제한 수 표현이 가능하다.|
|float|4byte 이며 고정 소수점 사용 형태이다.|
|decimal|길이 + 1byte 이며 소수점 사용 형태이다.|
|double|8byte 이며 double을 문자열로 저장한다.|


### 문자열 자료형

|자료형|설명|
|:------:|:---|
|char|최대 255byte 이며 고정 길이이다. 지정된 길이보다 짧은 데이터 입력시 나머지 공간은 공백으로 채워진다.|
|tinytext|최대 255byte 이다.|
|text|최대 65535byte 이다.|
|mediumtext|최대 16777215byte 이다.|
|longtext|최대 4294967295byte 이다.|


### 날짜 자료형

|자료형|설명|
|:------:|:---|
|timestamp|4byte 이며 기간 자료형이다. 시스템 변경시 자동으로 그 날짜와 시간이 저장된다.|
|year|1byte 이며 년도 자료형이다|