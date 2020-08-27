---
title: Python Json 자료형 매핑   
tags: Python
---

Python의 dict를 JSON형식으로 매핑하는 예제이다.  
또한 JSON형식을 Python형식으로 매핑하는 예제  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

**JSON 문자열은 Python 자료형(주로 dict, list)과 상호 변환 가능하며, 형식도 매우 유사하다.**  
**문자열의 경우 Python에서는 작은따옴표, 큰따옴표를 모두 사용할 수 있지만 JSON 표준에서는 큰따옴표만 허용된다.**  
**JSON 표준은 유니코드 처리 방식이 다르므로 한글 등을 처리시 유의해야 한다.**  

### Python 딕셔너리를 JSON형식으로 변환  

```python
import json

data = {'number':'1', 'data1':'abc', 'data2':'def'}
jsonform = json.dumps(data)
```

### JSON 문자열을  Python 딕셔너리로 변환  

```python
import json

data = '{"number":"1", "data1":"abc", "data2":"def"}'
pythonform = json.loads(data)
```

**JSON 형식은 멀티바이트 유니코드 문자를 \uxxxx로 표현하는 것이 표준이다.**  
**따라서 한글을 JSON 형식으로 변환하는 경우 인터넷상의 JSON변환기 등을 활용하면 편리하다.**  

**다시 Python 딕셔너리로 변환하면 원래의 한글로 변환됨.**  
