---
title: Start React project
tags: React
---

리액트 프로젝트를 제대로 작업 하려면 Node, yarn, Webpack, Babel 등의 도구를 설치하여 프로젝트를 설정.  
**What is Webpack, Babel?**  
Webpack - 여러가지의 파일을 한개로 결합하기 위한 도구  
Babel - JSX 를 비롯한 새로운 자바스크립트 문법들을 사용하기 위한 도구  

**이 포스팅은 Windows를 기준으로 작성됨**  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### 필요 항목  

**1. Node.js**  
Webpack 과 Babel 같은 도구들이 자바스크립트 런타임인 Node.js 를 기반으로 만들어졌기 때문에 Node.js를 설치  

**2. Yarn**  
npm 은 Node.js 를 설치하게 될 때 같이 설치되는 패키지 매니저 도구이다.  
Yarn npm의 개선 버전.  

**3. 코드 에디터**  
편한대로 사용하면 된다. 필자는 VSCode를 사용  

**4. Git for Windows**  
윈도우는 터미널이 없기 때문에 [Git for Windows](https://gitforwindows.org/)를 설치해서 Git bash를 이용한다.  
<br />

### Node.js install  

[Node.js](https://nodejs.org/ko/download/)에서 LTS버전 설치.  
<br />

### Yarn install  

[Yarn](https://classic.yarnpkg.com/en/docs/install#windows-stable) 운영체제에 맞는 버전 설치  
<br />

### VSCode install  

[Visual Studio Code](https://code.visualstudio.com/)에서 설치.  
<br />

### create-react-app install  

**npm으로 설치**  
```bash
$ npm install -g create-react-app
```  

**yarn으로 설치**  
```bash
$ yarn global add create-react-app
```  
<br />

### Start React  

```bash
$ create-react-app hello-react
```  
터미널에서 위의 명령 입력.  

![그림1](/assets/React/post2_start_react/1.PNG)  
위의 결과가 나왔다면 성공.  

```bash
$ cd hello-react
$ yarn start
```
위의 명령 실행시 아래의 창이 뜬다.  
![그림1](/assets/React/post2_start_react/2.PNG){: width="70% height="70%"}   
