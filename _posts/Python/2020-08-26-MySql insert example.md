---
title: MySql Insert example(Python)
tags: MySql Python
---

Python 코드상에서 DB에 접속하여 데이터를 삽입하는 예제  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

```python
import pymysql

### Database connect ###
conn = pymysql.connect(host='127.0.0.1', user='test', password='',
        db='test_db', charset='utf8')
### get Cursor ###
curs = conn.cursor()
### insert CONST_COMMAND ###
sql = """insert into test_table(number, data1, data2, data3) values(%s, %s, %s, %s)"""

curs.execute(sql, (3, 'g', 'h', 'i'))

conn.commit()

conn.close()
```

**MySql 사용 절차**  
1\. PyMySql 모듈을 import한다.  

PyMySql이 없다면  
```bash
$ sudo pip install PyMySQL
```  

2\. MySQL에 Connect - pymysql.connect()메서드  
3\. Connection 객체로부터 Cursor객체를 가져온다 - cursor()메서드  
4\. SQL문장을 DB 서버에 전송 - execute()메서드  

5-1. Cursor 객체의 fetch 메서드를 사용하여 데이터를 서버로부터 가져와 사용  
5-2. 삽입, 갱신, 삭제 등의 DML(Data Manipulation Language)문장을 실행하는 경우 Connection 객체의  
commit()메서드로 데이터를 확정 갱신  

6\. Connection 객체의 close() 메서드를 사용하여 DB 연결을 종료  


<br />
![그림1](/assets/Python/MySql_insert_ex/1.png)  
데이터 삽입 전  

<br />
![그림2](/assets/Python/MySql_insert_ex/2.png)  
데이터 삽입 후  
