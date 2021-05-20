---
title: Linux device driver
tags: Linux
---

**Device와 Device Driver**  
* Device란 네트워크 어댑터, LCD 디스플레이, 오디오, 터미널, 키보드, 하드디스크, 플로피디스크, 프린터 등과 같은 주변 장치들을 말한다.
* Device는 Device의 구동에 필요한 프로그램, 즉 Device Driver가 필수적으로 요구됨

 [send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## Device Driver  

* 실제 장치 부분을 추상화시켜 사용자 프로그램이 정형화된 인터페이스를 통해 디바이스를 접근할 수 있도록 해주는 프로그램  
* 디바이스 관리에 필요한 정형화된 인터페이스 구현에 요구되는 함수와 자료구조의 집합체  
* 표준적으로 동일 서비스 제공을 목적으로 커널의 일부분으로 내장
* 응용 프로그램이 하드웨어를 제어할 수 있도록 인터페이스 제공
* 하드웨어 독립적인 프로그램을 작성할 수 있도록 함.

### Linux Device Driver  

리눅스에서는 device를 하나의 파일로 인식하여 파일에 대한 접근을 통해 하드웨어에 접근이 가능하다.  

### 문자 드라이버(char)  

* device를 파일처럼 접근하여 직접 read/write 수행
* data 형태는 stream 방식으로 전송
* (ex) console, keybord, serial port driver 등

**문자 디바이스(char device) 특징**  
* 자료의 순차성을 지닌 장치
* 버퍼 캐쉬를 사용하지 않음
* 장치의 raw data를 사용자에게 제공
* Terminal, Serial/Parallel, Keyboard, Sound Card, Scanner, Printer 등

### 블록 드라이버(block)  

* disk와 같은 file system을 기반으로 block 단위로 데이터를 read/write
* (ex) hard disk, CD-ROM driver, floppy disk

**블록 디바이스(block device)**  
* random access 가능
* 블록 단위의 입출력이 가능한 장치
* 버퍼 캐쉬에 의한 내부 장치 표현
* 파일 시스템에 의해 mount 되어 관리되는 장치
* 디스크, RAM Disk, CD-ROM등

### 네트워크 드라이버(network)  

* network의 물리계층과 frame 단위의 데이터 송수신
* (ex) Ethernet device driver

**네트워크 디바이스(network device)**  
* 대응하는 장치파일이 없음
* 네트워크 통신을 통해 패킷을 송수신 할 수 있는 장치
* 응용프로그램과의 통신은 표준 파일 시스템 관련 호출 대신에 socket()이나 bind() 등의 시스템 호출
* Ethernet, PPP, ATM, ISDN 등이 있다