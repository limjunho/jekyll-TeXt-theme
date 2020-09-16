---
title: Class vs Hook
tags: React
---

React 컴포넌트는 클래스형 컴포넌트(Class Component)와 함수형 컴포넌트(Functional Component)로 나뉜다.  
기존의 개발방식은 함수형 컴포넌트를 주로 사용하되 state나 Life Cycle method를 사용해야 할 때 클래스형 컴포넌트를 사용  

**하지만 React버전 16.8에 새로 Hook이라는 개념이 추가되며 함수형 컴포넌트에서 클래스형 컴포넌트의 작업을 할 수 있게됨**  

이 포스팅에서는 Class와 Hook을 비교하고 Hook을 소개한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### Class형  

**1. 코드가 길고 복잡하다.**  
constructor, this, binding 등 지켜야 할 규칙이 많아서 코드가 복잡하고 길어진다.  
Life Cycle method로 인해 클래스 자체가 무겁다.  

**2. Logic의 재사용이 어렵다.**  
부분적인 DOM 관련 처리나 API사용 및 state을 다루는 등의 logic에 있어서는 경우에 따라 같은 로직을 2개 이상의 Life Cycle method에 중복해서 넣어야하는 등 재사용이 어렵다.  

```jsx
class App extends Component{
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <h1 className="App-title">Welcome to React</h1>
        </header>
        <p className="App-intro">
          To get started, edit <code>src/App.js</code> and save to reload.
        </p>
      </div>
    );
  }
}
```
**Class형 컴포넌트**

### 함수형(Hook)  

Hook의 등장으로 인해 함수형 컴포넌트에서 클래스형 컴포넌트의 작업까지 할 수 있게되었다.  
**기존 클래스의 단점들을 한번에 해결**  

**1. 선택적 사용**  
기존의 코드를 다시 작성할 필요 없이 일부의 컴포넌트들 안에서 Hook를 사용할 수 있다.  
-> 100% 이전 버전과의 호환성  

**2. 쉬운 Logic의 재사용**  
Hook을 활용한 함수형 컴포넌트에서 원하는 기능을 만들어 필요한 곳에 집어넣기만 하면 된다.  

```jsx
import React, { useState } from 'react';

function Count() {
    const [count, setCount] = useState(0);

    return(
      <div>
        <h1>Counter</h1>
        <div>count: {count}</div>
        <button onClick={() => setCount(count + 1)}>+</button>
        <button onClick={() => setCount(count - 1)}>-</button>
      </div>
    );
}; 

export default Count;
```
**함수형 컴포넌트(Hook)**  

[참고](https://ko.reactjs.org/docs/hooks-overview.html)