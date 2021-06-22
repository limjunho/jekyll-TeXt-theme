---
title: 비즈니스 로직(Business logic)
tags: Backend
---

## Summry  

본 문서에서는 비즈니스 로직(Business logic) 용어의 정의를 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### 정의 1

일반적으로 Database와 user interface 사이의 정보 교환을 처리하는 알고리즘을 설명하는 데 사용하는 비기술적 용어이다.

### 정의 2

business logic이란 업무에 필요한 데이터처리를 수행하는 응용프로그램의 일부를 말한다.  
이것은 데이터 입력, 수정, 조회 및 보고서 처리 등을 수행하는 루틴, 좀더 엄밀히 말하면 보이는 것의 그 뒤에서 일어나는 각종 처리를 의미한다.
대개 클라이언트 프로그램은 user interface와 business logic으로 구성되며, 서버프로그램은 대부분 business logic만으로 되어 있다.  
특히, client/server model인 경우에는 이외에도 통신링크가 추가되지만, 통신과 관련된 infrastructure는 user interface처럼 business logic의 일부는 아니다.

### 정의 3

하나의 프로젝트나 프로그램중 업무와 관련된 처리를 하는 일부분을 뜻하는데, 프로젝트를 하면서 데이터베이스에서 어떠한 자료를 가져와서 웹에서 출력을 할때 데이터베이스 연결, 통신, 자료가공, 페이지 구성등 여러가지 작업을 하지만 그 중에서 사용자가 원하는 자료의 가공 부분을 의미한다.  
예를 들어 자료를 저장할때는 부가세가 포함되지않고 자료가 저장이 되어있는데. 최종적으로 사용자에게는 부가세가 포함된 자료를 디스플레이해야 하는 업무를 처리해야 한다고 할 때 이를 처리하는 과정을 의미한다고 할 수 있다.

