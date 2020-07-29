---
title: Android LifeCycle(생명주기)
tags: Android
---

Android Programming을 할 때 알아야 할 LifeCycle(생명주기)에 대한 정리글이다.  

<br />

**정의**  
안드로이드에서의 생명 주기는 Activity(화면)이 실행되고 종료될 때까지의 전 과정을 뜻한다.  

 [send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

![그림1](/assets/Android/LifeCycle/1.png)  
위의 그림이 생명주기로 위에서 아래가 순서이고 가장 마지막의 shut down이 어플리케이션의 종료이다.  

파란색 박스가 메소드들인데 크게 3가지로 구분하면 실행전, 종료전, 재시작으로 구분할 수 있다.  
실행전 - onCreate(), onStart(), onResume()  

**onCreate()**   
가장 먼저 호출되는 메소드로 반드시 구현되어야 하는 메소드이다.  
화면이 사용자에게 보여지기 전에 호출되는 메서드이기 때문에 Activity의 필수 구성 요소(Button, Layout등) 초기화하는 코드들을 작성  

**onStart()**  
onCreate()와 마찬가지로 화면이 보여지기 전에 호출되는 메서드이나 반드시 구현하지 않아도 되는 메소드이다.  
Activity를 사용자에게 보여줄 준비가 되었을 때 호출한다. 주로 Activity를 보여주기 위한 리소스를 설정  
개발자의 기획이나 코드작성 스타일에 따라 구현 또는 미구현.  

**onResume()**  
Activity가 화면의 포그라운드에 있고 사용자에게 포커스를 가지고 있는 상태이다. 즉 실행중인 상태.  
사용자와 상호작용할 수 있는 코드들을 주로 작성. ex) 버튼 클릭, 토스트메시지  

<br />
종료전 - onPause(), onStop(), onDestroy()  
Activity가 종료되는 경우는 여러가지 이유가 있는데 사용자가 홈버튼을 누르거나 어플리케이션을 종료시키거나 화면을 다음 화면으로 이동하여 다음 화면에 덮혀지는 경우들이 있다.  

**onPause()**  
화면을 떠난다는 첫 번째 신호인 메소드이다.  
어플리케이션을 종료하였을 때는 화면이 소멸되겠지만 홈버튼을 누르거나 다음 화면으로 이동하는 경우에는 Activity가 소멸되지 않는다.  
따라서 그 화면에서의 변경사항을 미리 저장하는 작업을 이 메소드에서 다룬다.  
(소멸되진 않았지만 만약 메모리상에서 문제가 발생한다면 시스템에서 종료시키기도 한다.) - Processse is killed  

**onStop()**  
내가 보고 있던 화면1(Activity)가 화면2(Activity)에 완전히 가려진 상태(Activity가 소멸되지 않음).  
포그라운드가 아닌 백그라운드의 상태이기 때문에 뒤로가기 버튼을 누르는 경우 화면2에서 화면1로 돌아갈 수 있다.  
이 경우 Activity가 다시 사용자와 상호작용을 하게되며 (재시작)onRestart()가 실행되고 onStart()메소드로 돌아간다.  

**onDestroy()**  
화면을 종료시키거나 어플리케이션을 강제종료 시키거나 프로세스를 강제종료시키는 등의 경우 호출되는 메소드이다.  
액티비티가 받는 가장 마지막 호출이다.  

각 메소드들이 언제 호출되는지 확인하기 위해 Log와 Toast  message를 띄우는 예제  
[LifeCycle 예제](https://github.com/limjunho/Android/tree/master/LifeCycle_ex) click this
위의 그림을 보면서 뒤로가기, 홈버튼 등등 여러 동작을 수행해보면 이해하기 쉽다.  

**화면을 가로방향 세로방향을 바꾸면 onDestroy가 호출되고 다시 onCreate가 호출된다.**  