---
title: C Thread example
tags: C
---

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---
### Process?  

사전적 의미로는 컴퓨터에서 연속적으로 실행되고 있는 컴퓨터 프로그램이며 운영체제로부터 시스템 자원을 할당받는 작업의 단위이다.  

**할당받는 시스템 자원 예**  
* CPU 시간
* 주소 공간  
* 메모리 공간  

기본적으로 프로세스당 최소 1개의 스레드(Main thread)를 가진다.  
또한 각각 독립된 주소공간에서 실행되기 때문에 다른 프로세스의 변수나 자료구조에 접근할 수 없다.  
* 접근을 위해서는 프로세스간 통신(IPC, inter-process communication)을 이용할 수 있다.  
    * ex) 파이프, 파일, 소켓 등을 이용하여 통신  

### Thread?  

사전적 의미로는 프로세스 내에서 실행되는 여러 흐름의 단위이다.  
스레드는 프로세스 내에서 Stack만을 따로 할당받고 Code, Data, Heap 영역은 공유한다. 따라서 프로세스 내의 주소 공간이나 자원들을 같은 프로세스 내에 스레드끼리 공유하며 실행된다.  

### 사용 이유  

**1. 컴퓨팅 자원 분할**     
1초에 1씩 총 1000을 카운트하는 프로그램이 있고 1개의 Main Thread만을 사용한다고 가정하면 1000을 카운트하는데는 총 1000초의 시간이 걸릴 것이다.  
2개의 Thread를 사용한다면 각각 500초의 시간동안 500을 카운트하고 1000을 카운트하는데 총 500초의 시간이 걸릴 것이다.  

**2. 프로세스간 통신(IPC)의 부담**  
프로세스간 통신(IPC) 비용 보다 하나의 프로세스 내에서 여러 스레드간의 통신 비용이 훨씬 저렴하다.

### 주의사항  

* 멀티 프로세스 구조에서 여러 개의 자식 프로세스 중 하나에 문제가 발생하면 자식 프로세스 하나만 죽는다 해서 다른 곳에 영향을 끼치지는 않는다.  
* 멀티 스레드 구조에서 자식 스레드중 하나에 문제가 생긴 경우에는 전체 프로세스가 영향을 받게 된다.  
* 멀티 스레딩을 과도하게 사용하면 오히려 시스템 성능의 저하를 초래할 수 있다.  
* 메모리가 공유되기 때문에 안정성 및 보안을 좀 더 추구하는 경우에는 멀티 스레드보단 멀티 프로세싱이 더 좋다.  

### example code  

아래 코드는 1번 스레드는 3초에 한번 카운트, 2번 스레드는 1초에 한번 카운트하도록 동작한다.  

```python
import time
import threading
from threading import Thread

def thread_function():
    i = 0
    while(True):
        print("thread_function("+str(i)+")")
        i += 1
        time.sleep(3)

if __name__ == "__main__":
    thread = threading.Thread(target=thread_function, args=())
    thread.start()

    i = 0
    while(True):
        print("main thread("+str(i)+")")
        i += 1
        time.sleep(1) 
```