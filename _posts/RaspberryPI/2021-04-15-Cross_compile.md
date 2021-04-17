---
title: Linux Cross Compile
tags: Linux RasberryPI
---

**크로스 커널 컴파일이란?**

크로스 컴파일러(cross compiler)는 컴파일러가 실행되는 플랫폼이 아닌 다른 플랫폼에서 실행 가능한 코드를 생성할 수 있는 컴파일러이다. 크로스 컴파일러 툴은 임베디드 시스템 혹은 여러 플랫폼에서 실행파일을 생성하는데 사용된다. 이것은 운영 체제를 지원하지 않는 마이크로컨트롤러와 같이 컴파일이 실현 불가능한 플랫폼에 컴파일하는데 사용된다. 이것은 시스템이 사용하는데 하나 이상의 플랫폼을 쓰는 반가상화에 이 도구를 사용하는 것이 더 일반적이게 되었다.

* 크로스 컴파일러를 사용하여 타겟용 kernel 소스를 컴파일하여 타겟용 Kernel을 추출해내는 것

* Image file이란?
	: Window에서는 .iso라는 확장자를 가진 파일을 말하며 CD/DVD를 압축해놓은
	파일이라고 할 수 있다. CD/DVD-ROM이라는 HW가 없어도 가상 드라이브를 통
	해 CD/DVD를 실행 가능하게 해주는 파일이다.

**크로스컴파일러를 이용해 타겟머신의 Kernel Image file을 생성해본다.**  
* 타겟 머신 : RaspberryPi4 B  

 [send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## tools install

```bash
$ sudo apt install make
```
* Makefile을 참조하여 빌드하는 명령어  
* Makefile은 컴파일시 다양한 옵션을 정리해놓은 파일로 쉘스크립트로 구성됨.  

```bash
$ sudo apt install git
```
* 외부(다른서버) 저장소에 위치한 파일/디렉터리를 복사  

```bash
$ sudo apt-get install bison flex libssl-dev bc
```
* 크로스컴파일시 시스템에 필요한 툴  

```bash
$ sudo apt-get install gcc-arm-linux-gnueabih
```
* ARM코어용 크로스 컴파일러  


## 환경 구축

```bash
$ vi ~/.bashrc
```  
* 가장 끝 줄(명령모드 G)에 밑의 한 개의 line추가  
rpi4는 KERNEL=kernel7l, rpi 2이상 kernel7 , rpi 1 이하 kernel

```bash
$ source ~/.bashrc
```  
* .bashrc파일의 스크립트 수행.  
해당 명령어를 사용한 뒤 echo $KERNEL를 사용하여 정상등록 됐는지 확인


## 커널 소스 내려받기  

**1번째 방법**  
```bash
$ git clone --depth=1 --branch rpi-[vir] https://github.com/raspberrypi/linux.git ~/linux
depth=1
```
: 최신 커널소스를 내려받기 해당 옵션이 없을시 저장소 전체를 내려받음  
branch[vir] : [vir]의 커널소스를 내려받기
두 옵션 중 하나만 사용해도 되고 사용하지 않아도 됨

**2번째 방법**  
RaspberryPi kerne을 github에서 내려받기  
URL : github.com/raspberrypi/linux 접속


## .config생성  
***커널소스 디렉터리에서 실시***

**라즈베리파이 4이상의 모델**  
```bash
$ sudo make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- bcm2711_defconfig
```
**라즈베리파이 2이상의 모델**  
```bash
$ sudo make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- bcm2709_defconfig
```
**라즈베리파이 B+이하의 모델**  
```bash
$ sudo make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- bcmrpi_defconfig
```


## 커널 컴파일 실행  
***커널소스 디렉터리에서 실시***

```bash
$ make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- zImage modules dtbs
```
Target시스템의 img file, modules.* file, *.dtb *.dtbo 생성
	
* dtb(device tree blob)  
임베디드보드 하드웨어 설정내용을 커널로 넘기기 위한 구조체


## 타겟 시스템 모듈 인스톨  

```bash
$ mkdir ~/modules
```
모듈 인스톨 시 저장해둘 dir생성  

```bash
$ sudo make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- INSTALL_MOD_PATH=~/modules modules_install  
```
Target시스템 모듈인스톨 수행  
INSTALL_MOD_PATH는 컴파일시 생성되는 파일들이 저장될 경로

```bash
$ rm build source
작업디렉터리 : ~/modules/lib/modules/[커널버전] 
```
불필요한 링크파일 삭제  
삭제하지 않아도 정상수행 가능하나 시간단축을 위해 삭제하는 것  


## 타겟 시스템으로 컴파일 된 파일 및 디렉터리 전송

**ssh에서 제공하는 scp명령어를 사용하여 타겟시스템으로 전송**  
```bash
$ scp linux-rpi-[ver]/arch/arm/boot/zImage pi@xx.xx.xx.xx:/home/pi
$ scp linux-rpi-[ver]/arch/arm/boot/dts/*.dtb pi@xx.xx.xx.xx:/home/pi
$ scp linux-rpi-[ver]/arch/arm/boot/dts/overlays/*.dtb* pi@xx.xx.xx.xx:/home/pi
$ scp linux-rpi-[ver]/arch/arm/boot/dts/overlays/README pi@xx.xx.xx.xx:/home/pi
$ scp -r ~/modules/lib/modules/[ver]/ pi@xx.xx.xx.xx:/home/pi
```

## 호스트에서 받은 파일 적용  
***RaspberryPi에서 진행***  

```bash
$ mv ./zImage /boot/kernel7l.img
```
생성한 커널이미지를 boot내의 kernel7l.img로 교체  
라즈베리파이3 B+이하의 버전일시 kernel7.img로 교체  
라즈베리파이 B+이하 버전일시 kernel.img로 교체  

```bash
$ mv ./[modules ver]/ /lib/modules/.
```
생성한 모듈(directory) 이동
```bash
$ mv ./*.dtb /boot/.
$ mv ./*.dtb* /boot/overlays/. 
$ mv ./README /boot/overlays/.
$ sudo reboot
```
타겟머신 리부팅
```bash
$ uname -r
```
변경된 커널버전 확인하기