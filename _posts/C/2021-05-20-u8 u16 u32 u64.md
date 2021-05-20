---
title: u8, u16, s8, s16
tags: C
---

## Summry  

C 코드에서 u8, u16, u32, u64 또는 s8, s16, s32, s64 라는 자료형을 보았을 것이다.  
본 문서에서는 위의 자료형을 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## u, s 의미

u8, u16, u32, u64, s8, s16, s32, s64는 각각 unsinged 8bit, 16bit, 32bit, 64bit 그리고 singed 8bit, 16bit, 32bit, 64bit이다.  

각 OS(architecture) 마다 재정의된 내용이며, 이를 사용하는 목적은 int, float, double 등의 자료형은 architecture 마다 크기가 틀려질 수 있기 때문에 이를 방지하고 원하는 크기의 자료형을 사용하기 위해 명시적으로 쓰이는 자료형이다.

## compiler 호환성 체크

```c
if (sizeof(u8) != 1 || sizeof(u16) != 2 || sizeof(u32) != 4 || '\0' != 0){
   fprintf(stderr, "%s: compiler incompatibility\n", argv[0]);
   exit(255);
}
```  

## Reference

> [u8 u16 s8 s16 자료형 - jangpro](https://blog.naver.com/jangpro1003/90099447543)  