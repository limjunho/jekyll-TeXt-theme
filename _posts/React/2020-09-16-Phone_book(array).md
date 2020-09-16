---
title: Array Rendering Example(Hook)
tags: React
---

[Start React project](https://limjunho.github.io/2020/09/12/Start-React.html) - 프로젝트만들기  
[JSX Example](https://limjunho.github.io/2020/09/15/JSX-Example.html) - 컴포넌트를 만드는 법과 JSX문법  
[state and props](https://limjunho.github.io/2020/09/15/state_and_props.html) - State와 props  

**리엑트에서 배열을 렌더링하는 예제**  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### 배열 렌더링(index접근)  

```jsx
import React from 'react';

function User({ user }) {
    return (
        <div>
            <b>{user.username}</b> <span>({user.phoneNumber})</span>
        </div>
    )
}

function UserList() {
    const users = [
        {
            id: 1,
            username: '임준호',
            phoneNumber: '010-0000-0000'
        },
        {
            id: 2,
            username: 'testuser1',
            phoneNumber: '010-1234-1234'
        },
        {
            id: 3,
            username: 'testuser2',
            phoneNumber: '010-2222-4444'
        }
    ];

    return (
        <div>
            <User user={users[0]} />
            <User user={users[1]} />
            <User user={users[2]} />
        </div>
    );
}

export default UserList;
```
**UserList.js**  
배열의 인덱스를 조회하는 방식  
<br />

### 배열 렌더링(동적배열 접근)  

```jsx
import React from 'react';

function User({ user }) {
    return (
        <div>
            <b>{user.username}</b> <span>({user.phoneNumber})</span>
        </div>
    )
}

function UserList() {
    const users = [
        {
            id: 1,
            username: '임준호',
            phoneNumber: '010-0000-0000'
        },
        {
            id: 2,
            username: 'testuser1',
            phoneNumber: '010-1234-1234'
        },
        {
            id: 3,
            username: 'testuser2',
            phoneNumber: '010-2222-4444'
        }
    ];

    return (
        <div>
            {users.map(user => (
                <User user={user} key={user.id} />
            ))}
        </div>
    );
}

export default UserList;
```
**UserList.js**  
동적인 배열을 렌더링 할 때는 자바스크립트 배열의 내장함수 map()사용.  
**mpa()함수는 배열안의 각 원소를 변환하여 새 배열을 만들어준다.**  
**리엑트에서 배열을 렌더링 할 때는 key라는 props를 설정.**  
-> key값은 원소들이 가진 고유값으로 설정(만약 없다면 map()의 2번 째 파라미터 index를 key로 사용)  
<br />  

```jsx
import React from 'react';
import UserList from './components/UserList';

function App() {
  return (
    <div>
      <UserList />
    </div>
  );
}

export default App;
```
**App.js**  
<br />

![그림1](/assets/React/post7_array_ex/1.PNG)  
**예제 실행결과**  