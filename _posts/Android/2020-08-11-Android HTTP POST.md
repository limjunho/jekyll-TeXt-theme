---
title: Android HTTP POST
tags: Android
---

이번 포스팅에서는 안드로이드로 HTTP server에 POST방식으로 요청본문(body)와 쿼리스트링(queryString)을 전송하는 예제를 설명한다.  

**예제의 용도**  
Node.js로 웹서버를 열어 HTTP POST방식으로 안드로이드로부터 요청을 수신후 안드로이드로부터 전달받은 쿼리스트링을 DB에 삽입하는 용도로 사용하였음.

[클라이언트 요청 수신](https://limjunho.github.io/2020/08/11/Node.js-client-request-receive.html)에서 쿼리스트링을 사용.  
[node.js mysql](https://limjunho.github.io/2020/08/11/Node.js-Mysql.html)에서 node.js로 mysql에 연동 및 데이터 입/출력. 
[queryString into DB](https://limjunho.github.io/2020/08/11/Node.js-queryString-into-DB.html)에서 쿼리스트링을 DB에 삽입.  
  
 [send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### UI 및 권한

**UI**  
![그림1](/assets/Android/HTTP_POST/1.PNG)  
SEND DATA버튼을 클릭할 경우 코드상에서 작성된 파라미터를 URL에 전달하여 쿼리스트링을 전달.  

**권한**  
![그림2](/assets/Android/HTTP_POST/2.PNG)  
안드로이드 앱에서 인터넷에 접속할 수 있도록 하는 권한이다.  

### 예제 소스코드  
[예제소스](https://github.com/limjunho/Android/tree/master/HTTP_POST_ex)
