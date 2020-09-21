---
title: Array Modify Example(Hook)
tags: React
---

[Start React project](https://limjunho.github.io/2020/09/12/Start-React.html) - 프로젝트만들기  
[JSX Example](https://limjunho.github.io/2020/09/15/JSX-Example.html) - 컴포넌트를 만드는 법과 JSX문법  
[state and props](https://limjunho.github.io/2020/09/15/state_and_props.html) - State와 props  

**리엑트에서 배열에 원소를 수정하는 예제**  
리액트애서는 state 내부의 값을 직접적으로 수정하면 절대로 안되며 이를 불변성 유지라고 한다.  
push, splice, unshift, pop 같은 내장함수는 배열 자체를 직접 수정하게 되므로 적합하지 않다.  
그 대신에, **기존의 배열에 기반하여 새 배열을 만들어내는 함수인 concat, slice, map, filter 같은 함수를 사용해야한다.**  

[Array Append Example](https://limjunho.github.io/2020/09/17/Array_Append_Example.html)  
[Array Delete Example](https://limjunho.github.io/2020/09/21/Array_Delete_Example.html)

[React Array Example코드](https://github.com/limjunho/React/tree/master/array_ex)  
지금까지 배열 예제코드는 이 코드에 모두 적용(github)  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### 배열 항목 수정하기   

```jsx
import React from 'react';

function User({ user, onRemove, onUpdate }) {
    return (
        <div>
            <b>{user.username}</b> <span>({user.phonenumber})</span>
            <button onClick={() => onRemove(user.id)}>삭제</button>
            <button onClick={() => onUpdate(user.id)}>수정</button>
        </div>
    )
}

function UserList({ users, onRemove, onUpdate }) {
    return (
        <div>
            {users.map(user => (
                <User user={user} 
                key={user.id} 
                onRemove={onRemove} 
                onUpdate={onUpdate} />
            ))}
        </div>
    );
}

export default UserList;
```
**UserList.js**  
각 User 컴포넌트를 보여줄 때, 수정버튼도 함께 랜더링하도록 코드수정.  
onUpdate는 App.js에서 구현  
<br />
<br />

```jsx
import React, { useRef, useState } from 'react';
import UserList from './components/UserList';
import CreateUser from './components/CreateUser';

function App() {
  const [users, setUsers] = useState([
    {
      id: 1,
      username: '임준호',
      phonenumber: '010-0000-0000'
    },
    {
      id: 2,
      username: 'testuser1',
      phonenumber: '010-1234-1234'
    },
    {
      id: 3,
      username: 'testuser2',
      phonenumber: '010-2222-4444'
    }
  ]);

  const [inputs, setInputs] = useState({
    username: '',
    phonenumber: ''
  });
  const { username, phonenumber } = inputs;

  const onChange = e => {
    const { name, value } = e.target;
    setInputs({
      ...inputs,
      [name]: value
    });
  };

  const nextId = useRef(4);

  const onCreate = () => {
    const user = {
      id: nextId.current,
      username,
      phonenumber
    };
    setUsers(users.concat(user))

    setInputs({
      username: '',
      phonenumber: ''
    });

    nextId.current += 1;
  }

  const onRemove = id => {
    setUsers(users.filter(user => user.id !== id));
  }

  const onUpdate = id => {
    setUsers(
      users.map(user =>
        user.id === id ?
          { username: username, phonenumber: phonenumber }
          : user
      )
    )

    setInputs({
      username: '',
      phonenumber: ''
    });
  }

  return (
    <div>
      <CreateUser
        username={username}
        phonenumber={phonenumber}
        onChange={onChange}
        onCreate={onCreate}
      />
      <UserList users={users} onRemove={onRemove} onUpdate={onUpdate} />
    </div>
  );
}

export default App;
```
**App.js**  
onUpdate를 구현  
-> user.id가 일치하는 원소의 값을 수정하여 새로운 배열을 만든다.  

[useRef Example](https://limjunho.github.io/2020/09/21/useRef-Example.html)  
useRef는 위의 정리글 참고  

**값을 입력하고 submit!!대신에 수정을 입력하면 값이 수정된다.**  
<br />
<br />

![그림1](/assets/React/post10_array_modify_ex/1.PNG)  
![그림2](/assets/React/post10_array_modify_ex/2.PNG)  
**예제 실행결과**  