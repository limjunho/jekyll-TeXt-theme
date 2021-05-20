---
title: MySql Insert Image(Python)
tags: MySql Python
---

python을 사용하여 mysql에 image를 삽입/검색 예제  
blob type으로 데이터를 삽입한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

[github](https://github.com/limjunho/Python/tree/master/image_db)


확장자에 상관없이 저장과 읽기가 가능  

MySQL Connector python 설치 필요  
sudo pip3 install mysql-connector-python 명령어로 설치  

데이터베이스와 테이블 사전 생성 필요  
테이블 생성문  
create table images(id int auto_increment primary key, name text not null, photo blob not null);

이미지의 size에 맞게 아래를 참고하여 테이블 생성  

MySQL BLOB별 size  
TINYBLOB  :    maximum length of 255bytes  
BLOB      :    maximum length of 65,535bytes  
MEDIUMBLOB:    maximum length of 16,777,215bytes  
LONGBLOB  :    maximum length of 4,294,967,295bytes  


> 참고 https://pynative.com/python-mysql-blob-insert-retrieve-file-image-as-a-blob-in-mysql/