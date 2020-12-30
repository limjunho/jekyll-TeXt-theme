---
title: React Router를 이용한 페이지전환(주소라우팅)
tags: React
---

**이 포스팅에서는 React Router를 이용해 URL에 따라 다른 결과물을 보여주는 방법을 설명한다.**  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### install React Router

```bash
$ npm install --save react-router
```

```es6
import { BrowserRouter, Route, Redirect, Switch } from "react-router-dom";
```
위의 코드를 Router.js 상단에 삽입.  

### Route 할 컴포넌트 만들기  
각자에게 필요한 코드를 구현할 것.  

### Link걸기  
```javascript
function NoticePage() {
  return (
    <Container>
      <Header />
      <Inner>
        <TableTop>
          <Title> 공지사항 관리 </Title>
          <BtnDiv>
            <Buttons>편집</Buttons>
            <Link to="/write">
              <Buttons>글쓰기</Buttons>
            </Link>
          </BtnDiv>
        </TableTop>
        <TableList>
          <thead>
.
.
.
.
```
**위의 코드는 글쓰기 버튼을 눌렀을 때 /write로 이동하도록 링크를 거는 코드이다.**  
html의 a태그와 비슷한 방법으로 사용된다.  

### Router 설정하기  
```javascript
import React from "react";
import { BrowserRouter, Route, Redirect, Switch } from "react-router-dom";

import LoginPage from "./sign/LoginPage";
import MainPage from "./main/mainpage/MainPage";

import NoticePage from "./notice/NoticePage";
import WritePage from "./notice/WritePage";

/*
 * path 는 URL 이다.
 * 사용자가 이벤트 발생 시 이에 해당하는 컴포넌트로 이동한다.
 */

function Routers(authenticated) {
  return (
    <BrowserRouter>
      <>
        <Switch>
          <Route path="/" exact component={LoginPage} />

          <Route path="/main" exact component={MainPage} />

          <Route path="/notice" component={NoticePage} />
          <Route path="/write" component={WritePage} />

          {/* 사용자가 잘못된 경로로 접근 시 메인페이지로 리다이렉트한다. */}
          <Redirect from="*" to="/" />
        </Switch>
      </>
    </BrowserRouter>
  );
}

export default Routers;
```
router.js  
**위의 코드는 각 컴포넌트에 경로(ULR을 지정해주는 코드이다.**  

### 정리  
react router를 이용해 URL에 따라 다른 결과물을 보여주기 위해서는  
1. react-router 설치  
2. route할 컴포넌트를 만든다.  
3. 버튼 등에 Link를 걸어 이동할 경로(URL) 지정해준다.  
4. router.js에서 경로(URL)과 컴포넌트를 연결해준다.  