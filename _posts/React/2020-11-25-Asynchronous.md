---
title: 비동기(Asynchronous)처리
tags: React
---

**Javascript는 동기적인 언어이고, blocking이며, single-threaded한 언어이다. 하지만 이것은 한번에 한번의 작업만 진행할 수 있다는 특성을 의미한다.**  
만약 많은 양의 데이터 베이스를 요청해야 한다면 동기적으로 처리했을 때 하나의 작업에 대한 요청을 하고 응답을 기다리는 동안 다른 작업은 대기해야 할것이다.  

**이러한 이슈를 해결하기 위해 Javascript를 비동기적으로 동작하도록 할 수 있다.**  

**비동기적으로 처리하는 경우**  
* Ajax Web API 요청 : 서버쪽에서 데이터를 받아와야 하는 경우
* 파일 읽기 : 서버에서 파일을 읽어야 하는 경우
* 암호화/복호화 : 바로 처리 되지 않고, 시간이 어느정도 걸리는 경우
* 작업 예약 : setTimeout을 사용하여 비동기 처리하는 경우  


[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## 비동기처리방식  

### CallBack  
**콜백 함수는 함수 안에서 어떤 특정한 시점에 호출되는 함수**를 말한다.  
보통 콜백함수는 함수의 매개변수로 전달하여 특정 시점에 콜백 함수를 호출한다.  

**동기 처리에서 가장 간단한 해결책은 비동기 callback을 사용하는 것이다.**  
콜백 지옥은 비동기 처리를 위해 콜백 함수를 연속해서 사용할 때 발생하는 문제이다.  

```javascript
doSomething(function(result) {
  doSomethingElse(result, function(newResult) {
    doThirdThing(newResult, function(finalResult) {
      console.log('Got the final result: ' + finalResult);
    }, failureCallback);
  }, failureCallback);
}, failureCallback);
```

함수의 반환 값을 받은 다음 비동기 처리를 해야 하는 경우에 위와 같은 함수를 콜백으로 받아 비동기 처리를 할 수 있다.  
**하지만 콜백 함수는 가독성이 매우 떨어진다.**  
비동기 처리가 위처럼 3개로 끝나지 않는다면 끝없이 옆으로 누운 피라미드를 그리게 된다.

* 이러한 콜백 지옥을 해결하는 방법에는 Promise와 Async/await를 사용할 수 있다.  

### Promise  
Promise는 자바스크립트에서 비동기 작업을 좀 더 편리하게 처리 할 수 있도록 ES6에 도입된 기능이다.  

```javascript
const promise = new Promise ((resolve, reject) => {
	//executor 실행자, 실행 함수.. 
})
```

* resolve : 작업이 성공적으로 끝난 경우, 그 결과를 나타내는 value와 함께 호출한다.
* reject : 에러 발생 시 에러 객체를 나타내는 error와 함께 호출한다.  
**executor는 자동으로 실행되는데 여기서 원하는 일이 처리된다.**  
처리가 끝나면 성공 여부에 따라 resolve나 reject를 호출한다.  
</br>

> Promise의 3가지 상태(states)  
프로미스는 3가지의 상태(states)를 가지며, 여기서 상태란 프로미스의 처리 과정을 의미한다.  
new Promise()로 프로미스를 생성하고 종료될 때까지 3가지 상태를 가진다.  
* Pending : 비동기 처리 로직이 아직 완료되지 않은 상태 (대기)
* Fulfilled : 비동기 처리가 완료되어 프로미스가 결과 값을 반환해준 상태 (성공)
* Rejected : 비동기 처리가 실패하거나 오류가 발생한 상태(실패)

```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject(new Error());
  }, 1000);
});

myPromise
  .then(n => {
    console.log(n);
  })
  .catch(error => {
    console.log(error);
  });
```
성공하면 .then()메소드가 실행되고, 실패하면 reject를 사용하고,  
error는.catch()메소드를 사용해서 처리한다.  

### async/ await  
async/ await는 ES8문법으로서 가장 최근에 나온 문법이다.  
**콜백함수와 promise의 단점을 보완하고 개발자가 읽기 좋은 코드를 작성하게 도와준다.**  

async/await는 promise를 기반으로 모든 async함수는 promise를 리턴하고, 모든 await함수는 일반적으로 promise가 된다.  
</br>

> 기본문법
```javascript
async function 함수명(){
 await 비동기처리_메서드명();
}
```
함수 앞에 async를 붙이고, 호출할 비동기 함수 앞에 즉 promise앞에 await 키워드를 붙인다.  
**함수 앞에 async가 선언 되어야만 await를 사용할 수 있다.**  

* async  
  * function앞에 위치한다. function앞에 async를 붙이면 해당 함수는 항상 promise를 반환한다.  
* await  
  * await를 만나면 promise가 처리될 때 까지 기다리고 결과는 그 이후에 변환된다.  
  * 일반 함수에는 사용할 수 없으며 async함수가 아닌데 await를 사용하면 문법 에러가 발생한다.  

**async & await에서 예외를 처리하는 방법은 try-catch이다.**  
```javascript
async function logTodoTitle() {
  try {
    var user = await fetchUser();
    if (user.id === 1) {
      var todo = await fetchTodo();
      console.log(todo.title); // delectus aut autem
    }
  } catch (error) {
    console.log(error);
  }
}
```

> 출처
https://velog.io/@daybreak/Javascript-%EB%B9%84%EB%8F%99%EA%B8%B0-%EC%B2%98%EB%A6%AC