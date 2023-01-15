---
title: Amazon SageMaker 개요 및 Jupyter Notebook 구성
tags: AWS Jupyter
---

## Summry

본 문서에서는 Amazon SageMaker의 개요를 설명하고 Jupyter Notebook 구성 방법을 정리한다.

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## Amazon SageMaker?

- 아마존 세이지메이커는 2017년 11월 시작된 클라우드 기계 학습 플랫폼이다. 세이지메이커는 개발자들이 기계학습 모델을 클라우드에 개발, 트레이닝, 디플로이할 수 있도록 만들어준다.
  - 데이터 준비를 단순화하고 협업을 개선하며 배포를 자동화하도록 설계됨
  - 팀은 데이터 세트의 다양한 부분을 시각화하고 분석하기 위해 상용구 코드를 작성하는 데 몇 달을 소비한다. SageMaker Data Wrangler가 제공하는 새로운 내장형 데이터 준비 기능을 사용하면 몇 번의 클릭만으로 노트북에서 직접 데이터를 시각화하고, 품질 문제를 감지하고, 제안을 받고, 문제를 해결할 수 있다.
  - Amazon SageMaker는 공유 공간을 만들 수 있다. 팀은 리소스를 동시에 구성, 공유 및 협업할 수 있다.
    실시간으로 함께 편집, 실행 및 검토하여 더 빠르게 반복할 수 있다.
  - 모든 개발자 및 Data Scientist들이 ML(Machine Learning) 모델을 빠르게 구축, 훈련 및 배포할 수 있도록 하는 모듈식의 완전 관리형 서비스
- 간단하게 RDS는 ‘EC2 인스턴스 위에 DB를 설치한 완전 관리형 서비스’라고 이해할 수 있는 것과 같이, SageMaker는 ‘EC2 인스턴스 위에 머신러닝에 대표적인 Jupyter Notebook을 설치한 것’이라고 이해할 수 있다.

## SageMaker 장점

- 비싼 컴퓨터를 필요로 하지 않는다.
- ML 패키지들을 설치할 필요가 없다.
- 코드 작성부터 배포까지 한 번에 가능하다.
- 타 AWS 서비스와의 연계가 잘 이루어진다.

## Jupyter Notebook 구성

1. sageMaker 검색 및 접속

   ![그림1](/assets/AWS/SageMaker/1.png)

2. 노트북 카테고리 → 노트북 인스턴스

   ![그림2](/assets/AWS/SageMaker/2.png)

3. 새 노트북 생성 및 스펙 선택

   ![그림3](/assets/AWS/SageMaker/3.png)

   - 탄력적 추론(Elastic Inference)이란? - Amazon SageMaker, Amazon EC2 및 Amazon ECS와 통합되는 기능이다.
     - Amazon Elastic Inference를 사용하면 Amazon EC2 인스턴스, Amazon SageMaker 인스턴스 또는 ECS 작업에 GPU 지원 추론 가속을 적절한 양만 연결할 수 있다. 따라서 애플리케이션의 전체 컴퓨팅, 메모리 및 스토리지 요구 사항에 가장 적합한 CPU 인스턴스를 선택한 후 필요한 양의 GPU 지원 추론 가속을 별도로 구성할 수 있다.
     - Amazon Elastic Inference는 AWS의 향상된 TensorFlow Serving, Apache MXNet 및 PyTorch 버전에서 사용하도록 설계되었다. IAM 정책을 사용하여 액셀러레이터에 대한 액세스를 안전하게 제어할 수 있으며 향상된 TensorFlow Serving, MXNet 및 PyTorch 라이브러리가 Amazon SageMaker, AWS Deep Learning AMI 및 AWS Deep Learning Containers에 자동으로 제공되므로 프로덕션에 모델을 배포하기 위해 코드를 직접 변경할 필요가 없다.
     - Amazon Elastic Inference는 Amazon SageMaker, Amazon EC2 및 Amazon ECS 인스턴스의 크기 조정에 사용하는 그룹과 동일한 Amazon EC2 Auto Scaling 그룹에 속할 수 있다. 따라서 애플리케이션 수요를 충족하기 위해 애플리케이션의 컴퓨팅 용량과 함께 추론 가속의 크기를 쉽게 조정할 수 있다.

4. 완료

   ![그림4](/assets/AWS/SageMaker/4.png)

   - 노트북 인스턴스를 생성하면 잠시동안 Pending 상태가 이어지는데, 2분 이내에 InService 상태가 된다.

5. Jupyter 열기

   ![그림5](/assets/AWS/SageMaker/5.png)

   - 상태가 InService로 바뀌면 Jupyter 열기를 클릭 해 Jupyter web에 접속할 수 있다.

## Reference

[[AWS] 완전관리형 ML서비스 Amazon SageMaker 서비스들 정리](https://honglab.tistory.com/59)

[[AWS] Amazon SageMaker](https://blog.kico.co.kr/2022/03/02/aws-amazon-sagemaker/)

[Use Amazon SageMaker Elastic Inference (EI)](https://docs.aws.amazon.com/sagemaker/latest/dg/ei.html#ei-intro-notebook)

[What Is Amazon Elastic Inference?](https://docs.aws.amazon.com/elastic-inference/latest/developerguide/what-is-ei.html)

[Amazon Elastic Inference 기능 - Amazon Web Services](https://aws.amazon.com/ko/machine-learning/elastic-inference/features/)

[Amazon SageMaker를 사용해서 기계학습 모델을 구축, 학습 및 배포를 해보았습니다 | DevelopersIO](https://dev.classmethod.jp/articles/yjsong_sagemaker_xgboost/)
