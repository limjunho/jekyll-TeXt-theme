---
title: BOF/ LOB-level5(orc)
tags: LOB
---

LOB란 The Lord of the BOF의 약자로 Buffer over-flow를 연습해 볼 수 있는 해킹문제이다.    
총 20문제로 구성되어 있으며 각 문제를 격파하여 다음 레벨의 패스워드를 얻는 형식입니다.  
문제를 격파하고 다음 레벨의 bash를 획득하였다면 my-pass로 패스워드를 확인할 수 있습니다.  

 [send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---
### LOB Level 5(orc)
**ID : orc**  
**PASSWD : cantata**         

```bash
$ bash2
```
풀이 시작 전에 bash2로 꼭 변경  
**bash는 \xff를 \x00(NULL)로 인식하여 제대로 문제를 풀 수 없다.**  

![그림1](/assets/LOB/level5/1.PNG)  
사용자 ora의 권한의 ora파일이 있음을 확인할 수 있다.  
**set-uid가 걸려있으므로 사용자 ora의 권한으로 파일을 실행할 수 있다.**  
**ora파일이 쉘을 실행시키도록 한다면 사용자 ora의 쉘을 획득할 수 있다.**  

<br />
{% highlight C linenos %}  
/*
        The Lord of the BOF : The Fellowship of the BOF
        - wolfman
        - egghunter + buffer hunter
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

        // buffer hunter
        memset(buffer, 0, 40);
}
{% endhighlight %}  

**wolfman.c**

### 소스분석  
1. argc가 2보다 작다면 "ergv error"을 출력하고 프로그램을 종료한다.  
**argc란? - 파라미터의 개수를 의미한다. 즉 main에 최소 2개이상의 인자를 전달하여야 한다.**  
2. 환경변수를 memset으로 초기화  
**환경변수를 이용한 공격 불가**  
3. argv[1][47]의 값이 \xbf가 아닌경우 "stack is still your friend" 출력 후 프로그램 종료  
**RTL기법 사용 불가 What is RTL? - [LOB(level13)](https://limjunho.github.io/2020/03/14/LOB-level13(darkknight).html)**  
4. main에 전달받은 인자 argv[1](argv[0]은 파일명)을 buffer에 복사하여 출력
**strcpy함수는 복사할 데이터의 크기제한이 없기 때문에 argv[1]이 40Byte보다 크다면 buffer-overflow가 발생하는 취약점이 있다.**  
5. buffer를 memset으로 모두 0으로 초기화  
**4번의 취약점을 보완하였지만 argc가 2개 이상인 경우에 대한 보안은 없기 때문에 argv[2]를 이용할 수 있는 취약점이 있다.**  

### Solution  
**[LOB(level4)](https://limjunho.github.io/2019/11/03/LOB-level4(goblin).html)와 같이 argv[2]에 NOP과 ShellCode를 bof시켜 공격.**  

what is NOP sled? - NOP는 No-OPeration의 약자로 말 그대로 실행할 명령어가 없어 다음 명령어로 넘어간다.  
여기에 Sled(썰매)를 붙인 말로 고의적으로 실행흐름을 흘려 보내는 기법이다.  

Why use NOP Sled?  
임의의 메모리를 참조하게 되면 해당 위치에 무조건 악의적인 행위를 실행 하는 코드가 있을 수만은 없다.  
**gdb를 이용하여 디버깅을 하게 되면 한번 복사되어 들아가기 때문에 실제 프로세스 주소와 차이가 있다.**  
그래서 NOP Sled를 이용하면 RET에 변조한 주소에 악성코드가 없으면 자연스럽게 실행흐름을 타고 내려가 악성코드를 포함하고 있는 위치로 가게 되는 것이다.  

#### level5에서 사용한 쉘코드  
**\x31\xc0\xb0\x31\xcd\x80\x89\xc3\x89\xc1\x31\xc0\xb0\x46\xcd\x80\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\x31\xd2\xb0\x0b\xcd\x80**  
이 41Byte 쉘코드는 /bin/bash를 실행시키도록 하는 코드이다.  


### Attack  

[LOB(level4)](https://limjunho.github.io/2019/11/03/LOB-level4(goblin).html)**와 같은 방식으로 문제풀이**  

![그림2](/assets/LOB/level5/2.PNG)  
공격 성공.   