---
title: Android Handler
tags: Android
---

안드로이드에서 UI 작업은 worker thread가 아닌 main thread 에서 해야 한다. 만약 로직상 worker thread 내에서 UI 처리를 해야 한다면 해당 thread와 main thread를 이어주는 핸들러(handler)를 이용하고 있을 것이다. 

 [send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### 왜 UI 작업은 Main Thread에서 하는가?  

* 여타 비동기 작업과 마찬가지로 **UI 작업을 비동기적으로 처리한다면 반드시 동기화 문제에 마주치게 된다.** 안드로이드는 이러한 문제 발생을 예방하기 위해서 병렬로 동작하는 main thread와 worker thread 사이에 핸들러를 두고 UI 작업은 모두 메인 스레드로 전달하도록 한 것이다.

**예를 들어 하나의 textView에 main thread와 worker thread 동시에 setText를 하려고 하면 문제가 된다.**  
* 이런 문제를 해결하기 위해서 worker thread는 핸들러를 사용한다. 핸들러의 역할을 간단히 말하면 메시지를 전달하는 것이다. worker thread에서 핸들러는 main thread로 메시지를 전달한다. 그러면 메시지를 수신한 main thread에서 적절한 작업을 처리하는 것이다.

### Main Thread에서 무거운 작업은 지양하자.

* 안드로이드는 메인 스레드에서 UI 작업을 처리한다. 그렇기 때문에 **시간이 오래 걸리는 작업을 메인 스레드에서 하게 된다면 UI 처리가 늦게 이뤄지게 된다.** 이것은 반응성이 떨어트리게 되고 사용자는 답답함을 느끼게 될 것이다.  

### Handler

* 핸들러는 핸들러 객체를 만든 스레드와 해당 스레드의 Message Queue에 바인딩된다. Message Queue는 핸들러가 전달하는 message를 보관하는 FIFO(First In First Out) 방식의 큐이다.

* 다른 스레드에게 메시지를 전달하려면 수신 대상 스레드에서 생성한 핸들러의 post나 sendMessage 등의 함수를 사용해야 한다. 그래야 수신 대상 스레드의 Message Queue에 message가 저장되기 때문이다.

* Message Queue에 저장된 message나 runnable은 Looper가 차례로 꺼내서 핸들러로 전달한다. 핸들러가 힘들게 넣은 것을 다시 핸들러에게 전달하는 이유는 해당 message나 runnable을 처리하기 위해서이다.  


### Handler 주요 함수

1. Handler.sendMessage(Message msg) - Message 객체를 message queue에 전달하는 함수

2. Handler.sendEmptyMessage(int what) - Message의 what 필드를 전달하는 함수

3. Handler.post(new Runnable()) - Runnable 객체를 message queue에 전달하는 함수

등등.. 

[Handler 예제 코드](https://github.com/limjunho/Android/tree/master/Handler_thread_ex)