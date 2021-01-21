---
title: Android Context? Intent?
tags: Android
---

Android에서 Context와 Intent가 무엇인가?  

 [send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### Context?  

* 간결하게 정리하면 Context는 어플리케이션 환경에 대한 전역 정보를 접근하기 위한 인터페이스로 **어플리케이션에 관하여 시스템이 관리하고 있는 정보에 접근하고, Activity 실행, Intent 브로드캐스팅, Intent 수신등과 같은 안드로이드 시스템에서 제공하는 API를 호출할 수 있는 기능을 제공하는 클래스**

### Context 역할  

Context는 Application이 생성될 때나 Android 각 컴포넌트들이 생성될 때 서로 다른 종류의 Context가 생성된다.  안드로이드 플랫폼 관점에서 Context는 다음과 같은 두가지 역할을 수행한다. 

1. 자신이 어떤 Application을 나타내고 있는지 Android System에 알려주는 ID 역할  
2. 안드로이드에서 어플리케이션 관련된 정보에 접근하고자 할 때 통해야하만하는 ActivityManagerService에 접근할 수 있도록 하는 통로 역할

### Context 종류  

* Application Context : **Application의 life-cycle에 접목되는 개념으로 하나의 애플리케이션이 실행되어 종료될 때까지 동일한 객체**이다. 흔히 getApplicationContext() 함수를 사용해 얻는 객체를 의미. 또 하나 getApplication()이 있는데 이는 Application 객체를 참조하지만 Application 클래스는 Context 클래스를 상속받으므로 Application Context를 제공하는데 사용할 수 있다.  

* Activity Context : **Activity life-cycle에 접목되는 개념으로 하나의 액티비티가 실행되어 종료될 때까지 즉 onDextroy() 된 경우 사라질 수 있는 객체**이다. Activity에 대한 환경 정보들이 Context에 있고, 이 Context에 Intent를 통해 다른 Activity를 띄우면 액티비티 스택이 쌓이게 된다.        


### Intent?

Intent는 일종의 메시지 객체다. 이것을 사용해 다른 앱 구성 요소로부터 작업을 요청할 수 있다. 인텐트가 여러 구성 요소 사이의 통신을 용이하게 하는 데에는 몇 가지 방법이 있지만 기본적인 사용 사례는 다음과 같은 세 가지이다.  

1. **액티비티 시작** : Activity는 앱 안의 단일 화면을 나타낸다. **Activity의 새 인스턴스를 시작하려면 Intent를startActivity()로 전달하면 된다.**  
    Intent는 시작할 액티비티를 설명하고 모든 필수 데이터를 담고 있다.

2. **서비스 시작** : Service는 사용자 인터페이스 없이 백그라운드에서 작업을 수행하는 구성 요소다. **서비스를 시작하여 일회성 작업을 수행하도록 하려면(ex: 파일 다운로드) Intent를 startService()에 전달하면 된다.**  
    Intent는 시작할 서비스를 설명하고 모든 필수 데이터를 담고 있다.  

3. **브로드캐스트 전달** : 브로드캐스트는 모든 앱이 수신할 수 있는 메시지다. 시스템은 여러 시스템 이벤트에 대해 다양한 브로드캐스트를 전달하며 예를 들어 시스템이 부팅될 때 또는 기기가 변경되기 시작할 때 등이 해당된다. **다른 여러 앱에 브로드캐스트를 전달하려면 Intent를 sendBroadcast(), sendOrderedBroadcast() 또는 sendStickyBroadcast()에 전달하면 된다.**