---
title: MySql Insert Image(Python)
tags: MySql Python
---

python을 사용하여 mysql에 image를 삽입/검색 예제  
blob type으로 데이터를 삽입한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

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

### 삽입 예제

```python
  
import mysql.connector
from mysql.connector import Error

def convertToBinaryData(filename):
    # Convert digital data to binary format
    with open(filename, 'rb') as file:
        binaryData = file.read()
    return binaryData

def insertBLOB(number, name, photo):
    print("Inserting BLOB into images table")
    try:
        connection = mysql.connector.connect(host='127.0.0.1',
                                             database='image_db',
                                             user='junho',
                                             password='passwd123')

        cursor = connection.cursor()
        sql_insert_blob_query = """ INSERT INTO images
                          (id, name, photo) VALUES (%s,%s,%s)"""

        Picture = convertToBinaryData(photo)

        # Convert data into tuple format
        insert_blob_tuple = (number, name, Picture) 
        result = cursor.execute(sql_insert_blob_query, insert_blob_tuple) 
        connection.commit()
        print("Image and file inserted successfully as a BLOB into images table", result)

    except mysql.connector.Error as error:
        print("Failed inserting BLOB data into MySQL table {}".format(error))

    finally:
        if (connection.is_connected()):
            cursor.close()
            connection.close()
            print("MySQL connection is closed")

insertBLOB(None, "test1", "lena512.bmp")
```
**insert_img.py**  


### 읽기 예제

```python
import mysql.connector
from mysql.connector import Error

def write_file(data, filename):
    # Convert binary data to proper format and write it on Hard Disk
    with open(filename, 'wb') as file:
        file.write(data)

def readBLOB(number, photo):
    print("Reading BLOB data from images table")

    try:
        connection = mysql.connector.connect(host='192.168.1.8',
                                             database='image_db',
                                             user='junho',
                                             password='passwd123')

        cursor = connection.cursor()
        sql_fetch_blob_query = """SELECT * from images where id = %s"""

        cursor.execute(sql_fetch_blob_query, (number,))
        record = cursor.fetchall()
        for row in record:
            print("Id = ", row[0], )
            print("Name = ", row[1])
            image = row[2]
            print("Storing employee image on disk \n")
            write_file(image, photo)

    except mysql.connector.Error as error:
        print("Failed to read BLOB data from MySQL table {}".format(error))

    finally:
        if (connection.is_connected()):
            cursor.close()
            connection.close()
            print("MySQL connection is closed")

readBLOB(1, "lena512.bmp")
```
**read_img.py**  

> 참고 https://pynative.com/python-mysql-blob-insert-retrieve-file-image-as-a-blob-in-mysql/