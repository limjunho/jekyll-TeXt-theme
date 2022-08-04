---
title: Java Checked, Unchecked Exception
tags: JAVA
---

## Summry

본 문서에서는 JAVA의 Checked Exception과 Unchecked Exception을 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

JAVA의 예외는 크게 3가지로 나눌 수 있다.  
* Checked Exception
* Unchecked Exception
* Error

![그림1](/assets/JAVA/Exception/1.png)

자바에서 에러, 예외 관련된 클래스들의 계층구조는 위와 같다. Throwable 클래스를 기준으로 Error, Exception 클래스로 나뉘며 왼쪽에 보이는 Error는 말 그대로 에러와 관련된 클래스이다. 그리고 오른쪽에 보이는 Exception 도 말 그대로 예외와 관련된 클래스다.  
**자바에서는 실행 시(runtime) 발생할 수 있는 프로그램 오류를 에러(error)와 예외(exception) 두 가지로 구분하였다.**

## Error

에러는 시스템에 비정상적인 상황이 발생했을 경우에 발생한다. 예를들어, 메모리 부족(OutofMemoryError)이나 스택오버플로우(StackOverflowError)가 있다.

## Exception

예외(Exception)는 프로그램 실행 중에 개발자의 실수로 예기치 않은 상황이 발생했을 때 이다. 예를들어, 배열의 범위를 벗어난(ArrayIndexOutOfBoundsException), 값이 null이 참조변수를 참조(NullPointerException), 존재하지 않는 파일의 이름을 입력 (FileNotFoundException) 등이 있다.  

이러한 예외는 2가지로 나눌 수 있다.
* Checked Exception
* Uncheck Exception

### Checked Exception

**문법적으로 이미 명백한 오류가 범해진 상황인 경우 Checked Exception이 발생하게 되며 이에 대해 처리를 강제한다.**  
컴파일 단계에서 발생하는 Exception으로 대표적인 예는 아래와 같다.  
* IOException
* SQLException

### Uncheckd Exception

**실행 중(runtime)에서 발생하는 Exception이 Uncheckd Exception으로 분류되며 명시적으로 처리를 강제하지 않는다.**  처리를 강제하지 않는 이유는 개발자들의 실수로 발생하는 것들이기 때문이며 처리를 강제한다면 단순 배열의 원소를 출력하는 코드조차 try/catch문을 사용해야 할 것이다.  
실행(runtime)에서 발생하는 Exception으로 대표적인 예는 아래와 같다.  
* NullPointerException
* IllegalArgumentException
* IndexOutOfBoundException
* SystemException

### Checked Exception vs Uncheck Exception

||Checked Exception|Unchecked Exception|
|--|--|--|
|처리여부|반드시 예외 처리 필요|명시적 처리 강제하지 않음|
|확인시점|컴파일 단계|실행 중(Runtime) 단계|
|예외발생 시 트랜잭션|롤백하지 않음|롤백|
|대표 예외|IOException <br> SQLException|NullPointerException <br> IllegalArgumentException <br> IndexOutOfBoundException <br> SystemException|

## Reference

> [[Java] Checked Exception vs Unchecked Exception 정리 - Gyun's 개발일지](https://devlog-wjdrbs96.tistory.com/351)  
> [스프링 부트에서의 예외 처리 (Exception) | @ControllerAdvice, @ExceptionHandler [스프링 부트 (Spring Boot) - 어라운드허브 스튜디오]](https://www.youtube.com/watch?v=nyN4o9eXqm0&list=LL&index=4&t=562s&ab_channel=%EC%96%B4%EB%9D%BC%EC%9A%B4%EB%93%9C%ED%97%88%EB%B8%8C%EC%8A%A4%ED%8A%9C%EB%94%94%EC%98%A4-AroundHubStudio)  