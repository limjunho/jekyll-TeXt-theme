---
title: Array Append Example(Hook)
tags: React
---

[Start React project](https://limjunho.github.io/2020/09/12/Start-React.html) - 프로젝트만들기  
[JSX Example](https://limjunho.github.io/2020/09/15/JSX-Example.html) - 컴포넌트를 만드는 법과 JSX문법  
[state and props](https://limjunho.github.io/2020/09/15/state_and_props.html) - State와 props  

**리엑트에서 배열에 원소를 추가하는 예제**  
리액트애서는 state 내부의 값을 직접적으로 수정하면 절대로 안되며 이를 불변성 유지라고 한다.  
push, splice, unshift, pop 같은 내장함수는 배열 자체를 직접 수정하게 되므로 적합하지 않다.  
그 대신에, **기존의 배열에 기반하여 새 배열을 만들어내는 함수인 concat, slice, map, filter 같은 함수를 사용해야한다.**  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### 배열 항목 추가하기   

**[Array Rendering Example](https://limjunho.github.io/2020/09/16/Array_Rendering_Example.html)을 배열에 값을 추가할 수 있도록 코드 수정**  

```jsx
import React from 'react';

function User({ user }) {
    return (
        <div>
            <b>{user.username}</b> <span>({user.phonenumber})</span>
        </div>
    )
}

function UserList({ users }) {
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
App.js에서 UserList로 usesr를 전달.  
<br />
<br />

```jsx
import React from 'react';

function CreateUser({ username, phonenumber, onChange, onCreate }) {
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
      <button onClick={onCreate}>submit!!</button>
    </div>
  );
}

export default CreateUser;
```
**CreateUser.js**  
이름과 전화번호를 입력할 공간과 버튼을 구현  
onChange와 OnCreate는 App.js에서 구현하여 props로 전달  
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

    nextId.current += 1;
  }

  return (
    <div>
      <CreateUser 
        username={username}
        phonenumber={phonenumber}
        onChange={onChange}
        onCreate={onCreate}
      />
      <UserList users={users} />
    </div>
  );
}

export default App;
```
**App.js**  
onCreate는 버튼을 눌렀을 때 호출됨 -> 배열에 값을 추가하는 코드를 작성해야한다.  
<br />

![그림1](/assets/React/post8_array_append_ex/1.PNG)  
**예제 실행결과**  
<br />
<br />

### 전개연산자(spread)로 항목 추가  
전개연산자는 배열을 통째로 매개변수에 넘겨야 할 때 사용되며 ...ary와 같은 형식으로 사용한다.  

```jsx
const user = {
  name: 'limjunho'
};

const user2 = {
  name: 'limjunho',
  age: '20'
};

const user3 = {
  name: 'limjunho',
  age: '20',
  sex: 'man'
};
```
**전개연산자(spread)를 사용하지 않고 기존 객체를 수정하지 않고 새로운 객체를 만드는 경우**  
<br />

```jsx
const user = {
  name: 'limjunho'
};

const user2 = {
  ...user,
  age: '20'
};

const user3 = {
  ...user2,
  sex: 'man'
};
```
**전개연산자(spread)를 사용하여 기존 객체를 수정하지 않고 새로운 객체를 만드는 경우**
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
      id : nextId.current,
      username,
      phonenumber
    };
    setUsers([...users, user])

    setInputs({
      username: '',
      phonenumber: ''
    });

    nextId.current += 1;
  }

  return (
    <div>
      <CreateUser 
        username={username}
        phonenumber={phonenumber}
        onChange={onChange}
        onCreate={onCreate}
      />
      <UserList users={users} />
    </div>
  );
}

export default App;
```
**App.js**  
<br />

### concat함수로 항목 추가  
concat 함수는 기존의 배열을 수정하지 않고, 새로운 원소가 추가된 새로운 배열을 만든다.  

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
      id : nextId.current,
      username,
      phonenumber
    };
    setUsers(users.concat(user));

    setInputs({
      username: '',
      phonenumber: ''
    });

    nextId.current += 1;
  }

  return (
    <div>
      <CreateUser 
        username={username}
        phonenumber={phonenumber}
        onChange={onChange}
        onCreate={onCreate}
      />
      <UserList users={users} />
    </div>
  );
}

export default App;
```
**App.js**  
<br />

![그림2](/assets/React/post8_array_append_ex/2.PNG)  
**예제 실행결과**  
<br />