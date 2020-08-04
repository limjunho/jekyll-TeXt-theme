---
title: Android Wifi Scan(WifiManager)
tags: Android
---

Android로 주변 AP를 스캔하고 AP의 정보를 얻어오는 예제이다.  

<br />

 [send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

![그림1](/assets/Android/WifiScan/jpg)  
**위의 그림은 예제 실행 결과이다.**  

Android에서 Wifi를 스캔하기 위해 제공하는 라이브러리로 WifiManager가 있다.  
WifiManager를 이용하여 주변 AP를 스캔하기 위해서는 권한이 필요한데 아래와 같다.  

### Permission(권한)  
<br />

**1. ACCESS_FINE_LOCATION**  
**2. ACCESS_COARSE_LOCATION**  
**3. CHANGE_WIFI_STATE**   
위의 권한이 없는 경우 AP Scan은 SecurityException과 함께 실패한다.  
Android 9이상은 기기에서 위치서비스가 활성화되어야 한다. **(설정>위치)**  
<br />

### 사용 제한  
<br />

WifiManager.startScan()을 사용하는 스캔 빈도에 다음과 같은 제한이 적용된다.  

**Android 8.0 및 Android 8.1:**  
각 백그라운드 앱은 30분 간격으로 1회 스캔할 수 있다.  
<br />

**Android 9:**  
각 포그라운드 앱은 2분 간격으로 4회 스캔할 수 있다. 이 경우, 단시간에 여러 번의 스캔이 가능하다.  
백그라운드 앱은 모두 합쳐서 30분 간격으로 1회 스캔할 수 있다.  
<br />

**Android 10 이상:**  
Android 9와 동일한 사용 제한이 적용된다. 로컬 테스트를 위해 사용 제한을 해제하는 새로운 개발자 옵션이 추가되었다.  
(Developer Options > Networking > Wi-Fi scan throttling **off**).  
![그림2](/assets/Android/WifiScan/2.jpg)  

### Scan Process  
<br />

**WifiManager의 스캔 프로세스는 세 가지로 구성된다.**  
<br />

#### 1. SCAN_RESULTS_AVAILABLE_ACTION의 브로드캐스트 리스너 등록  
**이 리스너는 스캔 요청이 완료된 후에 호출되어 성공/실패 상태를 제공한다.**  
<br />

#### 2. WifiManager.startScan()을 사용하여 스캔을 요청  
**단시간에 너무 많은 스캔 요청은 스캔 실패 사유가 될 수 있다.**  
<br />

#### 3. WifiManager.getScanResults()를 사용하여 스캔 결과를 가져오기  
**반환된 스캔 결과는 가장 최근의 결과이다. 하지만 스캔이 완료되지 않았거나 실패한 경우 이전 결과를 가져올 수 있다.**  
스캔 결과에는 다양한 AP정보(SSID, BSSID, RSSI, Frequency등)가 있다.  
<br />

### 단말기의 5G 지원여부 확인  
<br />
{% highlight JAVA linenos %}  
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
    if (wifiManager.is5GHzBandSupported()) {
        Toast.makeText(this, "5g true", Toast.LENGTH_SHORT).show();
    } else {
        Toast.makeText(this, "5g flase", Toast.LENGTH_SHORT).show();
    }
}

{% endhighlight %}  
<br />

당연하게도 스레드를 사용하지 않으면 UI처리가 제대로 되지 않기 때문에 핸드러를 통한 스레드 통신을 사용하였다.  
[Handler Example](https://github.com/limjunho/Android/tree/master/Handler_thread_ex) click this  
<br />

주기적(10초)으로 주변 AP를 스캔하여 AP의 정보를 가져와 스크롤뷰에 띄우는 예제  
[Wifi Scan Example](https://github.com/limjunho/Android/tree/master/Wifi_Scan_ex) click this  