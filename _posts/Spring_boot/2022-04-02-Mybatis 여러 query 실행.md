---
title: Mybatis 여러 query 실행
tags: MySql Springboot
---

## Summry

본 문서에서는 Mybatis에서 insert와 같은 query를 여러 개 실행시키는 방법을 정리한다.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## application.propertise 수정

아래와 같이 DB 연결 부분에서 allowMultiQueries=true를 붙여주면 mybatis에서 여러 개의 query를 실행시킬 수 있다.  

```spring.datasource.url: jdbc:mysql://localhost:3306/testdb?characterEncoding=utf8&allowMultiQueries=true```

## Mapper 예시

```sql
<insert id="userjoin" parameterType="org.test.gradle.testproject.model.aws.dto.user.UserJoin.user_join_dto">
    INSERT INTO user_information(user_index, user_name, email, phone, nickname, device_token, join_date)
    VALUES(#{user_index}, #{user_name}, #{email}, #{phone}, #{nickname}, #{device_token}, #{join_date});

    INSERT INTO user_account(user_index, user_id, password)
    VALUES((SELECT user_index FROM user_information ORDER BY user_index DESC LIMIT 1), #{user_id}, #{password});

    INSERT INTO user_permit(user_index, email_permit, sms_permit, push_permit)
    VALUES((SELECT user_index FROM user_information ORDER BY user_index DESC LIMIT 1), 0, 0, 0)
  </insert>
```

한 개의 query를 작성할 때에는 세미콜론(;)을 query 끝에 붙이지 않아도 되지만 여러 개의 query를 작성할 때에는 세미콜론(;)을 사용해 주어야 한다.  

위의 예제는 회원가입을 위한 query로 유저 정보, 계정 정보, 알림허용 여부를 각각 다른 테이블에 INSERT한다.

## Reference

> [[MySQL & Mybatis] mybatis에서 insert 쿼리 등 여러개 실행시키는법 - IT공부블로그](https://kimtaekju-study.tistory.com/279)  