---
title: MySQL Version check
tags: MySql
---

## Summry

본 문서에서는 mysql 버전을 확인하는 방법을 간단히 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## Mysql server version check

먼저 DB에 접속한 후,  

```sql
SELECT VERSION();
```

위의 명령을 입력하면 아래의 결과를 확인할 수 있다.  

|VERSION|
|-------|
|8.0....|

더 자세한 정보를 알 고 싶다면 아래의 명령을 사용한다.  

```sql
SHOW VARIABLES LIKE '%VERSION%'
```

아래와 같은 결과를 얻을 수 있다.  

|Variable_name|Value|
|-------------|-----|
|admin_tls_version|“TLSv1,TLSv1.1,TLSv1.2,TLSv1.3”|
|immediate_server_version|999999|
|innodb_version|8.0.21|
|original_server_version|999999|
|protocol_version|10|
|slave_type_conversions||
|tls_version|"TLSv1,TLSv1.1,TLSv1.2,TLSv1.3"|
|version|8.0.21|
|version_comment|Homebrew|
|version_compile_machine|x86_64|
|version_compile_os|osx10.15|
|version_compile_zlib|1.2.11|


## Mysql client version check

```shell
mysql --version
```

또는  

```shell
mysql -V
```

명령을 사용하여 버전을 확인할 수 있다.

## Reference
> [[Mysql] 버전 확인하는 방법 - DEEPLIFY](https://deeplify.dev/database/troubleshoot/how-to-check-mysql-version)  