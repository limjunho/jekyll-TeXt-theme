---
title: C 전처리 지시어
tags: C
---

## Summry  

커널 소스를 보다보면 asmlinkage로 선언된 함수들을 볼 수 있다.  
대표적으로 시스템 콜 핸들러를 들 수 있으며 다음 해더 파일에 정의되어 있다.  

```c
// https://elixir.bootlin.com/linux/v4.14.70/source/include/linux/syscalls.h
asmlinkage long sys_fork(void);
asmlinkage long sys_exit(int error_code);
asmlinkage long sys_read(unsigned int fd, char __user *buf, size_t count);
asmlinkage long sys_write(unsigned int fd, const char __user *buf,
			  size_t count);
```

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## asmlinkage란  

asmlinkage는 어셈블리 코드에서 직접 호출(링크)할 수 있다는 의미이다.  
커널 소스의 <include/linux/linkage.h> 에 다음과 같이 정의되어 있다.  

```c
// [https://elixir.bootlin.com/linux/v4.14.70/source/include/linux/linkage.h]
#include <linux/compiler_types.h>
#include <linux/stringify.h>
#include <linux/export.h>
#include <asm/linkage.h>

#ifdef __cplusplus
#define CPP_ASMLINKAGE extern "C"
#else
#define CPP_ASMLINKAGE
#endif

#ifndef asmlinkage
#define asmlinkage CPP_ASMLINKAGE
#endif
```

## asmlinkage 사용 이유

일반적으로 C 함수는 어셈블리 코드에서 별 어려움없이 호출할 수 있지만 함수의 인자를 넘기거나 리턴값을 받는 부분 등의 호출 규약이 필요하다.  

레지스터에 여유가 있는 ARM이나 MIPS 등의 아키텍처에서는 함수의 인자를 넘길 때 특정 레지스터를 사용하도록 호출 규약이 정의되어 있지만, 그렇지 못한 x86 아키텍처에서는 때에 따라 레지스터, 스택 혹은 별도의 메모리 영역에 함수의 인자를 저장하여 넘길 수 있도록 지원한다.

인자를 레지스터에 저장하여 넘기는 방식이 빠르기 때문에(fastcall) 최적화 옵션을 켜고 컴파일하는 경우 인자를 레지스터를 통해 전달하도록 함수의 호출부와 구현부를 변경해 버릴 수 있다.(일반적인 최적화 방법)  
이 경우 GCC를 통해 자동 생성되는 코드는 적절히 변환되므로 문제가 없을테지만 직접 작성한 어셈블리 코드에서 함수를 호출하는 경우 문제가 발생하게 된다.

이 경우를 방지하기 위해 어셈블리 코드와 링크되는 함수는 인자를(레지스터를 이용하지 않고) 스택을 이용해서 전달하도록 선언하는 데, 이 때 asmlinkage가 사용된다.


## Reference

> [gcc - asmlinkage란 - rousalome](http://egloos.zum.com/rousalome/v/9986518)  
> [asmlinkage - studyfoss](https://m.blog.naver.com/PostView.nhn?blogId=sharonichoya&logNo=220507818075&proxyReferer=https:%2F%2Fwww.google.com%2F)  
