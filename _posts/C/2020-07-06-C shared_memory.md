---
title: C Shared memory  
tags: C
---

보통 프로세스에서 사용되는 메모리영역은 해당 프로세스만이 사용할 수 있다.  
하지만 때때로 여러개의 프로세스가 특정 메모리 영역을 사용했으면 하는 때에 사용하는 것이 공유메모리이다.

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---
### what is Shared memory? 

공유 메모리(Shared memory)는 컴퓨터 환경에서 여러 프로그램이 동시에 접근할 수 있는 메모리이다.  
이를 이용해 프로세스간 통신(IPC)을 하는 경우가 있다.  

### why use?  

1. 공유메모리는 여러 IPC 중에서 가장 빠른 수행속도를 보여준다.  
    * 하나의 메모리를 공유해서 접근하게 되므로, 데이터 복사와 같은 불필요한 오버헤드가 발생하지 않기 때문  
    * **하나의 프로세스가 메모리에 접근중에 있을때 또다른 프로세스가 메모리에 접근하는 일이 발생하면 자칫 데이터가 회손될 수 있으므로 주의가 필요하다.**  

2. 메모리 보존관리 방법으로 사용  
    * 프로세스에서 자주 사용하는 명령들이 특정 코드에 집중되어 있다. 따라서 이러한 코드 부분을 따로 분리하여, 메모리를 절약하는 것인데 이렇게 분리해놓은 것이 라이브러리(Library)이다.  
    * 라이브러리를 공유메모리 부분에 적재시켜 메모리를 아끼는 것.

### how use?   

**헤더**  
* #include <sys/ipc.h>  
* #include <sys/shm.h>  

**함수**  
* int shmget(key_t key, int size, int shmflg) 
    * shared memory 생성 또는 가져오는 함수 
    * key : 공유메모리를 할당할 떄 사용하는 고유의 값
    * size : 할당할 메모리의 크기
    * shmflg : IPC_CREAT와 IPC_EXCL 두 가지가 존재
        * IPC_CREAT : 새로운 메모리 세그먼트를 만든다. 이 flag를 사용하지 않는다면 shmget은 명시된 key와 연관된 찾고 접근할 수 있는 권한이 있는지 확인.  
        * IPC_EXCL : IPC_CREAT과 함께쓰는 플래그로 만약 메모리 세그먼트가 존재하면 shmget은 실패하게 된다.  
        IPC_EXCL을 사용하는 경우는 우선 공유메모리가 있는지 확인 후 없으면 IPC_CREAT을 통해 할당받으라는 뜻이며 이렇게 공유메모리가 오염되는 것을 방지할 수 있다.
* void *shmat(int shmid, const void *shmaddr, int shmflg )
    * process에 shared memory를 할당하는 함수 
    * shmid : shared memory id
* int shmdt(const void *shmaddr)
    * process에 할당된 shared memory를 분리하는 함수 
* int shmctl(int shmid, int cmd, struct shmid_ds *buf)
    * shared memory를 제어하는 함수

### example code  

[Github](https://github.com/limjunho/C/tree/master/shared_memory)  
make_shared_memory.c -> 공유 메모리 생성하며 파라미터로 전달받은 값을 메모리에 저장  
read_shared_memory.c -> 공유 메모리의 값을 읽음  
clear_shared_memory.c -> 공유 메모리 삭제  