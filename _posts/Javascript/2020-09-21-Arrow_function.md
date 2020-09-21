---
title: Arrow function(화살표함수)
tags: Javascript React
---

Arrow function(화살표 함수)는 function 키워드 대신 화살표(=>)를 사용하여 보다 간략한 방법으로 함수를 선언할 수 있다.  
하지만 모든 경우 화살표 함수를 사용할 수 있는 것은 아니다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### Arrow function 특징  

**Arrow function은 익명함수이다.**  
익명함수란 함수의 이름이 없는 함수를 가리킨다.  
따라서 Arrow function표현식은 함수의 이름이 없는 익명함수를 변수에 할당하는 방식이다.  
<br />

**객체 생성자로 이용할 수 없다.**  
-> error발생!  
<br />

**this가 정적이다.**  
화살표 함수는 함수를 선언할 때 this에 바인딩할 객체가 정적으로 결정된다.  
동적으로 결정되는 일반 함수와는 달리 화살표 함수의 this 언제나 상위 스코프의 this를 가리킨다.  
이를 Lexical this라 한다.  
<br />

### Arrow function 기본 문법   

```javascript
// 매개변수 지정 방법
    () => {  } // 매개변수가 없는 경우
     a => {  } // 매개변수가 한 개인 경우 소괄호를 생략 가능
(a, b) => {  } // 매개변수가 여러 개인 경우 소괄호를 생략 불가

// 함수 몸체 지정 방법
x => { return x * x }  // 한줄 구문
x => x * x             // 함수 몸체가 한줄의 구문이라면 중괄호를 생략할 수 있으며 암묵적으로 return된다.

() => {           // 여러줄 구문
  const x = 10;
  return x * x;
};  
```
<br />

```javascript
const onChange = e => {
    const { name, value } = e.target;
    setInputs({
      ...inputs,
      [name]: value
    });
  };
```
**React에서 사용했던 예제**  