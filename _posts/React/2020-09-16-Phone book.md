---
title: Phone book Example(Hook)
tags: React
---

[Start React project](https://limjunho.github.io/2020/09/12/Start-React.html) - 프로젝트만들기  
[JSX Example](https://limjunho.github.io/2020/09/15/JSX-Example.html) - 컴포넌트를 만드는 법과 JSX문법  
[state and props](https://limjunho.github.io/2020/09/15/state_and_props.html) - State와 props  

전화번호 입력 예제  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### 프로젝트 생성하기  

```bash
$ create-react-app phone_book_ex
```
**새 프로젝트 생성**  

### 컴포넌트 생성  

**1. src디렉터리에 components디렉터리 생성**  

**2. components디렉터리에 PhoneBook.js파일 생성**  

```jsx
import React, { useState } from 'react';

function PhoneBook(){
    const [name, Setname] = useState('');
    const [number, Setnumber] = useState('');

    const handleChangeName = e => {
        Setname(e.target.value);
    }

    const handleChangeNumber = e => {
        Setnumber(e.target.value);
    }

    return(
        <div>
        <h1>PhoneBook!</h1>
        <form>
            <input
            placeholder="input name..."
            value={name}
            onChange={handleChangeName}
            />
            <br />
            <input
            placeholder="input number..."
            value={number}
            onChange={handleChangeNumber}
            />
            <div>{name} {number}</div>
        </form>
        </div>
    );
};

export default PhoneBook;
```
**PhonBook.js**
onChange 이벤트가 발생하면 e.target.value 값을 통하여 이벤트 객체에 담겨있는 현재의 텍스트 값을 읽어올 수 있다.  
<br />  

```jsx
import React from 'react';
import PhoneBook from './components/PhoneBook';

function App() {
  return (
    <div>
      <PhoneBook />
    </div>
  );
}

export default App;
```
**App.js**  
<br />

![그림1](/assets/React/post6_Phone_book/1.PNG)  
**예제 실행결과**  
<br />

### 입력값 로그남기기  

```jsx
import React, { useState } from 'react';

function PhoneBook(){
    const [name, Setname] = useState('');
    const [number, Setnumber] = useState('');

    const handleChangeName = e => {
        Setname(e.target.value);
    }

    const handleChangeNumber = e => {
        Setnumber(e.target.value);
    }

    const handleUpdateLog = e => {
        console.log(name,number);
    }
    
    return(
        <div>
        <h1>PhoneBook!</h1>
        <form>
            <input
            placeholder="input name..."
            value={name}
            onChange={handleChangeName}
            />
            <br />
            <input
            placeholder="input number..."
            value={number}
            onChange={handleChangeNumber}
            />
            <div>{name} {number}</div>
            <button onClick={handleUpdateLog} type="button">Submit!!</button>
        </form>
        </div>
    );
};

export default PhoneBook;
```
**PhoneBook.js**  
onClick 이벤트 발생 시 이름과 전화번호를 log로 남긴다.  
<br />

![그림2](/assets/React/post6_Phone_book/2.PNG)  
**예제 실행결과**  

[소스코드](https://github.com/limjunho/React/tree/master/phone_book_ex)
[참고](https://ko.reactjs.org/docs/hooks-state.html)
