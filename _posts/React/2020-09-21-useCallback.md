---
title: useCallback Example(Hook)
tags: React
---

**특정 함수를 재사용하는 useCallback예제**

[useMemo Example](https://limjunho.github.io/2020/09/21/useMemo.html)는 특정 결과값을 재사용하는 반면, useCallback은 특정 함수를 새로 만들지 않고 재사용한다.

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### useCallback Example

[Array Example](https://limjunho.github.io/2020/09/21/Array_Modify_Example.html)에서 사용하던 onChange, onCreate, onRemove, onUpdate등의 함수는 컴포넌트가 리렌더링될 때 마다 새로 만들어진다.  
**함수를 선언하는 것 자체는 사실 그 자체 만으로 큰 부하가 생길일은 없지만 함수를 재사용하는 것은 여전히 중요**

```jsx
import React, { useRef, useState, useMemo, useCallback } from "react";
import UserList from "./components/UserList";
import CreateUser from "./components/CreateUser";

function countUsers(users) {
  console.log("counting users...");

  return users.length;
}

function App() {
  const [users, setUsers] = useState([
    {
      id: 1,
      username: "임준호",
      phonenumber: "010-0000-0000",
    },
    {
      id: 2,
      username: "testuser1",
      phonenumber: "010-1234-1234",
    },
    {
      id: 3,
      username: "testuser2",
      phonenumber: "010-2222-4444",
    },
  ]);

  const [inputs, setInputs] = useState({
    username: "",
    phonenumber: "",
  });
  const { username, phonenumber } = inputs;

  const onChange = useCallback(
    (e) => {
      const { name, value } = e.target;
      setInputs({
        ...inputs,
        [name]: value,
      });
    },
    [inputs]
  );

  const nextId = useRef(4);

  const onCreate = useCallback(() => {
    const user = {
      id: nextId.current,
      username,
      phonenumber,
    };
    setUsers(users.concat(user));

    setInputs({
      username: "",
      phonenumber: "",
    });

    nextId.current += 1;
  }, [users, username, phonenumber]);

  const onRemove = useCallback(
    (id) => {
      setUsers(users.filter((user) => user.id !== id));
    },
    [users]
  );

  const onUpdate = useCallback(
    (id) => {
      setUsers((users) =>
        users.map((user) =>
          user.id === id
            ? { id: id, username: username, phonenumber: phonenumber }
            : user
        )
      );

      setInputs({
        username: "",
        phonenumber: "",
      });
    },
    [users, username, phonenumber]
  );

  const count = useMemo(() => countUsers(users), [users]);

  return (
    <div>
      <CreateUser
        username={username}
        phonenumber={phonenumber}
        onChange={onChange}
        onCreate={onCreate}
      />
      <UserList users={users} onRemove={onRemove} onUpdate={onUpdate} />
      <div>사용자 수 : {count}</div>
    </div>
  );
}

export default App;
```

**App.js**  
**함수 안에서 사용하는 상태 혹은 props 가 있다면 반드시 deps 배열안에 포함시켜야 한다.**  
위의 조건을 지키지 않는 경우 함수 내에서 해당 값들을 참조할 때 가장 최신 값을 참조할 것이라 보장할 수 없다.  
props로 받아온 함수도 deps에 포함!

<br />

### 추가사항

**useCallback을 사용한다고 눈에 띄는 최적화는 없지만 컴포넌트 렌더링 최적화 작업을 같이 하면 성능이 최적화된다.**
