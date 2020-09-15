---
title: state and props
tags: React
---

React 컴포넌트에서 다루는 데이터는 크게 두 가지로 props, state이다.  
**props는 부모 컴포넌트가 자식 컴포넌트에 넘기는 값**    
**state는 컴포넌트 내부에서 선언하며 내부에서 값을 변경**  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### props 예제  

**src디렉터리에 Test.js파일을 만든다.**  
-> 새 컴포넌트 생성  

```jsx
import React, { Component } from 'react';

class Test extends Component {
  render() {
    return (
      <div>
        props에서 받은 값은 <b>{this.props.value}</b> 입니다.
      </div>
    );
  }
}

export default Test;
```
**Class형 컴포넌트(props예제)**
<br />  

```jsx
import React from 'react';

const Test = ({ value }) => {
  return (
    <div>
      props에서 받은 값은 {value} 입니다.
    </div>
  );
};

export default Test;
```
**함수형 컴포넌트(props예제)**  
위의 코드들은 부모 컴포넌트로부터 값을 받아 보여주는 코드이다.  
<br />

**props를 실수로 빠트리거나, 특정 상황에 의도적으로 props를 전달하지 않을 때 기본값을 defaultProps라고 한다.**  
아래의 코드들은 defaultProps예제이다.  

```jsx
import React, { Component } from 'react';

class Test extends Component {
  static defaultProps = {
    value: 'defalut'
  }
  render() {
    return (
      <div>
        props에서 받은 값은 <b>{this.props.value}</b> 입니다.
      </div>
    );
  }
}

export default Test;
```
**Class형 컴포넌트(defaultProps예제)**
<br />


```jsx
import React from 'react';

const Test = ({ value }) => {
  return (
    <div>
      props에서 받은 값은 {value} 입니다.
    </div>
  );
};

Test.defaultProps={
  value: 'default'
}
export default Test;
```
**함수형 컴포넌트(defaultProps예제)**  
<br />

### 새 컴포넌트 사용  

```jsx
import React from 'react';
import Test from './Test';

function App() {
  return (
    <div className="App">
      <Test value = "테스트"/>
    </div>
  );
}
```
**App.js(props예제)**  
import하여 새로만든 컴포넌트를 불러오고 렌더링.  

![그림1](/assets/React/post4_state_props/1.PNG)  
**props 예제 실행결과**  
<br />

```jsx
import React from 'react';
import Test from './Test';

function App() {
  return (
    <div className="App">
      <Test />
    </div>
  );
}

export default App;
```
**App.js(defaultProps예제)**

![그림2](/assets/React/post4_state_props/2.PNG)  
**defaultProps 예제 실행결과**  
<br />

### state 예제(Hook)

**동적인 데이터를 다룰 때 state사용**  

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
**src디렉터리에 count.js파일 생성**  
**useState Hook을 이용하면 state 변수와 해당 state를 갱신할 수 있는 함수가 만들어진다.**  

```jsx
import React from 'react';
import Count from './Count';

function App() {
  return (
    <div className="App">
      <Count />
    </div>
  );
}

export default App;
```
**App.js**  

![그림3](/assets/React/post4_state_props/3.PNG)  
**Hook state 예제 실행 결과**  

[참고](https://ko.reactjs.org/docs/hooks-state.html)