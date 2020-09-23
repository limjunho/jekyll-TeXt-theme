---
title: Start html
tags: html&css
---

**html은 웹 페이지를 위한 언어로 HyperText Markup Language의 약자이다.**  
웹사이트에서 흔히 볼 수 있는 htm이나 html확장자가 이 언어로 작성된 문서이다.

**html 구성요소**  
요소(Elements): 시작 태그와 종료 태그로 이루어진 모든 명령어.  
태그(Tag): 요소의 일부, 시작 태그(요소를 시작)와 종료 태그(요소를 끝냄)가 있음.  
속성(Attributes): 명령을 구체화 시키는 것으로 요소의 시작태그 안에서 사용됨.  
변수(Arguments): 속성과 관련된 값.

**이 구성요소들을 모두 합쳐 만든것이 html**

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### start html

**1. 빈 파일 생성(html확장자)**

**2. 코드에디터로 편집(VSCode 추천)**  
![그림1](/assets/html_css/start_html/1.PNG)  
**자동완성에서 html:5 선택시 자동으로 초기코드 생성**  
5는 버전임.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body></body>
</html>
```

html을 기본적으로 hede와 body로 나뉜다.  
**<head> : html문서의 메타데이터(metadata)를 정의**  
메타데이터(metadata)란 html 문서에 대한 정보로 웹 브라우저에는 직접적으로 표현되지 않는 정보를 의미한다.

**<body> : 웹 브라우저를 통해 보이는 부분**  
실제로 웹 브라우저에 보이는 부분은 <body>여기에 코딩한다.</body>

**html에서는 대소문자를 구분하지 않으며 종료태그엔 /를 붙인다.**  
**주석은 <!-- 주석 내용 -->으로 표기한다.**

### 매우 기본적인 태그(tags)

**<!DOCTYPE> : 현재 문서가 HTML 문서 타입을 명시한다. (HTML5 문서 타입은 <!DOCTYPE html> 이다.)**  
**<html> : HTML 문서의 루트(root) 요소를 정의한다.**  
**<title> : HTML 문서의 제목(title)을 정의하며, 다음과 같은 용도로 사용된다.**

1. 웹 브라우저의 툴바(toolbar)에 표시된다.
2. 웹 브라우저의 즐겨찾기(favorites)에 추가할 때 즐겨찾기의 제목이 된다.
3. 검색 엔진의 결과 페이지에 제목으로 표시된다.  
   **<h1> ~ <h6> : 제목(heading)을 나타낸다.**  
   **<p> : 단락(paragraph)을 나타낸다.** `

[MDN web docs](https://developer.mozilla.org/ko/docs/Web/HTML) **html에 대한 자료는 여기서 참고**  
요소들의 대한 정보를 모두 여기에서 볼 수 있음.

### 사용자의 정보를 입력받는 예제

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>User Infomation</title>
  </head>

  <body>
    <header>
      <h1>Create An Account</h1>
    </header>
    <main>
      <form>
        <span>
          First Name
          <input
            type="text"
            id="first_name"
            placeholder="First Name"
            required
          />
        </span>
        <br />

        <span>
          Last Name
          <input type="text" id="last_name" placeholder="Last Name" required />
        </span>
        <br />

        <span>
          Email Name
          <input type="email" id="eamli" placeholder="Email Name" required />
        </span>
        <br />

        <span>
          User Name
          <input type="text" id="user_name" placeholder="User Name" required />
        </span>
        <br />

        <span>
          PassWord
          <input
            type="password"
            id="PassWord"
            placeholder="PassWord"
            required
            minlength="10"
          />
        </span>
        <br />

        <span>
          Birth Day
          <input type="date" id="calender" />
        </span>
        <br />

        <span>
          How happy are you?
          <input type="range" id="range" />
        </span>
        <br />

        <span>
          What is fav.color?
          <input type="color" id="color" />
        </span>
        <br />

        <input type="submit" id="button" value="Create Account" />
      </form>
    </main>
  </body>
</html>
```

![그림2](/assets/html_css/start_html/2.PNG)  
간단히 유저의 정보를 받는 예제

![그림3](/assets/html_css/start_html/3.PNG)  
![그림4](/assets/html_css/start_html/4.PNG)  
![그림5](/assets/html_css/start_html/5.PNG)  
**위와같이 입력을 검사**
