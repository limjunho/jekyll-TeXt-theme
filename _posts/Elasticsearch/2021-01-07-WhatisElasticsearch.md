---
title: What is Elasticsearch?
tags: Elasticsearch
---

Elasticsearch는 Apache Lucene( 아파치 루씬 ) 기반의 Java 오픈소스 분산 검색 엔진이다.  

* 루씬은 자바 언어로 이루어진 정보 검색 라이브러리 자유-오픈 소스 소프트웨어이다.  
<br />

Elasticsearch를 통해 루씬 라이브러리를 단독으로 사용할 수 있게 되었으며, **방대한 양의 데이터를 신속하게, 거의 실시간( NRT, Near Real Time )으로 저장, 검색, 분석할 수 있다.**  
<br />

Elasticsearch는 검색을 위해 단독으로 사용되거나 ELK스택으로 사용되기도 한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### ELK스택  

ELK(Elasticsearch, Logstatsh, Kibana)스택이란  

* Logstatsh  
  * 다양한 소스(DBm csv파일 등)의 로그 또는 트랜잭션 데이터를 수집, 집계, 파싱하여 Elasticsearch로 전달  
* Elasticsearch
  * Logstash로부터 받은 데이터를 검색 및 집계를 하여 필요한 관심 있는 정보를 획득  
* Kibana
  * Elasticsearch의 빠른 검색을 통해 데이터를 시각화 및 모니터링  
<br />

### Elasticsearch와 RDB

RDB의 요소들과 Elasticsearch의 요소는 아래의 표와 같이 대응된다.  
![그림1](/assets/Elasticsearch/WhatisElasticsearch/1.PNG)  
<br />

### Elasticsearch 용어정리  

![그림1](/assets/Elasticsearch/WhatisElasticsearch/2.png)  

* 클러스터(cluseter)  
  * 클러스터란 **Elasticsearch에서 가장 큰 시스템 단위**를 의미하며, 최소 하나 이상의 노드로 이루어진 노드들의 집합이다. 서로 다른 클러스터는 데이터의 접근, 교환을 할 수 없는 독립적인 시스템으로 유지되며 여러 대의 서버가 하나의 클러스터를 구성할 수 있고, 한 서버에 여러 개의 클러스터가 존재할수도 있다.  

* 노드(node)  
  * Elasticsearch를 구성하는 하나의 단위 프로세스를 의미하며 그 역할에 따라 **Master-eligible, Data, Ingest, Tribe 노드로 구분**할 수 있다.  

    * master-eligible node  
    \- **클러스터를 제어하는 마스터로 선택할 수 있는 노드.** 여기서 master 노드가 하는 역할은 다음과 같다.  
      1. 인덱스 생성, 삭제  
      2. 클러스더 노드들의 추적, 관리  
      3. 데이터 입력 시 어느 샤드에 할당할 것인지  
    * Data node  
    \- **데이터와 관련된 CRUD 작업과 관련있는 노드.** 이 노드는 CPU, 메모리 등 자원을 많이 소모하므로 모니터링이 필요하며, master 노드와 분리되는 것이 좋다.  
    * Ingest node
    \- **데이터를 변환하는 등 사전 처리 파이프라인을 실행하는 역할을 한다.**  
    * Coordination only node  
    \- **data node와 master-eligible node의 일을 대신**하는 이 노드는 대규모 클러스터에서 큰 이점이 있다. 즉 로드밸런서와 비슷한 역할을 한다.  

* 인덱스(index) / 샤드(Shard) / 복제(Replica)  
  shard와 replica는 Elasticsearch에만 존재하는 개념이 아니라, 분산 데이터베이스 시스템에도 존재하는 개념이다.  
  * index  
  \- RDBMS에서 database와 대응하는 개념  
  *  sharding  
  \- 데이터를 분산해서 저장하는 방법을 의미한다. 즉, Elasticsearch에서 *scale out*을 위해 **index를 여러 shard로 쪼갠 것으로 기본적으로 1개가 존재**하며, 검색 성능 향상을 위해 클러스터의 샤드 갯수를 조정하는 튜닝을 하기도 한다.  
  * replica  
  \-  또 다른 형태의 shard라고 할 수 있다. **노드를 손실했을 경우 데이터의 신뢰성을 위해 샤드들을 복제**하기 때문에 replica는 서로 다른 노드에 존재할 것을 권장합니다.
<br />

### Elasticsearch 특징  

* Scale out  
  * 샤드를 통해 규모가 수평적으로 늘어날 수 있다.  
* 고가용성  
  * Replica를 통해 데이터의 안정성을 보장한다.  
* Schema Free  
  * Json 문서를 통해 데이터 검색을 수행하므로 스키마 개념이 없다.  
* Restful  
  * 데이터 CURD 작업은 HTTP Restful API를 통해 수행한다.  

  Data CURD|Elasticsearch Restful|  
  :---:|:---:|  
  SELECT|GET|  
  INSERT|PUT|  
  UPDATE|POST|  
  DELETE|DELETE|  
<br />

### inverted index(역색인)  

Elasticsearch는 방대한 양의 데이터를 신속하게, 거의 실시간( NRT, Near Real Time )으로 저장, 검색, 분석할 수 있다고 소개했는데 그 이유는 **inverted index(역색인)**에 있다.  

**index와 inverted index의 차이**  
쉽게 말해 책에서 맨 앞에 볼 수 있는 목차가 index, 책 맨 뒤에 키워드마다 찾아볼 수 있도록 찾아보기가 inverted index.

Elasticsearch는 텍스트를 파싱해서 검색어 사전을 만든 다음에 inverted index 방식으로 텍스트를 저장한다.  
**각 단어들을 저장하고 대문자는 소문자처리(lowercase)처리하고, 유사어도 체크하는 등의 작업을 통해 텍스트를 저장한다.** 때문에 RDBMS보다 전문검색(Full Text Search)에 빠른 성능을 보인다.

> 참조  
https://www.slideshare.net/deview/2d1elasticsearch  
https://github.com/exo-archives/exo-es-search  
https://victorydntmd.tistory.com/308  