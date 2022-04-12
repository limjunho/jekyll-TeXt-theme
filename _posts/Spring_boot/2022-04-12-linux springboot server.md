---
title: Ubuntu Springboot build & start
tags: MySql Springboot
---

## Summry

본 문서에서는 Ubuntu에서 Springboot server를 빌드하고 실행하는 방법을 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### 1. 개발한 코드 업로드

작성자는 windows에서 개발한 코드를 aws ec2 ubuntu server에 clone했다.

### 2. 필요한 패키지 install

```shell
sudo apt update
sudo apt install openjdk-8-jdk
```

버전에 맞게 잘 설치하면 된다.  

### 3. TimeZone 변경

기본 TimeZone은 UTC로 설정되어있기 때문에, 변경해 주어야 한다.

```shell
sudo timedatectl set-timezone 'Asia/Seoul'
```

date 명령으로 시간을 확인할 수 있다.

### 4. Build

프로젝트의 / 디렉터리에서 ```./gradlew build```를 입력하고 빌드를 기다린다.

단, gradlew 파일에 실행권한이 있어야 한다. 없다면 ```chmod u+x gradlew``` 명령 사용

**작성자처럼 ec2 프리티어를 쓰거나 낮은 사양의 서버를 사용하는 경우 빌드가 매우 오래걸릴 수 있다.**  
작성자는 window에서 빌드해 깃허브에 업로드 한 뒤, aws ec2 서버에 pull받아 사용하였다.  
ec2(vCPU 1개, memory 1G, volume 8G)환경에서 12분이 넘도록 빌드가 되지 않는걸 개인local(8core cpu, memory 32G)에서 빌드하니 5초밖에 걸리지 않았다.  
**이 방법 적극 추천.**  

* window build 방법
    * cmd로 프로젝트 디렉터리로 이동 후 gradlew build

### 5. Start

빌드가 끝났다면 build/libs/로 이동한다.

```java -jar {jarfile}``` 명령으로 실행한다.

* **백그라운드 실행**
    * ```java -jar {jarfile} &```
* **ssh접속 터미널을 종료하여도 서버가 종료되지 않도록 하려면**
    * ```nohup java -jar {jarfile} &```

## Reference

> [Ubuntu SpringBoot 서버 설정하기](https://velog.io/@prayme/Ubuntu-SpringBoot-%EC%84%9C%EB%B2%84-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)  
> [#2 spring boot 빌드와 실행 (window)](https://velog.io/@eunji/2-spring-boot-%EB%B9%8C%EB%93%9C%EC%99%80-%EC%8B%A4%ED%96%89-window)  