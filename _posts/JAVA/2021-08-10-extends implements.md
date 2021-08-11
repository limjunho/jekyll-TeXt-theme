---
title: JAVA extends, implements 차이
tags: JAVA
---

## Summry  

본 문서에서는 extends와 implements의 차이를 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## extends(상속)

**상속의 대표적인 형태로 부모의 메소드를 그대로 사용할 수 있으며 오버라이딩 할 필요 없이 부모에 구현되있는 것을 직접 사용 가능하다.**  

```java
class Vehicle {
  protected int speed = 3;
  
  public int getSpeed(){
    return speed;
  }
  public void setSpeed(int speed){
    this.speed = speed;
  }
}

class Car extends Vehicle{
  public void printspd(){
    System.out.println(speed);
  }
}

public class ExtendsSample {
  public static main (String[] args){
    Car A = new Car();
    System.out.println(A.getSpeed());
    A.printspd();
  }
}
```
Car라는 클래스는 Vehicle을 상속 받았다. 그리고 getSpeed() 메소드로 speed를 print 했다.
그런데 vehicle의 getSpeed 메소드에서 speed를 직접 바로 사용했다. extends를 했으니 메소드 뿐만 아니라 변수까지 사용이 가능해 지는 것이다.  

---
**자바는 "다중상속"을 지원하지 않는다.**  
다중상속이란 부모클래스가 두개 이상 존재할 수 있다는 것인데, 자바에서는 이를 지원하지 않는다.  
즉, ```public class Son extends Father, Mother{...}``` 이것이 지원되지 않는다.  
대신 implements(인터페이스)가 등장했다.  

## implements(상속)

```java
interface TestInterface{
  public static int num = 8;
  public void fun1();
  public void fun2();
}

class InterfaceExam implements TestInterface{
  @Override
  public void fun1(){
    System.out.println(num);
  }
  
  @Override
  public void fun2() {
    
  }
}

public class InterfaceSample{
  public static void main(String args[]){
    InterfaceExam exam = new InterfaceExam();
    exam.fun1();
  }
}
```
**implements의 가장 큰 특징은 이렇게 부모의 메소드를 반드시 오버라이딩(재정의)해야 한다.**  

* 또한 이 implements는 다중상속을 대신해준다.
* ```public class Son implements Father, Mother{...}``` 이러한 형태로..

## 정리

* extends는 일반 클래스와 abstract 클래스 상속에 사용되고, implement는 interface 상속에 사용된다.
* class가 class를 상속받을 땐 extends를 사용하고, interface가 interface를 상속 받을 땐 extends를 * 사용한다.
* class가 interface를 사용할 땐 implements를 써야하고
* interface가 class를 사용할 땐 implements를 쓸수 없다.
* extends는 클래스 한 개만 상속 받을 수 있다.
* extends 자신 클래스는 부모 클래스의 기능을 사용한다.
* implements는 여러개 사용 가능하다.
* implements는 설계 목적으로 구현 가능하다.
* implements한 클래스는 implements의 내용을 다 사용해야 한다.

## Reference

> [자바 extends, implements 차이 - hkoo9329](https://velog.io/@hkoo9329/%EC%9E%90%EB%B0%94-extends-implements-%EC%B0%A8%EC%9D%B4)  