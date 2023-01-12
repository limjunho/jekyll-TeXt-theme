---
title: Anaconda 개요 및 설치
tags: Python Jupyter
---

## Summry

본 문서에서는 Anaconda의 개요를 설명하고 설치 방법을 정리한다.

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## 아나콘다란

- 아나콘다는 **머신러닝이나 데이터 분석 등에 사용하는 여러가지 패키지가 기본적으로 포함되어있는 파이썬 배포판이다.** 때문에 해당 분야를 파이썬으로 접근하고자 할 때 세팅이 매우 간단해진다.
- 또한 아나콘다는 **파이썬 가상 환경을 구축하는데도 유용하게 사용**할 수 있다. **내부적으로 conda라는 환경/패키지 관리자가 존재**하며 이 conda를 통해 패키지를 설치하거나 가상 환경을 관리할 수 있다.

### 왜 사용하는가?

- 공식 홈페이지에서 설치한 파이썬은 패키지 관리자인 **pip를 제외한 어떤 패키지도 추가로 설치되지 않는다.**
    - 필요한 패키지를 하나하나 손수 설치해야 하며 가상 환경도 virtualenv등을 따로 사용해서 구축
    - 아나콘다는 가상환경을 만들어 그 안에 필요한 패키지들을 설치하고 그 가상환경안에서 다른 모든 요소들과 ‘논리적’으로 분리되어 개발환경을 조성할 수 있다. 쉽게 말해 A환경과 B환경에 Keras 패키지가 있다고 했을 때 각 패키지의 버전을 다르게 구성 가능하다.

- 머신러닝과 데이터 분석의 입지가 나날이 커지고 두 분야에서 파이썬이 강세를 보이는 현재 시점에선 당장은 필요없을지 몰라도 **나중을 생각하면 아나콘다를 쓰지 않을 이유는 딱히 없으며** 여차하면 그냥 가상 환경 관리자로만 써도 유용하게 사용할 수 있다.
- **기본 용량이 크다는 것이 아나콘다의 단점**으로 꼽히는데, 설치된 패키지의 수를 줄여 용량이 크다는 단점을 보완한 **미니콘다(miniconda)**라는 것도 있다.

### Install

[Anaconda - Anaconda Distribution](https://www.anaconda.com/products/distribution)

- Window - 파이썬이 이미 설치된 경우 반드시 삭제 후 아나콘다 설치
    - 중복 설치 시 환경 변수 충돌 등 문제 발생 소지가 있음.
- Mac - 기본 설치된 파이썬을 무시해도 무방함.

### Anaconda Jupyter Notebook

![그림1](/assets/Python/Anaconda_install/1.png)

- Anaconda 메뉴중 Jupyter Notebook을 선택한 경우, Jupyter Notebook 서버 (콘솔 프로그램)와 클라이언트로 사용되는 웹 브라우저 등 2개의 프로그램이 실행된다.
- Notebook 서버 프로그램은 백그라운드에서 실행되는 파이썬 프로그램으로 웹 브라우저 클라이언트가 접속하는 서버 프로그램이며 실제 파이썬 코드 실행은 여기에서 일어난다. 그리고, 웹 브라우저는 파이썬 코드를 입력받고, 실행해 볼 수 있는 UI를 제공하는 클라이언트로서 코드를 Notebook 서버에 보내 결과를 다시 웹 브라우저에 뿌려 주게 된다.

## Reference

> [파이썬(python) - 아나콘다(Anaconda)란?](https://tibetsandfox.tistory.com/36)  
> [예제로 배우는 파이썬 프로그래밍 - Jupyter Notebook](http://pythonstudy.xyz/python/article/514-Jupyter-Notebook)  
> [[Python] 아나콘다/주피터 노트북(Anaconda/jupyter notebook) 설치하기 on windows](https://annajang.tistory.com/26)