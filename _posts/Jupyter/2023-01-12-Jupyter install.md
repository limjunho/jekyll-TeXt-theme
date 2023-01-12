---
title: Springboot API 호출하는 Client IP 수집하기
tags: Springboot
---

## Summry

본 문서에서는 API를 호출하는 Client의 IP Address를 수집하는 방법을 정리한다.

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
- cmd 또는 terminal에서 아래 절차 수행

1. jupyter install

```bash
pip3 install jupyter
```

2. 설치 완료 후 아래 명령 입력

```bash
jupyter notebook
```

3. 아래와 같은 페이지가 열리면 우측 상단 New 버튼을 누르고 Python3를 클릭

![그림1](/assets/Jupyter/jupyter_install/1.png)

4. 상단에 타이틀명은 기본값이 Untitled이다. 클릭해서 변경하고 아래와 같이 코드를 입력할 수 있다.
5. 입력 셀을 클릭 후 “M”을 입력하면 마크다운 편집기로 변한다. 이를 이용해 마크다운 주석을 남기는 등의 작업이 가능하다.

![그림2](/assets/Jupyter/jupyter_install/2.png)

- jupyter notebook은 아래와 같이 한줄을 입력하고, 실행을 한 다음 그 한줄을 추가하여 코드를 이어나갈 수 있다. 즉 중간중간 확인할 수 있는 장점이 있다.

## Reference

> [주피터 노트북 (Jupyter Notebook) 이란? / 설치방법? - 파이썬을 웹(Web) 으로! - 심코딩](https://s1mcoding.tistory.com/26)
