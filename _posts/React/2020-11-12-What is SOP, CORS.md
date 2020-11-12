---
title: What is SOP, CORS??
tags: React
---

![그림1](/assets/React/What_is_SOP,_CORS/1.PNG) 
**웹을 개발하다보면 한번쯤 겪게되는 에러라고 한다. 이 에러에 대해 알아보자.**  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### 출처(Origin)란?  

![그림2](/assets/React/What_is_SOP,_CORS/2.PNG)  
위와 같은 Url에서 Protocol과 Host, Port가 같다면 같은 출처라고 본다.  

**기준 출처**  
https://limjunho.github.io  
<br />

**같은 출처**  
https://limjunho.github.io/about.index  
**-> Protocol이나 Port가 생략되어 있지만 Host까지 모두 동일하므로 같은 출처이다.**  

https://limjunho.github.io:443/about.index?test=test  
**-> http나 https에 대한 port는 생략이 가능하기 때문에 Protocol, Host, Port가 같아 같은 출처이다.**  

http://limjunho.github.io:8080  
**-> Port가 다르나 현재 IE를 제외한 대부분의 브라우저에서 Port번호가 다른것을 같은 출처로 인정한다.**  
<br />

**다른 출처**  
http://limjunho.github.io  
**-> Protocol이 다르기 떄문에 다른 출처이다.**  

https://github.com/limjunho  
**-> Host가 다르기 때문에 다른 출처이다.**  

### SOP  

**SOP(Same Origin Policy)**  
동일 출처 정책  

웹의 경우는 오픈되어 있기 때문에 좋은 리소스도 있지만 나쁜 리소스도 있기 때문에 보안을 위해(악성 요청등을 방지) 같은 출처인 경우에만 리소스를 받도록 하는 정책이다.  
http나 https 통신을 하게되면 위의 정책을 지키게 된다.  

**하지만 하나의 홈페이지에 여러 정보를 보여주기 위해(웹의 정보들을 활용하기 위해) 예외조항이 있는데 CORS가 그것들중 하나**  

### CORS

**CORS(Cross Origin Resource Sharing)**  
교차 출처 자원 공유 - **모든 출처의 자원을 받는 것은 보안상 취약하기 때문에 원하는 출처의 자원만 추려받는 정책**  


**Origin == Access-Control-Allow-Origin**  
Request에는 Origin이라는 헤더가 있고 Response에는 Access-Control-Allow-Origin이라는 헤더가 있다.  
위 두개의 헤더가 같다면 같은 출처로 브라우저가 인식한다.  

**CORS는 서버나 클라이언트가 아닌 브라우저에서 판단**  

### 결론  

**서버 개발자가 프론트 개발자를 위해 응답 헤더에 올바른 Access-Control-Allow-Origin이 내려올 수 있도록 세팅해 주어야 한다.**  
쉽게말해 프론트 개발자가 아닌 서버 개발자가 이 오류를 해결해준다.  

예외) Webpack Dev Server로 리버스 프록싱 하여 우회 가능하나 로컬환경에서만 가능하고 여러 제약이 있다.  
가장 좋은것은 서버 개발자에게 요청하는것  

**참고**  
https://www.youtube.com/watch?v=7iGIfcEsc2g&t=7s&ab_channel=%EC%9A%B0%EC%95%84%ED%95%9CTech