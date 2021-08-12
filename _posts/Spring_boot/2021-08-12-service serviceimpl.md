---
title: Spring boot Service와 ServiceImpl
tags: Springboot
---

## Summry

본 문서에서는 Service와 ServiceImpl이 나누어진 이유를 정리한다.  

Service가 뭔지 잘 모르겠다면 [Spring boot mysql 연동(mybatis)](https://limjunho.github.io/2021/08/11/spring-mysql.html)참고.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## 분리 이유

**결론부터 말하자면 OOP(Object-Oriented Programming)의 다형성을 충족하기 위해서이다.**  

* 다형성(polymorphism)이란 하나의 객체가 여러 가지 타입을 가질 수 있는 것을 의미한다.
  * 자바에서는 보통 부모 클래스를 자식이 상속받아 기능을 확장하거나 변경하는 방식으로 쓴다.  

Service라는 부모를 ServiceImpl이라는 자식이 상속받게 되는 것이다.

이름들이 Service와 ServiceImpl인 이유는 **부모인 Service를 interface로 만들고, 자식인 ServiceImpl 클래스가 부모를 상속받아 implement하는 구현체**이기 때문이다.  
이렇게 직관적으로 자식 클래스의 이름을 ~Impl로 짓는 것은 해외의 개발자들도 따르는 naming convention이다.  

## 각자의 역할

키보드를 예시로 들어보겠다. 이것을 역할과 구현으로 나누면, **타자를 치는 역할**을 구현하기 위해 사용자는 A 키보드를 사용할 수 있고 B 키보드를 사용할 수도 있다. 키보드를 바꾸어도 역할에는 영향이 없다.  

이걸 Service와 ServiceImpl로 생각하면 KeyboardService라는 interface를 implement하는 AkeyboardServiceImpl과 BkeyboardServiceImpl class들이 “타자를 치는” 기능을 구현하는 것이다.  

**이렇게 역할과 구현으로 나누게 되면 역할 부분을 새로운 방식으로 구현해야 할 때 구현하는 부분만 손쉽게 바꿀 수 있다. 따라서 Service라는 interface를 만든 후 해당 역할을 구현하는 ServiceImpl class를 만드는 것이다.**  

## Reference

> [[Spring] Service와 ServiceImpl - Jamie](https://itzjamie96.github.io/2021/01/24/spring-service-and-serviceimpl/)  
