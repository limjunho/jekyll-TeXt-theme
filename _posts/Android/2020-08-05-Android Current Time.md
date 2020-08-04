---
title: Android Time(현재시간 가져오기)
tags: Android
---

Android Programming을 할 때 현재 시간이 필요한 경우가 꽤 많다.  

<br />

JAVA에서는 이를 위해 제공하는 라이브러리가 있다.  

 [send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### 1. 현재 시간 가져오기  

**long now = System.currentMills();**  
<br />

### 2. Date로 변경  

**Date reDate = new Date(now);**  
<br />

### 3. 날짜, 시간등 원하는 형태로 바꾸기  

**SimpleDateFormat Format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");**  
**String formatDate = Format.format(reDate);**  
<br />

Format을 변경하는 것 만으로 년,월,일 또는 년,월,일,시,분 등 다양한 형태로 현재 시간을 가져올 수 있다.  

**ex) SimpleDateFormat Format = new SimpleDateFormat("yyyy-mm-dd); -> 년,월,일**  

![그림1](/assets/Android/CurrentTime/1.jpg)  
**예제 실행결과**  

다양한 형태로 현재 시간을 가져오는 예제    
[Current time example](https://github.com/limjunho/Android/tree/master/Time_ex) click this  
 