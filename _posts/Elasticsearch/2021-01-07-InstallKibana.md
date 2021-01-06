---
title: Kibana install
tags: Elasticsearch
---

window환경에서 Kibana 설치하는 방법.  

Kibana는 Elasticsearch와 함께 사용하도록 설계된 오픈소스 분석 및 시각화 플랫폼이다.  
**Kibana를 사용하여 Elasticsearch 색인에 저장된 데이터를 검색하고 보고 상호 작용을 수행할 수 있다.** 손쉽게 고급 데이터 분석을 수행하고 다양한 차트, 테이블, 지도의 형태로 데이터를 시각화할 수 있으며 **Kibana는 많은 양의 데이터를 쉽게 이해할 수 있도록 도와준다.**  
간단한 브라우저 기반 인터페이스에서 Elasticsearch 쿼리의 변경 사항을 실시간으로 표시하는 동적 대시보드를 신속하게 생성하고 공유할 수 있다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### Install  

[install](https://www.elastic.co/kr/downloads/kibana)에서 윈도우 버전을 다운로드.  

![그림1](/assets/Elasticsearch/InstallKibana/1.png)  

### Start

Kibana의 실행은 윈도우버전이므로 bin/kibana.bat을 더블클릭해서 실행한다. **(elasticsearch.bat을 먼저 실행하고 실행)**   
Kibana의 설정파일은 config/kibana.yml인데 여기서 변경을 하지 않는다면 **5601포트에서 서비스된다.**  

크롬 웹브라우저에서 Kibana 서버(http://localhost:5601 )에 접속한 모습이다.  
**Management의 Dev Tools메뉴로 이동하면 Elasticsearch에 쿼리를 할 수 있다.**  
![그림2](/assets/Elasticsearch/InstallKibana/2.png)  

![그림3](/assets/Elasticsearch/InstallKibana/3.png)  