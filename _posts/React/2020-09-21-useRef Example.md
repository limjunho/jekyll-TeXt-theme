---
title: useRef Example(Hook)
tags: React
---

**useRef로 특정 DOM을 선택하고 컴포넌트 내의 변수를 생성하는 예제**  
1. 프로젝트를 하다보면 가끔 DOM을 직접 선택해야하는 경우가 발생한다.  
**ex) 특정 엘리먼트의 크기를 가져오거나 스크롤바의 위치를 가져오는 경우, 포커스를 설정하는 경우**  
이 때, React에서는 ref를 사용한다. **함수형 컴포넌트(Hook)에서는 useRef라는 Hook함수를 사용**  
이 포스팅에서는 포커스를 설정하는데 ref를 사용한다.
<br />

2. useRef로 관리하는 변수는 값이 바뀌더라도 컴포넌트가 리랜더링되지 않는다.  
**리액트 컴포넌트에서의 상태는 상태를 바꾸는 함수를 호출하고 나서 렌더링 이후로 업데이트 된 상태를 조회**  
**useRef 로 관리하고 있는 변수는 설정 후 바로 조회**  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### 예제만들기(포커스 설정예제)   

```jsx
import React, { useState, useRef } from 'react';

function InputSample() {
  const [inputs, setInputs] = useState({
    username: '',
    phonenumber: ''
  });

  const { username, phonenumber } = inputs;

  const onChange = e => {
    const { value, name } = e.target; 
    setInputs({
      ...inputs, 
      [name]: value 
    });
  };

  return (
    <div>
      <input
        name="username"
        placeholder="input name..."
        onChange={onChange}
        value={username}
      />
      <br />
      <input
        name="phonenumber"
        placeholder="input phonenumber..."
        onChange={onChange}
        value={phonenumber}
      />
      <br />
      <button>초기화</button>
      <div>
        {username} ({phonenumber})
      </div>
    </div>
  );
}

export default InputSample;
```
**App.js**  
특정 값을 입력하면 초기화 버튼 아래에 입력값이 랜더링되는 예제  
<br />
<br />

![그림1](/assets/React/post11_useRef_ex/1.PNG)  
**예제 실행결과**  
<br />

### useRef적용  
```jsx
import React, { useState, useRef } from 'react';

function InputSample() {
  const [inputs, setInputs] = useState({
    username: '',
    phonenumber: ''
  });
  const nameInput = useRef();

  const { username, phonenumber } = inputs; 

  const onChange = e => {
    const { value, name } = e.target; 
    setInputs({
      ...inputs, 
      [name]: value 
    });
  };

  const onReset = () => {
    setInputs({
      username: '',
      phonenumber: ''
    });
    nameInput.current.focus();
  };

  return (
    <div>
      <input
        name="username"
        placeholder="input name..."
        onChange={onChange}
        value={username}
        ref={nameInput}
      />
      <br />
      <input
        name="phonenumber"
        placeholder="input phonenumber..."
        onChange={onChange}
        value={phonenumber}
      />
      <br />
      <button onClick={onReset}>초기화</button>
      <div>
        {username} ({phonenumber})
      </div>
    </div>
  );
}

export default InputSample;
```
**App.js**  
**초기화 버튼을 클릭했을 때, username input에 포커스가 잡히도록 기능 구현**  
useRef()로 ref객체를 생성한 뒤, 이 객체를 선택하고 싶은 DOM에 ref값으로 설정  
**onReset함수에서 input에 포커스를 잡는 focus()라는 DOM API를 사용**  
<br />
<br />

**값을 입력하고 초기화 버튼을 누르면 값들이 초기화되며 username input에 포커스가 잡힌다.**  

### 컴포넌트 내의 변수  
배열에 사용하는 고유 id값을 관리하는데 ref를 사용한다.  