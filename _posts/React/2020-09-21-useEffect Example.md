---
title: useEffect Example(Hook)
tags: React
---

**useEffect 예제**  
useEffect 라는 Hook 을 사용하여 컴포넌트가 마운트 됐을 때 (처음 나타났을 때), 언마운트 됐을 때 (사라질 때), 그리고 업데이트 될 때 (특정 props가 바뀔 때) 특정 작업을 처리 
<br />

[React Array Example코드](https://github.com/limjunho/React/tree/master/array_ex)  
예제 소스코드(github)  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.  

<!--more-->

---

### 마운트, 언마운트 관리  

**마운트 시에 하는 작업들은 주로**  

1. props 로 받은 값을 컴포넌트의 로컬 상태로 설정  
2. 외부 API 요청  
3. 라이브러리 사용  
4. setInterval 을 통한 반복작업 혹은 setTimeout 을 통한 작업 예약  

**언마운트 시에 하는 작업들은 주로**  

1. setInterval, setTimeout 을 사용하여 등록한 작업들 clear 하기 (clearInterval, clearTimeout)  
2. 라이브러리 인스턴스 제거  

```jsx
import React, {useEffect} from 'react';

function User({ user, onRemove, onUpdate }) {
    useEffect(() => {
        console.log("컴포넌트 등장!");
        return () =>{
            console.log("컴포넌트 퇴장..");
        }
    }, []);
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
1. useEffect 를 사용 할 때에는 첫번째 파라미터에는 함수, 두번째 파라미터에는 의존값이 들어있는 배열 (deps)을 넣는다.  
**deps 배열을 비우면 컴포넌트가 처음 나타날때에만 useEffect 에 등록한 함수가 호출된다.**  

2. useEffect 에서는 함수를 반환 할 수 있는데 이를 cleanup 함수라한다.  
**cleanup 함수는 useEffect 에 대한 뒷정리 역할이며, deps 가 비어있는 경우에는 컴포넌트가 사라질 때 cleanup 함수가 호출됩니다.**  
<br />
<br />

![그림1](/assets/React/post12_useEffect_ex/1.PNG)  
**예제 실행결과**  
컴포넌트를 추가 및 삭제해보면 로그 확인 가능  
<br />

### deps관리  

deps 에 특정 값을 넣으면 지정한 값이 바뀔 때, 값이 바뀌기 직전에도 호출이 된다.  

```jsx
import React, {useEffect} from 'react';

function User({ user, onRemove, onUpdate }) {
    useEffect(() => {
        console.log("컴포넌트 등장!");
        console.log(user);
        return () =>{
            console.log("컴포넌트 퇴장..");
            console.log(user);
        }
    }, [user]);
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
실행결과.  
![그림2](/assets/React/post12_useEffect_ex/2.PNG)  
<br />

### deps 생략   

deps파라미터를 생략해버리면 컴포넌트가 리렌더링 될 때마다 호출된다.  

```jsx
import React, {useEffect} from 'react';

function User({ user, onRemove, onUpdate }) {
    useEffect(() => {
        console.log("컴포넌트 등장!");
        console.log(user);
        return () =>{
            console.log("컴포넌트 퇴장..");
            console.log(user);
        }
    });
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
실행결과.  

![그림3](/assets/React/post12_useEffect_ex/3.PNG)  