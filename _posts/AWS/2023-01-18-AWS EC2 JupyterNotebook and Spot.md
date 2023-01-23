---
title: EC2 & JupyterNotebook 에서 Spot Instance 점유하기
tags: AWS
---

## Summry

본 문서에서는 Amazon EC2에 JupyterNotebook을 구성하고 Spot Instance 점유 및 중단 관리에 대해 설명한다.

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## EC2 Server에서 Jupyter Notebook 실행

1. EC2 Instance 생성
2. Jupyter용 EC2 서버 사용자 지정(보안 그룹 설정)
3. EC2에 연결
4. Jupyter 노트북 설치
5. Jupyter 노트북의 경로 구성
6. Jupyter 노트북 설정 구성
7. 노트북용 디렉터리 만들기
8. EC2 Jupyter 서버에 연결

자세한 과정은 [Running Jupyter Notebook on an EC2 Server](https://dataschool.com/data-modeling-101/running-jupyter-notebook-on-an-ec2-server/) 참고

## Jupyter Notebook에서 Spot Instance 점유하기

- Jupyter Notebook에서도 boto3 Lib가 사용 가능
- SageMaker는 Spot Instance의 provisioning까지 서비스에 포함되어 있지만 일반적인 Jupyter Notebook에는 그러한 기능이 없어 사용자가 직접 구현해야함.
    - example code
        - A Python script that requests a spot EC2 instance. Using tag name to control specific instance.  
        [auto-aws-ec2-spot - github](https://github.com/s-mawjee/auto-aws-ec2-spot)
        

---

## 중단에 대해..

- SageMaker와 달리 Spot을 provisioning 해주거나 interruption에 대해 매니징해주는 기능이 없음 덕분에 SageMaker보다 저렴하지만 사용자의 공수가 필요함
    - EC2, SageMaker 가격 비교(23.01.16 기준 서울 리전)
        - EC2 Insatnce t3.medium. vCPU 2,  MEM 4GiB $0.052/h(시간 당 64.36원)
        - SageMaker ml.t3.medium vCPU 2, MEM 4GiB $0.062/h(시간 당 76.74원)
        - Spot Instance t3.medium  vCPU 2, MEM 4GiB $0.0156/h(시간 당 19.31원)

- 중단에 대비하는 여러 방법
    - 기본은 데이터 백업.
    - 중단 2분 전 (구)CloudWatch Events (현)EventBridge로 알림. 이 알림을 Lambda로 처리하여 중단을 대비하는 작업을 실행하는 식으로 직접 중단 관리
1. Auto Scaling Group을 사용하여 스팟이 중단되면 대체 인스턴스 시작
2. 로드 밸런서에서 등록 해제 
    - 요청이 들어오는 작업들을 수행하는 경우 사용(예를 들어 트래픽이 가장 많은 시간에 사용)

### 기타

- jupyter notebook에서 Spot price를 확인하고 예측할 수 있는 서비스를 구현하여 서비스중인 업체가 있으며 AWS Marketplace에서 비용을 지불하고 이용할 수 있음.
    - CloudCluster의 AWS Spot Pricing Jupyter Notebook 서비스
        
        [AWS Spot Pricing Jupyter Notebook](http://aws.cloudycluster.com/blog/aws-spot-pricing-jupyter-notebook/)
        
        [CloudyCluster HPC with Open OnDemand](https://aws.amazon.com/marketplace/pp/prodview-am3yf3mjcj2bs)
        

---

## Reference

[Prepare for interruptions](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/prepare-for-interruptions.html)

[Managing Spot Instance Interruptions](https://docs.aws.amazon.com/whitepapers/latest/cost-optimization-leveraging-ec2-spot-instances/managing-instance-termination.html)

[ec2-spot-labs/README.md at master · awslabs/ec2-spot-labs](https://github.com/awslabs/ec2-spot-labs/blob/master/ec2-spot-interruption-handler/README.md)

[Amazon EC2 스팟 인스턴스 중단 알림 활용하기 - megazone cloud](https://www.megazone.com/techblog_180326_amazon-ec2-spot-instance/)

[Running Jupyter Notebook on an EC2 Server](https://dataschool.com/data-modeling-101/running-jupyter-notebook-on-an-ec2-server/)

[EC2 온디맨드 인스턴스 요금 - Amazon Web Services](https://aws.amazon.com/ko/ec2/pricing/on-demand/)

[Amazon EC2 스팟 인스턴스 요금](https://aws.amazon.com/ko/ec2/spot/pricing/)

[Amazon SageMaker Pricing - Machine Learning - Amazon Web Services](https://aws.amazon.com/ko/sagemaker/pricing/)

### 작성자의 생각

온프레미스나 개인 로컬PC에서 Jupyter Notebook을 사용하는 것이 아닌 상황에서 DNN 학습을 하는 경우 SageMaker 대신 EC2(JupyterNotebook)에서 Spot Instance를 요청하는 방법을 선택할 근거는 찾지 못하였다.  
DNN 학습이 아닌 다른 용도로 사용한다면 AutoScaling이나 Load Balancing을 통해 적절한 대응이 가능하므로 가격이 더 비싼 SageMaker 대신 EC2에서 워크로드의 증가나 트래픽 증가에 따른 대응으로 스팟을 끌어오는 식으로 사용할 수 있다.  
DNN 학습이 아닌 경우 대부분 로드 밸런서나 모니터링을 활용하는 경우가 많기 때문에 스팟 중단 관리를 위해서 아키텍처 구조를 조정할 일은 없을 듯 하여 이벤트 처리에 대한 부분을 제외하고는 큰 부담이 없을 듯 생각된다.