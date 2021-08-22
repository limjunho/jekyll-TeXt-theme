---
title: MySql Select example(Python)
tags: MySql Python
---

DB에서 데이터를 가져와 코드상에서 활용하는 경우를 위한 예제

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

```python
import pymysql

### Database connect ###
conn = pymysql.connect(host='127.0.0.1', user='test', password='',
        db='test_db', charset='utf8')
### get Cursor ###
curs = conn.cursor(pymysql.cursors.DictCursor)
### select CONST_COMMAND ###
sql = """select * from test_table"""

curs.execute(sql)

# fetchone() - 한번 호출에 하나의 Row를 가져올 때 사용
# fetchall() - 모든 데이터를 한꺼번에 가져올 때 사용
# fetchmany(n) - n개 만큼의 데이터를 가져올 때 사용

data = curs.fetchone()
print(data)

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

**데이터를 가져오는 경우**  
fetchall() - 모든 데이터를 가져오는 경우 사용  
fetchone() - 한번 호출에 하나의 Row를 가져오는 경우 사용  
fetchmany(n) - n개 만큼의 데이터를 가져올 때 사용  

5-2. 삽입, 갱신, 삭제 등의 DML(Data Manipulation Language)문장을 실행하는 경우 Connection 객체의  
commit()메서드로 데이터를 확정 갱신  

6\. Connection 객체의 close() 메서드를 사용하여 DB 연결을 종료  


<br />
![그림1](/assets/Python/MySql_select_ex/1.png)  
사용 예제1  

