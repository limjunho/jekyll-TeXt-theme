---
title: Blocking I/O, Syncronous Non-Blocking I/O, Asyncronous Non-Blocking I/O
tags: etc
---

## Summry  

본 문서에서는 Blocking I/O, Syncronous Non-Blocking I/O, Asyncronous Non-Blocking I/O를 정리한다.

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## Blocking/Non-Blocking

**Blocking/NonBlocking은 호출되는 함수가 바로 리턴하느냐 마느냐가 관심사다.**  
* 호출된 함수가 바로 리턴해서 호출한 함수에게 제어권을 넘겨주고, 호출한 함수가 다른 일을 할 수 있는 기회를 줄 수 있으면 Non-Blocking이다.
* 호출된 함수가 자신의 작업을 모두 마칠 때까지 호출한 함수에게 제어권을 넘겨주지 않고 대기하게 만든다면 Blocking이다.
* Blocking I/O(동기 I/O)는 I/O 작업 시 대상 파일의 디스크립터(Descriptor)가 준비되어 있지 않은 프로세스는 시스템 호출 응답 대기상태(차단 상태)가 된다. 즉, 그 동안 프로그램 처리를 진행할 수 없다.  

## Synchronous/Asynchronous

**Synchronous/Asynchronous는 호출되는 함수의 작업 완료 여부를 누가 신경쓰냐가 관심사다.**  
* 호출되는 함수에게 callback을 전달해서, 호출되는 함수의 작업이 완료되면 호출되는 함수가 전달받은 callback을 실행하고, 호출하는 함수는 작업 완료 여부를 신경쓰지 않으면 Asynchronous다.
* 호출하는 함수가 호출되는 함수의 작업 완료 후 리턴을 기다리거나, 또는 호출되는 함수로부터 바로 리턴 받더라도 작업 완료 여부를 호출하는 함수 스스로 계속 확인하며 신경쓰면 Synchronous다.
* 호출 직후 프로그램으로 부터 제어가 돌아옴으로서 시스템 호출 종료를 기다리지 않고 다음 처리로 넘어갈 수 있다. 일반적으로 O_NONBLOCK  플래그를 이용해 논-블로킹 모드를 선언하지만, 이 때 프로세스가 블로킹 상태가 아니기 때문에 CPU 를 다른 프로세스에서 사용함으로서 I/O 대기시간을 줄이거나 활용할 수 있다. 이 때 발생하는 오류는 응용프로그램에서 처리하고 재시도 하는 타이밍을 따로 정의할 필요가 있으며, 논-블로킹 I/O 소켓을 포함한 네트워크에 사용되는 I/O 모델에서는 디스크 I/O가 개입하지 않는다.

## 정리

### sync blocking I/O

* 시스템 콜이 들어오면, 커널은 IO 작업이 완료되기전에는 응답을 하지않는다.
* 즉 IO 작업이 완료되기전에는 제어권을 커널이 갖고있는다
* 그렇기에 시스템 콜을 보낸후에, 유저 프로세스는 응답을 받기 전에는 block이 되어 다른 작업을 하지못한다. **즉 IO 작업이 완료되기 전에는 다른 작업을 수행하지 못한다.**

### sync non-blocking I/O

* 시스템 콜이 들어오면, **커널은 IO 작업의 완료 여부와는 무관하게 즉시 응답**을 해준다. (완료 되지않았다면 에러코드를 응답함.)
* 이는 커널이 시스템 콜을 받자마자 제어권을 다시 유저 프로세스에게 넘겨 준다는 것이기에, **유저 프로세스는 I/O 가 완료 되기 전에도 다른 작업을 할수있다.**
* 유저 프로세스는 다른 작업들을 수행하다가 중간 중간에 시스템 콜을 보내서 I/O가 완료되었는지 커널에게 물어보게된다.

### async blocking I/O

* 이 모델에서는 IO는 non-blocking이지만, 통지(notification)는 blocking 방식으로 하도록 되어있다.
* select() 는 유저 프로세스를 block 한다.
* select() 는 데이터가 사용이 가능해지면 통지를 받게되고, 통지를 받으면 block을 푼다.

### async non-blocking I/O:

* 시스템 콜이 들어오면, **커널은 I/O 작업의 완료 여부와는 무관하게 즉시 응답**을 해준다.
* **유저 프로세스는 I/O 가 완료 되기 전에도 다른 작업을 할수있는 것이다.**
* *I/O 처리는 백그라운드에서 실행되다가, 완료되면 커널이 유저 프로세스에게 알려준다.*
    * ***이는 sync non-blocking IO와의 차별점이라고 볼수있다.***
    * sync nonblocking IO는 유저 프로세스가 I/O 완료 여부를 커널에게 계속 물어봐야 했지만, async non-blocking I/O는 I/O 완료가 되면 그때 커널이 유저프로세스에게 알려주는 방식이다.

## 정리

### blocking vs non-blocking

* blocking/non-blocking은 호출되는 함수가 바로 리턴하느냐 마느냐가 관심사이다.
* blocking: 바로 리턴 하지 않는다.
* non-blocking: 바로 리턴 한다.

### sync vs async

* sync/async는 호출되는 함수의 작업 완료 여부를 누가 신경쓰냐가 관심사이다.
* 동기(sync): 호출되는 함수의 작업 완료 여부를, 호출 하는 함수가 신경쓴다.
* 비동기(async): 호출되는 함수의 작업 완료 여부를, 호출 되는 함수가 신경쓴다.

### non-blocking vs async

* non-blocking async는 각 개념이 바라보는 관점이 다르기 때문에 비교 되상이 되기 어렵다.

## Reference

> [[카카오 면접] Blocking I/O, Syncronous Non-Blocking I/O, Asyncronous Non-Blocking I/O - 김백개발자](https://baek-kim-dev.site/38)  
> [논-블로킹(non-blocking) I/O, 비동기(Asynchronous) I/O 차이 이해하기 - GRIP.News](https://grip.news/archives/1304)  
> [blocking, non-blocking IO, 동기, 비동기 개념 정리 - limdongjin](https://limdongjin.github.io/concepts/blocking-non-blocking-io.html#ibm-%E1%84%8B%E1%85%A1%E1%84%90%E1%85%B5%E1%84%8F%E1%85%B3%E1%86%AF)  
