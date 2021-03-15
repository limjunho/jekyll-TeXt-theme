---
title: Linux session hijacking
tags: Linux
---

tcpdump 툴을 이용한 session hijacking을 실습  

> session hijacking?  
세션 하이재킹은 기본적으로 접속하고자 하는 시스템의 접속하기 위한 인가된 아이디와 패스워드가 없을 경우 현재 접속된 시스템들의 세션 정보를 가로채는 공격 기법
-> 아이디와 패스워드 없이 다른 유저의 세션을 가로채어 시스템에 접속


 [send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

### 테스트 환경  

* Host
    * Server : Apache/2.4.41 (Debian)
    * 웹 어플리케이션 사용(dvwa)
* Target
    * machine : VMware workstation pro
    * OS : ubuntu x86
* Attacker
    * machine : VMware workstation pro
    * OS : kali linux x86

> DBWA?  
DVWA는 보안 전문가가 법적인 환경에서 기술과 도구를 테스트 할 수 있도록 지원한다.  
패킷을 아무곳에서나 수집할 수 없기 때문에 DVWA를 활용


### 실습  

![그림1](/assets/Linux/session_hijacking/1.png)  

* ubuntu(192.168.152.128)에서 DVWA에 접속  
* DVWA는 kali(192.168.152.130)에서 구동중

**PHPSESSID : s3v685o7qqk9kpmgp26iisgb5g**  

ID : admin  
password : password

![그림2](/assets/Linux/session_hijacking/2.PNG)  

* 웹사이트만 스니핑 하기 위해 tcp port 80설정
* 192.168.152.130은 dvwa서비스가 실행중인 주소이다.

**spoofing 결과**  
* Cookie: security=low
* PHPSESSID : s3v685o7qqk9kpmgp26iisgb5g
* 스니핑 결과로 얻은 PHPSESSID가 Target의 PHPSESSID와 일치한다.

![그림3](/assets/Linux/session_hijacking/3.PNG)  

* ubuntu에서 DVWA에 접속 후 SQL injection페이지에서 값을 입력하여 submit하면 서버로 데이터가 전송된다.

![그림4](/assets/Linux/session_hijacking/4.png)  

* kali에서 tcpdump로 ubuntu가 DVWA 웹사이트로 요청한 URL값을 확인  
* ```http://192.168.152.130/dvwa/vulnerabilities/sqli/?id=junho&Submit=Submit```

![그림5](/assets/Linux/session_hijacking/5.PNG)  

```bash
$ sqlmap -u "http://192.168.152.130/dvwa/vulnerabilities/sqli/?id=junho&Submit=Submit" --cookie="PHPSESSID=s3v685o7qqk9kpmgp26iisgb5g; security=low" --dbs
```
알아낸 PHPSESSID값과 ULR을 이용해 세션 하이재킹(Session hijacking)이 가능하다.  
먼저 sqlmap을 사용하여 SQL의 전체적인 구조를 파악한다.  

> sqlamp?
공개 모의침투 도구로 SQL구문삽입(SQL Injection) 취약점을 탐지/진단하고 데이터베이스에 직간접적으로 접근할 수 있는 취약점 분석 도구이다.  
sqlmap은 DB 구조파악, 테이블 내용 유출 등을 자동화해주기 때문에 웹취약점에 대한 수동분석 과정에서 상당히 많은 시간을 절약할 수 있다.  

-u 옵션을 사용하여 파라미터 값으로 DVWA웹사이트로 요창한 URL을 적어주고 
--cookies에 PHPSESSID와 security값을적어준다.  
마지막으로 --dbs옵션을 사용해 DB정보를 파악

![그림6](/assets/Linux/session_hijacking/6.PNG)  

* -D 옵션을 통해 알아낸 DB의 이름을 선택하고 --tables옵션을 주면 해당 데이터 베이스에 있는 테이블목록을 조회

![그림7](/assets/Linux/session_hijacking/7.PNG)  

* 유저 데이터를 탈취하는것이 목적이므로 -T옵션으로 users의 테이블을 선택하고 --columns옵션을 사용해 컬럼을 조회

![그림8](/assets/Linux/session_hijacking/8.PNG)  

* users 테이블의 password 칼럼을 모두 추출하도록 하여 user의 ID와 PASSWORD를 탈취한다.

![그림9](/assets/Linux/session_hijacking/9.png)  

**위에서 탈취한 ID와 PASSWORD를 사용해 dvwa로그인이 가능하다.**

**session hijacking 진행과정**  
1. tcpdump를 이용하여 서버에 접속하는 유저의 쿠키와 세션ID, 요청 URL값을 탈취
2. sqlmap을 이용하여 공격자가 탈취한 세션ID를 자신의 세션ID인것처럼 속여(세션 하이재킹) 스푸핑공격
3. 탈취한 유저의 ID와 PASSWORD로 로그인