---
title: 시스템 콜(System Call)
tags: OperratingSystem
---

## Summry  

운영체제에서 프로그램이 구동되는데 있어 파일을 읽어 오거나, 파일을 쓰거나, 혹은 화면에 메시지를 출력하는 등 많으 부분이 커널 모드를 사용한다. 시스템 콜은 이러한 커널 영역의 기능을 사용자 모드가 사용 가능하게, 즉 프로세스가 하드웨어에 직접 접근해서 필요한 기능을 사용할 수 있게 해준다.  

**보통 C나 C++과 같은 고급 언어로 작성된 프로그램들은 직접 시스템 호출(System Call)을 사용할 수 없기 때문에 고급 API를 통해 시스템 호출에 접근하게 하는 방법이다.**

통상적으로 시스템 콜은 여러 종류의 기능으로 나뉘는데 각 시스템 콜에는 번호가 할당되고 시스템 콜 인터페이스는 이러한 번호에 따라 인덱스 되는 테이블을 유지된다. 

본 문서에서는 System Call 의 유형과 기능을 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## System Call 이해

**System Call 은 사용자 프로세스가 소프트웨어 인터럽트를 통해 커널의 기능을 이용하기 위한 서비스를 요청하는 하나의 방법이다.**  

**처리방식**  
1. 사용자 프로세스가 시스템 콜을 요청하면 제어가 커널로 넘어옴(사용자 모드 -> 커널 모드)
2. 커널은 내부적으로 각각의 시스템 콜을 구분하기 위해 기능별로 고유번호를 할당하고 그 번호에 해당하는 제어루틴을 커널 내부에 정의
3. 커널은 요청받은 시스템 콜에 대응하는 기능번호를 확인
4. 커널은 그 번호에 맞는 서비스 루틴을 호출
5. 서비스 루틴을 모두 처리하고 나면 커널 모드에서 사용자 모드로 다시 전환

## System Call 기능

1. 사용자 모드에 있는 응용 프로그램이 커널의 기능을 사용할 수 있도록 한다.
2. 시스템 호출을 하면 사용자 모드에서 커널 모드로 바뀐다.
3. 커널에서 시스템 호출을 처리하면 커널 모드에서 사용자 모드로 돌아가 작업을 계속한다.

## System Call 유형

### 프로세스 제어(Process Control)

* 끝내기(end), 중지(abort)
* 적재(load), 실행(execute)
* 프로세스 생성(create process)
* 프로세스 속성 획득과 설정(get process attribute and set process attribute)
* 시간 대기(wait time)
* 사건 대기(wait event)
* 사건을 알림(signal event)
* 메모리 할당 및 해제 : malloc, free

### 파일 조작(File Manipulation)

* 파일 생성(create file), 파일 삭제(delete file)
* 열기(open), 닫기(close)
* 읽기(read), 쓰기(write), 위치 변경(reposition)
* 파일 속성 획득 및 설정(get file attribute and set file attribute)

### 장치 관리(Devide Management)

* 장치를 요구(request devices), 장치를 방출release device)
* 읽기, 쓰기, 위치 변경
* 장치 속성 획득, 장치 속성 설정
* 장치의 논리적 부착(attach) 또는 분리(detach)

### 정보 유지(Information Maintenance)

* 시간과 날짜의 설정과 획득(time)
* 시스템 데이터의 설정과 획득(date)
* 프로세스 파일, 장치 속성의 획득 및 설정

### 통신(Communication)

* 통신 연결의 생성, 제거
* 메시지의 송신, 수신
* 상태 정보 전달
* 원격 장치의 부착 및 분리

## System Call 매개변수 전송

필요한 기능이나 시스템 환경에 따라 System Call 이 발생할 때 좀 더 많은 정보가 필요할 수 있다. 그러한 정보가 담긴 매개변수를 운영체제에 전달하기 위해서는 대략 3가지 정도의 방법이 있다.  

1. **레지스터를 통한 전송** - 매개변수를 CPU 레지스터 내에 전달한다. 이 경우에 매개변수의 갯수가 CPU 내의 총 레지스터 개수보다 많을 수 있다.
	* 매개변수가 레지스터로 직접 전달되나, 레지스터 크기보다 더 많은 매개변수가 존재하므로 좋지 않은 방법
2. **블록 or 테이블을 통한 전송** - 매개변수를 메모리 내의 블록이나 테이블에 저장하고 해당 메모리의 주소가 레지스터에 전달된다.
3. **스택을 통한 전송** - 매개변수는 프로그램에 의해 스택(stack)으로 전달(push) 될 수도 있다.
	* 매개변수는 프로그램에 의해 스택에 넣어지고, 운영체제에 의해 꺼내짐

**전달되는 매개변수들의 개수나 길이를 제한하지 않기 때문에 2번 방법을 많이 사용한다.**  

## Reference

> [운영체제 04 : 시스템 콜 (시스템 호출, System Call) - Yowu](https://luckyyowu.tistory.com/133)  
> [시스템 호출 - 위키백과](https://ko.wikipedia.org/wiki/%EC%8B%9C%EC%8A%A4%ED%85%9C_%ED%98%B8%EC%B6%9C)  
> [시스템 콜(System Call) - 양상길](https://ypangtrouble.tistory.com/entry/%EC%8B%9C%EC%8A%A4%ED%85%9C-%EC%BD%9CSystem-Call)  
