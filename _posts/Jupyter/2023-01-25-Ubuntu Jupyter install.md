---
title: Ubuntu Jupyter Install
tags: Jupyter Linux
---

## Summry

본 문서에서는 Jupyter Notebook의 개요를 설명하고 Ubuntu에 Jupyter Notebook 설치 방법을 정리한다.

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## 주피터란?

- 주피터 노트북이란 오픈소스 (Open source) 기반의 웹 플랫폼으로, 파이썬을 비롯한 다양한 프로그래밍 언어로 코드 작성 및 실행하는 개발 환경을 말한다.
- 주피터 노트북은 데이터 분석에 유용하게 사용이 될 수 있고 최근들어 머신러닝, 딥러닝에 많이 활용이 되고 있다. 아래 세 가지 이유 참조
    - 시각화 (그래프, 마크다운)를 만드는데 아주 유용하다.
    - 다양한 프로그래밍 언어를 지원한다.
    - 개발 중간중간 프로그램을 계속해서 실행 하여 확인 할 수 있다.

하지만 주피터 노트북도 이러한 장점 외에도 단점이 있는데, 바로 그건 주피터 노트북이 웹 기반 플랫폼이라는 고유 속성에서 오는 단점이다.

- 다른 디바이스에서 같은 프로젝트를 이어서 진행하기 까다롭다.

하지만 말그대로 "까다롭다" 이지 "불가능하다"는 아니기 때문에 그렇게 큰 단점은 아니라고 볼 수 있다.

## Install

- python은 사전 설치되어 있어야 함

### 1. pip3 install

```bash
sudo apt-get install python3-pip -y
sudo pip3 install --upgrade pip
```

### 2. virtualenv install

가상환경을 사용하지 않을 예정이라면 생략

```bash
sudo pip3 install virtualenv
```

### 3. jupyter용 디렉터리 생성 및 가상환경 생성

```bash
mkdir jupyter
cd jupyter

// 가상환경을 사용하지 않으면 아래 명령 생략
virtualenv jup_notebook
source jup_notebook/bin/activate
```

### 4. jupyter install

```bash
pip3 install jupyter
```

### 5. jupyter notebook setting

* jupyter notebook 접속 시 입력할 패스워드 설정  
암호화된 출력값을 설정파일에 기입해야 하므로 메모 또는 복사

```bash
python3

>> from notebook.auth import passwd
>> passwd()
```

* jupyter config 생성 및 수정

```bash
jupyter notebook --generate-config 

sudo vi /home/ubuntu/.jupyter/jupyter_notebook_config.py
```

* config 파일 최하단에 아래 내용 기입

```
c = get_config()
c.NotebookApp.password = u'암호화된 비밀번호'
c.NotebookApp.ip = 'IP 접속 규칙(모두 허용 : 0.0.0.0)'
c.NotebookApp.notebook_dir = '/home/ubuntu/jupyter(주피터 노트북 시작 경로, 이 경로로 노트북이 열림)'
c.NotebookApp.allow_origin = '*'
```

### 6. jupyter notebook start

```bash
jupyter notebook --config /home/ubuntu/.jupyter/jupyter_notebook_config.py
```

OSError: [Errno 99] Cannot assign requested address가 발생한다면 아래 명령으로 시작

```bash
jupyter notebook –-ip=0.0.0.0 –-port=8888 –allow-root
```

## Reference

> [How to Install Jupyter notebook on Ubuntu 22.04](https://linuxhint.com/install-jupyter-notebook-ubuntu-22-04/)  
> [AWS EC2 Jupyter Notebook 개발환경 구축하기](https://wooono.tistory.com/165)  
> [리눅스에서 Jupyter notebook 실행시 OSError: [Errno 99] Cannot assign requested address 에러 해결 방법](https://stynxh.github.io/2020-06-27-solve-jupyter-notebook-no-assign-req-addr-error-korean/)