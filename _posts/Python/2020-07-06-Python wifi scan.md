---
title: Python WifiScan  
tags: Python
---

Raspberry Pi 4에서 주변 AP의 정보를 수집하는 용도로 작성된 코드이다.  
[Github](https://github.com/limjunho/Python_WifiScan)  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

* python에서 외부 명령어(iwlist wlan0 scan)를 통해 얻은 정보를 python의 문자열 슬라이싱을 이용하여 수집  

* Cell 01의 정보는 현재 접속중인 AP의 정보이며 이 예제에서는 Cell 01의 정보만
수집한다.  

* 수집 정보  
    1. now time
    2. AP macAddress
    3. frequency
    4. quality
    5. dBm
    6. ESSID
    7. client macAddress
