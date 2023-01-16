---
title: Managed Spot Training in Amazon SageMaker
tags: AWS
---

## Summry

본 문서에서는 Amazon SageMaker의 Spot Instance 기능과 관리 및 운영을 정리한다.

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## Managed Spot Training?

- **Managed Spot Training은 온디맨드 인스턴스에 비해 모델 교육 비용을 최대 90%까지 최적화할 수 있다.**  
- **SageMaker는 사용자 대신하여 스팟 중단을 관리한다.**

## 기능

- 스팟 인스턴스를 사용하는 훈련 작업과 Amazon EC2 스팟 인스턴스를 사용하여 작업이 실행될 때까지 SageMaker가 대기하는 시간을 지정하는 중지 조건을 지정할 수 있다.
- 교육 실행 중에 생성된 지표 및 로그는 CloudWatch에서 사용할 수 있다.

### Spot Instance interruption Handling

- **스팟 인스턴스 중단 시 SageMaker는 로컬 경로에서 Amazon S3로 체크포인트 데이터를 복사, 작업이 다시 시작되면 SageMaker는 Amazon S3의 데이터를 로컬 경로로 다시 복사한다. (다음 학습 작업을 다시 시작하는 대신 마지막 체크포인트에서 다시 시작할 수 있다.)**

### Spot Training Process

1. 체크포인트 저장
    - **Amazon SageMaker는 학습 스크립트에서 생성된 체크포인트 파일을 Amazon S3에 자동으로 백업하고 동기화**
    - 로컬 체크포인트 파일 경로( `/opt/ml/checkpoints`기본값)를 가져오고 최신 체크포인트 및 관련 epoch 번호에서 로드된 모델을 반환. 체크포인트 파일 default path : `/opt/ml/checkpoints`, 아래 구현 예시 참조
        
        ```python
        from tensorflow.keras.callbacks import ModelCheckpoint
        checkpoint_path  = "/opt/ml/checkpoints"
        checkpoint_names = 'cifar10-'+model_type+'.{epoch:03d}.h5'
        checkpoint_callback = ModelCheckpoint(filepath=f'{checkpoint_path}/{checkpoint_names}',
                                                  save_weights_only=False,
                                                  monitor='val_loss')
        
        model.fit(train_dataset, ...
                  epochs=epochs,
                  initial_epoch=epoch_number,
                  callbacks=[checkpoint_callback])
        ```
        
    - **체크포인트 저장 예시**
    
    ![그림1](/assets/AWS/SageMaker_and_Spot/1.png)
    
2. 체크포인트 파일에서 재개(중단 후 새 인스턴스 시작 시)
    - **학습 스크립트는 체크포인트 파일에서 훈련 재개할 수 있도록 구현해야함 그렇지 않으면 스팟 중단 후 새 스팟 시작 시 처음부터 학습**, 아래 구현 예시 참조
    - 로컬 체크포인트 파일 경로( `/opt/ml/checkpoints`기본값)를 가져오고 최신 체크포인트 및 관련 epoch 번호에서 로드된 모델을 반환
        
        ```python
        Import os, re
        from tensorflow.keras.models import load_model
        def load_checkpoint_model(checkpoint_path):
            files = [f for f in os.listdir(checkpoint_path) if f.endswith('.' + 'h5')]  
            epoch_numbers = [re.search('(?<=\.)(.*[0-9])(?=\.)',f).group() for f in files]
              
            max_epoch_number = max(epoch_numbers)
            max_epoch_index = epoch_numbers.index(max_epoch_number)
            max_epoch_filename = files[max_epoch_index]
            
            resume_model = load_model(f'{checkpoint_path}/{max_epoch_filename}')
            return resume_model, max_epoch_number
        ```
        
3. Managed Spot Training을 실행하도록 Amazon SageMaker 지시
    - **관리형 스팟 교육 작업을 실행하려면 표준 Amazon SageMaker Estimator 함수 호출에 몇 가지 추가 옵션을 지정**, 아래 구현 예시 참조
        
        ```python
        from sagemaker.tensorflow import TensorFlow
        bucket_name = sagemaker_session.default_bucket()
        output_path = f's3://{bucket_name}/jobs'
        job_name    = 'tensorflow-spot-job'
        
        tf_estimator = TensorFlow(entry_point              = 'cifar10-training-sagemaker.py', 
                                  role                     = role,
                                  train_instance_count     = 1, 
                                  train_instance_type      = 'ml.p3.2xlarge',
                                  framework_version        = '1.15', 
                                  ...
                                  ...
                                  checkpoint_s3_uri        = f'{output_path}/{job_name}/checkpoints',
                                  train_use_spot_instances = True,
                                  train_max_wait           = 7200)
        ```
        
        - **`train_use_spot_instances`**: 관리형 스팟 교육을 실행하도록 Amazon SageMaker에 지시.
        - **`checkpoint_s3_uri` :** 체크포인트 파일을 이 Amazon S3 위치에 동기화하도록 Amazon SageMaker에 지시.
        - **`train_max_wait`:** 이 시간이 지나고 스팟 용량을 사용할 수 없게 되면 Amazon SageMaker에 작업을 종료하도록 지시.

### Monitoring

1. Jupyter 노트북에서 로그보기
2. Amazon SageMaker Console → Training Job → 학습 작업 이름을 클릭(교육 완료 시 얼마나 절약 했는지 확인 가능)
3. AWS CloudWatch

## Reference

[Amazon SageMaker 개요 및 Jupyter Notebook 구성](https://limjunho.github.io/2023/01/15/AWS-SageMaker-JupyterNotebook.html)

[Managed Spot Training in Amazon SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/model-managed-spot-training.html)

[A quick guide to using Spot instances with Amazon SageMaker](https://towardsdatascience.com/a-quick-guide-to-using-spot-instances-with-amazon-sagemaker-b9cfb3a44a68)