---
title: MySQL 비밀번호 변경
tags: MySql
---

## Summry

본 문서에서는 windows환경에서 mysql password를 변경하는 방법을 정리한다.

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

**root로 로그인 후 아래 명령 수행**  

```sql
ALTER user 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '변경 비밀번호';
FLUSH PRIVILEGES;
```

## Reference