---
title: React Developer Tools(React 개발도구)
tags: React
---

**React Developer Tools로 리렌더링을 확인하는 예제**

[React Array Example코드](https://github.com/limjunho/React/tree/master/array_ex)  
예제 소스코드(github)

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### React Developer Tools install

[React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi/related)에서 크롬 확장 프로그램 설치

**chrome://extensions/ -> React Developer Tools 세부정보-> 파일 URL에 대한 엑세스 허용**  
![그림1](/assets/React/React_DevTools/1.PNG)

### 사용

![그림2](/assets/React/React_DevTools/2.PNG)  
F12를 눌러 개발자 도구를 키면 React탭이 새로 생긴것을 확인.  
<br />

![그림3](/assets/React/React_DevTools/3.PNG)  
 Highlight updates when components render. 기능을 켜면 리렌더링 되는 컴포넌트에 사각형 형태로 하이라이트되어 보여진다.

이제 컴포넌트가 리렌더링 될 때 마다 쉽게 눈에 보이도록 박스가 쳐지는 모습을 볼 수 있다.

### Prettier(코드 스타일 관리 툴) install

Prettier 는 자동으로 코드의 스타일을 관리해주는 도구이다.  
**문자열을 사용 할 때 ' 또는 " 를 쓸지, 세미콜론(;)을 코드 뒤에 붙일지 말지, 들여쓰기 칸수, 이런 것들을 관리할 수 있다.**

**1. Visual Studio Code에서 Prettier를 설치**

![그림4](/assets/React/React_DevTools/4.PNG)

**2.(Ctrl + ,)를 눌러서 format on save를 검색하여 체크.**

![그림5](/assets/React/React_DevTools/5.PNG)

**3. 이 툴을 사용할 프로젝트의 루트 디렉터리에 .prettierrc파일을 만든다.**

```
{
  "trailingComma": "es5",
  "tabWidth": 2,
  "semi": true,
  "singleQuote": false
}
```

**.prettierrc**

**trailingComma - "none", "es5", "all" 중 하나로 설정하며, 객체 또는 배열이 여러줄로 구성되어 있으면 맨 마지막 줄에 쉼표를 붙인다.**  
**tabWidth - 들여쓰기의 크기를 설정**  
**semi - 세미콜론을 쓸지 말지 결정 사용하려면 true로 설정**  
**singleQuote - 문자열을 입력할 때 "를 쓸지 '를 쓸지 결정한다. "만 쓰려면 false로 설정**

위의 과정을 완료했다면 코드 작성 후 저장할 때 자동으로 코드 스타일 관리를 해준다.
