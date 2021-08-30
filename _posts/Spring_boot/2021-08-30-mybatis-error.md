---
title: mybatis 비교연산자 에러
tags: Springboot
---

## Summry

본 문서에서는 mybatis에서 비교연산자를 잘못 사용하는 경우 발생하는 에러의 해결방법을 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## error

**mapper.xml에서 비교연산자 <를 사용하는 경우 오류가 발생한다.**  
* < 를 태그의 시작으로 인식

```where #{idx} < 5```

## conflict

```where #{idx} <![CDATA[ < ]]> 5```  
위처럼 <![CDATA[]]>를 사용한다.

## Reference

> [mybatis mapper.xml 비교연산자 <=, >=, <, > 처리방법](https://sinpk.tistory.com/entry/mybatis-mapperxml-%EB%B9%84%EA%B5%90%EC%97%B0%EC%82%B0%EC%9E%90-%EC%B2%98%EB%A6%AC%EB%B0%A9%EB%B2%95)  