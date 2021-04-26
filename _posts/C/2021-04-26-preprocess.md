---
title: C 전처리 지시어
tags: C
---

## Summry  

전처리(preprocessing)란 컴파일 이전에 미리 처리되는 문장으로 전처리에 사용되는 작업 지시어를 전처리 지시어(preprocessing directive)라 한다.  

**전처리 지시어 종류**  
* 파일처리
    * #include
* 형태 정의
    * #define, #undef
* 조건 처리
    * #if, #else, #elif, #endif, #ifdef, #ifndef
* 에러 처리
    * #error 
* 디버깅
    * #line 
* 컴파일 옵션 처리
    * #pragma 

본 문서에서는 조건 처리에 대한 전처리 지시어를 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## if 와 #if 비교 

* 전처리 지시어의 경우 #endif 이렇게 닫아주는 지시어가 있다.
* 일반 조건문의 경우 괄호로 블락을 구분하지만 전처리기는 괄호를 사용하지 않는다.
* 일반 if 조건문은 false 인 경우, 실행이 되지 않을 뿐 컴파일은 되지만 #if 가 0 이라면 컴파일 자체가 되지 않는다.

아래는 예제이다.  

```c
#include <stdio.h>
int main()
{
	if (0){     //괄호로 범위 지정 
		printf("실행되지는 않지만 컴파일은 됨\n");
	}

#if 0
	printf("컴파일 자체가 안됨\n"); 
#endif
	return 0;
}
```

## #if 와 #ifdef 비교

#ifdef 의 def 는 define 의 약자이다. 즉, #if가 '만약~라면' 이라는 뜻이라면 #ifdef는 '만약 ~가 정의되어 있다면'을 의미한다.  
또한 ifndef는 if not define 으로 #ifdef 의 반대로 생각하면 된다.  
아래는 예제이다.  

```c
#include <stdio.h>
#define A 0
int main()
{
#ifdef A
	printf("A is defined\n");
#endif
//-----------------
#if A
	print("A is True\n");
#else
	printf("A is False\n");
#endif
	return 0;
}
```

실행 결과는 A 가 0의 값을 가짐에도 불구하고 A 가 정의되어 있기 때문에 "A is defined" 가 출력되게 된다.  

## #if 조건을 분기하는 #elif

#ifdef는 정의되어 있냐 정의되어 있지 않냐 이분법적으로만 나누기 때문에 #elif를 사용할 수 없지만, #if는 #elif를 사용할 수 있다.  
아래는 예제이다.  

```c
#include <stdio.h>
#define NUM 2
int main(void)
{
#if NUM==1
    printf("NUM is 1\n");
#elif NUM==2
    printf("NUM is 2\n");
#elif NUM==3
    printf("NUM is 3\n");
#else
    printf("NUM is %d\n", NUM);
#endif
	return 0;
}
```
실행 결과는 "NUM is 2" 가 출력되게 된다.  

## Reference

> [#if, #ifdef, #elif, #else, #endif 전처리기 지시어 알아보기. #if와 #ifdef 차이점이 무엇일까. 조건부 컴파일 매크로 - IT 양햄찌](https://jhnyang.tistory.com/299)  
> [C언어 전처리기(#if, #else, #endif, #ifdef, #ifndef, #undef) - SLL](https://m.blog.naver.com/PostView.nhn?blogId=sharonichoya&logNo=220507818075&proxyReferer=https:%2F%2Fwww.google.com%2F)  
