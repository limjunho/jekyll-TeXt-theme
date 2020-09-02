---
title: Android DownloadManager
tags: Android
---

DownloadManager를 이용해 HTTP파일을 다운로드 받는 방법  

 [send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### DownloadManager?  

**앱에 저장될 위치의 URI와 다운받을 HTTP URI를 DownloadManger에게 알려주면 알아서 다운로드해준다.**  
**DownloadManager는 백그라운드에서 다운로드를 받기 때문에 앱에서 쓰레드를 생성할 필요가 없다.**  
**노티피케이션에 상태를 보여주므로 개발자가 따로 노티피케이션을 구성하지 않아도 된다.**  
**불안정한 네트워크 상태에 대한 예외처리가 되어있다.**  


### UI 및 권한  

**UI**  
![그림1](/assets/Android/DownloadManager/1.jpg){: width="50% height="50%"}  
uri를 입력하여 다운로드 버튼을 클릭하면 다운로드를 시작한다.  

**권한**  
![그림2](/assets/Android/DownloadManager/2.PNG)  
안드로이드 앱에서 인터넷에 접속할 수 있도록 하는 권한과 storage권한.    
**usesCleartextTraffic을 true로 설정 -> 안전하지 않은 URL을 실행가능**  

### 예제 소스코드  
[예제소스](https://github.com/limjunho/Android/tree/master/DownloadManager_ex)

**DownloadManager는 다운로드 요청이 완료되면 브로드캐스트로 결과를 전달하기 때문에 브로드캐스트 리시버를 구현해야 한다.**  

### 참고자료  
[developers](https://developer.android.com/reference/android/app/DownloadManager#COLUMN_LOCAL_URI)  
[codechacha_github](https://github.com/codechacha/DownloadManager)  
[codechacha_blog](https://codechacha.com/ko/android-downloadmanager/)  