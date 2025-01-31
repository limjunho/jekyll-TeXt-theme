---
title: Linux discriptor
tags: Linux
---

## Summry  

리눅스에서는 기본적으로 모든 것을 파일 단위로 처리한다. 즉 소켓,디바이스,폴더 등등 모든 객체를 파일로 관리하며 리눅스 시스템에서 프로세스가 이 파일들에 접근 할때 파일 디스크립터를 이용해서 접근하게 된다.

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## file discriptor(fd) 

- 리눅스에서 파일을 open 함수로 열면 파일 디스크립터 변환
- 프로그램이 파일을 엑세스 할 때 파일 디스크립터 사용
- 리눅스에서는 모든파일, 하드웨어장치, 파이프, 소켓 등을 파일로 취급
- 파일 디스크립터 테이블 : 파일을 오픈시 시스템에서 파일 정보를 담은 구조체 할당,
    테이블의 인덱스 값이 파일 디스크립터

## Socket discriptor(sd)

- 소켓번호, 소켓을 만들고 얻은 파일 디스크립터
- 프로그램이 네트워킹을 할 때 소켓 디스크립터 사용  

리눅스상에서 파일 디스크립터를 사용하여 소켓을 열면 그것을 소켓 디스크립터라 부른다.
개념상으로는 파일 디스크립터와 같은 의미이다.

소켓디스크립터는 단순하게 리눅스 시스템이 소켓에 접근할 수 있는 매개체 정도로 이해하면 될 것 같다.

## Reference

> [소켓이란? what is socket - 코딩히어로](https://qmffor09.tistory.com/8)