---
title: JSX Example
tags: React
---

React에서 사용하는 JSX문법에 대해 알아본다.    

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### 코드 분석  

[Start React project](https://limjunho.github.io/2020/09/12/Start-React.html)에서 만든 hello-react프로젝트를 VSCode에서 불러온다.  
![그림1](/assets/React/post3_JSX_Example/1.PNG)  
**컴포넌트에 해당하는 코드는 App.js에 있다.**  

```jsx
import React from 'react';
import logo from './logo.svg';
import './App.css';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
  );
}

export default App;
```
**App.js**  
JSX를 사용하기 위해서는 React를 꼭 import해야한다.  

그 아래에는 같은 디렉터리에 있는 파일들을 import하는데 이는 webpack을 사용해서 가능한 작업이다.  
나중에 프로젝트를 빌드하게 됐을 때 webpack에서 파일의 확장자에 따라 다른 작업을 하는데, CSS 파일을 불러오게되면 나중에 프로젝트에서 사용한 프로젝트를 한 파일에 모두 결합해주는 작업을 진행하고 자바스크립트 파일을 불러오게되면 모든 코드들이 제대로 로딩되게끔 순서를 설정하고 하나의 파일로 합쳐준다.  
svg 처럼 사전에 따로 설정을 되지 않은 확장자의 경우, 그냥 파일로서 불러온다음에 나중에 특정 경로에 사본을 만들어주게되고, 해당 사본의 경로를 텍스트로 받아오게 된다.  
<br />

**컴포넌트를 만드는 방법은 두 가지가 있는데 클래스를 통해 만들거나 위의 예제처럼 함수를 통해 만드는 방법이 있다.**  
클래스형태의 컴포넌트는 반드시 render함수가 존재해야하며 render함수는 JSX를 return해야한다.  

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
**class예제**  

```jsx
export default App;
```
**위의 코드는 작성한 컴포넌트를 다른 곳에서 불러와 사용할 수 있도록 한다.**  

```jsx
import App from './App';
```
**컴포넌트를 불러오는 코드**  

브라우저 상에 리액트 컴포넌트를 보여주기 위해서는 ReactDOM.render함수를 사용한다.  
파라미터1은 렌더링할 결과물, 파라미터2는 컴포넌트를 그릴 DOM을 지정한다.  
```jsx
ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```
**index.js**  

### JSX  
**1. 태그는 꼭 닫혀야 한다.**  
**2. 두 개 이상의 엘리먼트는 반드시 하나의 엘리먼트로 감싸져야 한다.**  

```jsx
<div>
  Hello
</div>
<div>
  Bye
</div>
```
**잘못된 예제**  

```jsx
<div>
  <div>
    Hello
  </div>
  <div>
    Bye
  </div>
</div>
```
**올바른 예제1**  

```jsx
<Fragment>
  <div>
    Hello
  </div>
  <div>
    Bye
  </div>
</Fragment>
```
**올바른 예제2**  

**3. JSX안에 JSP값 사용**  
```jsx
function App() {
  const name = 'react';
  return (
    <div>
      hello {name}!
    </div>
  );
}
```
**자바스크립트 값 사용 예제**  

**4. 조건부 렌더링**  
JSX에서 조건부 렌더링을 할 때는 삼항 연산자나 AND연산자를 사용  
**if문 사용 불가 -사용하려면 IIFE(즉시 실행 함수 표현)을 사용**  

```jsx
<div>
  {
    1 + 1 === 2 
      ? (<div>true</div>)
      : (<div>flase</div>)
  }
</div>
```
**삼항 연산자 예제**  

```jsx
<div>
  {
     1 + 1 === 2 && (<div>true</div>)
  }
</div>
```
**AND연산자 예제**  

가끔씩은 좀 복잡한 조건을 작성해야 할 때도 있다.   
그러한 조건들은 웬만하면 JSX 밖에서 로직을 작성하는것이 좋지만 꼭 JSX 내부에서 작성해야 한다면  
```jsx
function App() {
  const num = 1;
  return (
    <div>
      {
        (function() {
          if (value === 1) return (<div>1</div>);
          if (value === 2) return (<div>2</div>);
          if (value === 3) return (<div>3</div>);
        })()
      }
    </div>
  );
}
```
**IIFE 예제**  

```jsx
(() => {
  if (value === 1) return (<div>하나</div>);
  if (value === 2) return (<div>둘</div>);
  if (value === 3) return (<div>셋</div>);
})()
```
**화살표 함수 사용예제**  
[화살표 함수?](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/%EC%95%A0%EB%A1%9C%EC%9A%B0_%ED%8E%91%EC%85%98)  

**5. 주석**  

```jsx
import React from 'react';
import './App.css';

function App() {
  return (
    <div>
      <div className="App">
      {/* 주석!! */}
        React!!
      </div>
      <div>
        Hello!!
      </div>
      주석!!
    </div>
  );
}
```
**주석은 {/\* ~~ */}사이에 넣거나, 태그사이에 넣을 수 있다.**  

export default App;
```

### 코드 수정해보기  

```jsx
.App {
  background: black;
  color: antiquewhite;
  font-size: 40px;
  padding: 1rem;
  font-weight: 600;
}
```
**App.css**  

```jsx
import React from 'react';
import './App.css';

function App() {
  return (
    <div className="App">
      React!!
    </div>
  );
}

export default App;
```
**App.js**  

![그림2](/assets/React/post3_JSX_Example/2.PNG)  
**실행결과**  