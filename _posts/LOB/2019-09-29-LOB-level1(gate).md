---
title: BOF/ LOB-level1(gate)
tags: LOB
---

LOB란 The Lord of the BOF의 약자로 Buffer over-flow를 연습해 볼 수 있는 해킹문제이다.    
총 20문제로 구성되어 있으며 각 문제를 격파하여 다음 레벨의 패스워드를 얻는 형식입니다.  
문제를 격파하고 다음 레벨의 bash를 획득하였다면 my-pass로 패스워드를 확인할 수 있습니다.  

 [send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---
### LOB Level 1(gate)
**ID : gate**  
**PASSWD : gate**         

![그림1](/assets/LOB/level1/1.PNG)  
사용자 gremlin의 권한의 gremlin파일이 있음을 확인할 수 있다.  
**set-uid가 걸려있으므로 사용자 gremlin의 권한으로 파일을 실행할 수 있다.**  
**gremlin파일이 쉘을 실행시키도록 한다면 사용자 gremlin의 쉘을 획득할 수 있다.**  

<br />
{% highlight C linenos %}  
/*
        The Lord of the BOF : The Fellowship of the BOF
        - gremlin
        - simple BOF
*/

int main(int argc, char *argv[])
{
    char buffer[256];
    if(argc < 2){
        printf("argv error\n");
        exit(0);
    }
    strcpy(buffer, argv[1]);
    printf("%s\n", buffer);
}

{% endhighlight %}  

**gremlin.c**

### 소스분석  
1. argc가 2보다 작다면 "ergv error"을 출력하고 프로그램을 종료한다.  
**argc란? - 파라미터의 개수를 의미한다. 즉 main에 최소 2개이상의 인자를 전달하여야 한다.**  
2. main에 전달받은 인자 argv[1](argv[0]은 파일명)을 buffer에 복사하여 출력하는 소스  
**strcpy함수는 복사할 데이터의 크기제한이 없기 때문에 argv[1]이 256Byte보다 크다면 buffer-overflow가 발생하는 취약점이 있다.**  

### Solution  
**쉘코드를 인자로 전달하고 쉘코드의 주소를 buffer over-flow시켜 프로그램이 쉘코드를 실행하도록 공격한다.**  
what is shellcode? - 주로 기계어 코드로 이루어진 소프트웨어 취약점을 공격하기 위한 코드.  

#### level1에서 사용한 쉘코드  
**\x31\xc0\xb0\x31\xcd\x80\x89\xc3\x89\xc1\x31\xc0\xb0\x46\xcd\x80\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\x31\xd2\xb0\x0b\xcd\x80**  
이 41Byte 쉘코드는 /bin/bash를 실행시키도록 하는 코드이다.  

**ShellCode를 이용하기 위해서는 프로그램 Stack의 구조를 알아야 한다.**  

스택구조|
---|
buffer(256Byte)|
sfp(4Byte)|
ret(4Byte)| 

**SFP(Stack frame pointer) - 이전 함수의 EBP주소를 담고 있다.**  
(EBP - 스택의 가장 바닥을 가리키는 포인터)  
**RET(Return Address) - 함수가 종료시 return될 주소**  

#### 쉘코드의 주소를 ret에 덮어씌우면 main함수가 종료되며 쉘코드를 실행시킬 것이다.  

### disassemble  

**쉘코드의 주소는 buffer의 시작주소가 될 것이기 때문에 buffer의 시작 주소를 얻기 위해 디버깅을 한다.**  
권한문제가 없도록 gremlin를 복사하고 gdb를 사용하여 intel방식으로 디스어셈블 한다.  
![그림2](/assets/LOB/level1/2.PNG)  
disassemble 결과  

![그림3](/assets/LOB/level1/3.PNG)  
**buffer의 시작주소를 확인하기 위해 main함수의 종료 전에 BreakPoint를 걸고 AAAA를 인자로 전달하여 실행시킨 뒤 스택의 값을 출력해본다.**  
esp - 스택의 최상단을 가리키는 포인터  
**b - break명령, r - run명령, x/[범위]wx - 메모리 상태 검사명령(주소 범위 만큼 16진수로 4Byte단위로 출력)**   

**인자로 전달한 AAAA의 Ascii값 41414141을 확인할 수 있다. -> buffer의 시작주소 = 0xbffffa28**  

### Attack  

ShellCode의 주소와 스택의 구조를 파악하였으므로 공격을 시도할 수 있다.  
**./gremlin [shellcode] + [dummy값] + [buffer시작주소]**의 형태로 공격.  
**이 때 shellcode + dummy값의 길이는 buffer의 길이 + sfp(4byte)만큼 즉 260Byte여야 한다.**
**또한 RET에 덮을 쉘코드의 주소는 리틀 엔디안 방식으로 기입한다.**  

**리틀 엔디안은 최 하위 바이트 부터 저장하는 방식이다.**  
ex) 0xbffffc11을 리틀 엔디안 방식으로 저장하면 11cfffbf가 된다.  
보통 엔디안 방식은 CPU 아키텍쳐에 따라 다르지만 intel x86, x64, AMD 계열은 리틀엔디안, 모토로라 프로세서들은 빅엔디안을 사용한다.  
현재 시스템의 엔디안방식을 확인하고 싶다면 echo -n I | od -to2 | head -n1 | awk'{print $2;}' | cut -c6  
명령으로 나오는 값이 1이라면 리틀 엔디안, 0이라면 빅엔디안이다.  

![그림4](/assets/LOB/level1/4.PNG)  
공격이 실패하였다.   

**why?**  
gdb를 이용하여 디버깅을 하게 되면 한번 복사되어 들어가기 때문에 실제 프로세스 주소와 차이가 있다.  

{% highlight C linenos %}  
#include <stdio.h>

int main(int argc, char *argv[])
{
    char buffer[256];
    if(argc < 2){
        printf("argv error\n");
        exit(0);
    }
    strcpy(buffer, argv[1]);
    printf("%s\n", buffer);
    printf("%x\n", buffer);
}

{% endhighlight %} 

![그림5](/assets/LOB/level1/5.PNG)   
**gcc는 컴파일러로 -o옵션은 생성할 바이너리파일의 이름을 지정한다.**  
get_addr.c를 컴파일하여 생성할 파일의 이름은 get_addr로 짓도록 하는 명령  


![그림6](/assets/LOB/level1/6.PNG)   
다시 알아낸 buffer의 주소를 대입하여 공격, 성공적으로 grimlin의 bash를 획득하였다.  


**리틀 엔디안은 최 하위 바이트 부터 저장하는 방식이다.**  
ex) 0xbffffc11을 리틀 엔디안 방식으로 저장하면 11cfffbf가 된다.  
보통 엔디안 방식은 CPU 아키텍쳐에 따라 다르지만 intel x86, x64, AMD 계열은 리틀엔디안, 모토로라 프로세서들은 빅엔디안을 사용한다.  
현재 시스템의 엔디안방식을 확인하고 싶다면 echo -n I | od -to2 | head -n1 | awk'{print $2;}' | cut -c6  
명령으로 나오는 값이 1이라면 리틀 엔디안, 0이라면 빅엔디안이다.  
