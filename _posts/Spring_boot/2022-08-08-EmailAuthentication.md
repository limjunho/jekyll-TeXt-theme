---
title: Springboot 이메일 인증 구현
tags: Springboot
---
 
## Summry

본 문서에서는 인증번호를 생성해 이메일을 보내는 방법을 정리한다.  
* 본 문서에서 다룬 방법은 Gmail을 기준으로 작성됨.  

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## 구글 계정 설정

2022년 5월 기준 보안 **수준이 낮은 앱의 액세스 설정이 불가능**하게 되었다. 따라서 아래의 방법을 따라 앱 비밀번호를 생성해야 정상적인 메일 발송이 가능하다.  

**1. 구글에 로그인 한 후 오른쪽 위 동그란 프로필을 눌러서 Google 계정 관리 접속**  

![그림1](/assets/Spring_boot/mail/1.png)


**2. 왼쪽의 보안 탭으로 들어가서 2단계 인증을 활성화한다.**  
**3. 2단계 인증을 받았다면 앱 비밀번호를 클릭한다.**  

![그림2](/assets/Spring_boot/mail/2.png)

**4. 메일, windows 컴퓨터를 선택한다.**  

![그림3](/assets/Spring_boot/mail/3.png)

**5. 발급받은 앱 비밀번호를 사용한다.**  
(아래 그림의 앱 비밀번호는 포스팅용으로 발급하였으며 현재는 제거되었음.)  

![그림4](/assets/Spring_boot/mail/4.png)

## Gmail 이메일 발송 한도

보통 수신자가 수백명을 넘어가는 대량 발송 이메일은 대량 이메일 발송 서비스나 마케팅 자동화 솔루션을 활용해 발송 및 관리하는 것이 가장 효율적이다. 하지만 이런 전문 솔루션을 활용하기 위해서는 추가 비용이 들기 때문에 비용 절감을 위해 대부분의 소규모 기업들은 웹메일을 통한 대량 이메일 발송을 고려할 수 밖에 없다.  

**Gmail의 개인 계정의 경우 일일 전송한도는 하루(24시간)에 500개로 제한되어 있다.**  
Google에서는 각 이메일 주소를 각기 다른 이메일로 계산하고 있기 때문에 5명의 수신자에게 보낸 하나의 이메일을 5개의 이메일로 계산한다.  

* **1회 전송 한도**: 100개 (메일 당 최대 100명에게 이메일을 전송할 수 있다.)
* **일일 (24시간) 전송 한도**: 500개
* **메일 당 수신자**: 외부 500개

>Google Workspace (전 G Suite)를 활용하는 경우의 일일 전송한도는 하루(24시간)에 2,000개이다. 더불어 Google의 SMTP 서비스를 사용하는 경우 메일 당 최대 100명의 수신자에게 이메일을 전송할 수 있다.

### 타 메일 발송 한도

**네이버**  

* **시간 한도**: 시간당 최대 30회
* **1회 전송 한도**: 100개

**다음 / 다음스마트워크**  

* **1회 전송 한도**: 150개
* **일일 (24시간) 전송 한도**: 10,000개

## 1. Dependency 추가

**build.gradle**
```xml
dependencies {
     implementation 'org.springframework.boot:spring-boot-starter-mail'
}
```

## 2. EmailConfig

```java
import java.util.Properties;
 
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;
import org.springframework.mail.javamail.JavaMailSender;
import org.springframework.mail.javamail.JavaMailSenderImpl;
 
 
@Configuration
@PropertySource("classpath:email.properties")
public class EmailConfig {
 
    @Value("${mail.smtp.port}")
    private int port;
    @Value("${mail.smtp.socketFactory.port}")
    private int socketPort;
    @Value("${mail.smtp.auth}")
    private boolean auth;
    @Value("${mail.smtp.starttls.enable}")
    private boolean starttls;
    @Value("${mail.smtp.starttls.required}")
    private boolean startlls_required;
    @Value("${mail.smtp.socketFactory.fallback}")
    private boolean fallback;
    @Value("${AdminMail.id}")
    private String id;
    @Value("${AdminMail.password}")
    private String password;
 
    @Bean
    public JavaMailSender javaMailService() {
        JavaMailSenderImpl javaMailSender = new JavaMailSenderImpl();
        javaMailSender.setHost("smtp.gmail.com");
        javaMailSender.setUsername(id);
        javaMailSender.setPassword(password);
        javaMailSender.setPort(port);
        javaMailSender.setJavaMailProperties(getMailProperties());
        javaMailSender.setDefaultEncoding("UTF-8");
        return javaMailSender;
    }
    private Properties getMailProperties()
    {
        Properties pt = new Properties();
        pt.put("mail.smtp.socketFactory.port", socketPort);
        pt.put("mail.smtp.auth", auth);
        pt.put("mail.smtp.starttls.enable", starttls);
        pt.put("mail.smtp.starttls.required", startlls_required);
        pt.put("mail.smtp.socketFactory.fallback",fallback);
        pt.put("mail.smtp.socketFactory.class", "javax.net.ssl.SSLSocketFactory");
        return pt;
    }
}
```

## 3. properties

```properties
mail.smtp.auth=true
mail.smtp.starttls.required=true
mail.smtp.starttls.enable=true
mail.smtp.socketFactory.class=javax.net.ssl.SSLSocketFactory
mail.smtp.socketFactory.fallback=false
mail.smtp.port=465
mail.smtp.socketFactory.port=465
 
# admin 구글 계정
AdminMail.id = 구글 아이디 입력
AdminMail.password = 발급받은 앱 비밀번호를 입력
```

## 4. EmailService

```java
public interface EmailService {
    String sendSimpleMessage(String to)throws Exception;
}
```

## 5. EmailServiceImpl

[Service와 ServiceImpl을 분리하는 이유](https://limjunho.github.io/2021/08/12/service-serviceimpl.html)

```java
import java.util.Random;
 
import javax.mail.Message.RecipientType;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;
 
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.mail.MailException;
import org.springframework.mail.javamail.JavaMailSender;
import org.springframework.stereotype.Service;
 
@Service
public class EmailServiceImpl implements EmailService{
 
    @Autowired
    JavaMailSender emailSender;
 
    public static final String ePw = createKey();
 
    private MimeMessage createMessage(String to)throws Exception{
        System.out.println("보내는 대상 : "+ to);
        System.out.println("인증 번호 : "+ePw);
        MimeMessage  message = emailSender.createMimeMessage();
 
        message.addRecipients(RecipientType.TO, to);//보내는 대상
        message.setSubject("이메일 인증 테스트");//제목
 
        String msgg="";
        msgg+= "<div style='margin:20px;'>";
        msgg+= "<h1> 안녕하세요 임준호입니다. </h1>";
        msgg+= "<br>";
        msgg+= "<p>아래 코드를 복사해 입력해주세요<p>";
        msgg+= "<br>";
        msgg+= "<p>감사합니다.<p>";
        msgg+= "<br>";
        msgg+= "<div align='center' style='border:1px solid black; font-family:verdana';>";
        msgg+= "<h3 style='color:blue;'>회원가입 인증 코드입니다.</h3>";
        msgg+= "<div style='font-size:130%'>";
        msgg+= "CODE : <strong>";
        msgg+= ePw+"</strong><div><br/> ";
        msgg+= "</div>";
        message.setText(msgg, "utf-8", "html");//내용
        message.setFrom(new InternetAddress("properties에 입력한 이메일","limjunho"));//보내는 사람
 
        return message;
    }
 
    public static String createKey() {
        StringBuffer key = new StringBuffer();
        Random rnd = new Random();
 
        for (int i = 0; i < 8; i++) { // 인증코드 8자리
            int index = rnd.nextInt(3); // 0~2 까지 랜덤
 
            switch (index) {
                case 0:
                    key.append((char) ((int) (rnd.nextInt(26)) + 97));
                    //  a~z  (ex. 1+97=98 => (char)98 = 'b')
                    break;
                case 1:
                    key.append((char) ((int) (rnd.nextInt(26)) + 65));
                    //  A~Z
                    break;
                case 2:
                    key.append((rnd.nextInt(10)));
                    // 0~9
                    break;
            }
        }
        return key.toString();
    }
    @Override
    public String sendSimpleMessage(String to)throws Exception {
        // TODO Auto-generated method stub
        MimeMessage message = createMessage(to);
        try{//예외처리
            emailSender.send(message);
        }catch(MailException es){
            es.printStackTrace();
            throw new IllegalArgumentException();
        }
        return ePw;
    }
}
```

## 6. Controller

```java
@PostMapping("/emailConfirm")
public String emailConfirm(@RequestParam String email) throws Exception {

  String confirm = emailService.sendSimpleMessage(email);

  return confirm;
}
```

## 7. 메일 확인

* **query parameter로 인증번호를 받을 이메일 입력.**  
* **front에 반환한 인증 코드와 서버 터미널에 찍힌 인증 코드가 같은지 확인.**

![그림5](/assets/Spring_boot/mail/5.png)

**PC 메일 확인**

![그림6](/assets/Spring_boot/mail/6.png)

**모바일 메일 확인**

![그림7](/assets/Spring_boot/mail/7.jpg)

## Reference

> [[Spring Boot] 이메일 인증 회원가입하기 구현 - 먹여살릴꽁이가있습니다.](https://javaju.tistory.com/100)  
>[Java/SpringBoot 메일을 보내는 방법들 - 코딩하는보기](https://code-boki.tistory.com/48)  
>[이메일 대량 발송에 대한 모든 것! - 아웃룩 토탈 애드인 솔루션, 토틀 (totle)](https://blendthings.tistory.com/entry/%EC%98%A4%ED%94%BC%EC%8A%A4-%ED%94%8C%EB%9F%AC%EA%B7%B8-%EC%9D%B4%EB%A9%94%EC%9D%BC-%EB%8C%80%EB%9F%89-%EB%B0%9C%EC%86%A1%EC%97%90-%EB%8C%80%ED%95%9C-%EB%AA%A8%EB%93%A0-%EA%B2%83)  
>[메일 주고받기 한도 - Gmail 고객센터](https://support.google.com/mail/answer/22839?hl=ko#zippy=%2C%EB%A9%94%EC%9D%BC-%EC%A0%84%EC%86%A1-%EC%A0%9C%ED%95%9C%EC%97%90-%EB%8F%84%EB%8B%AC%ED%96%88%EC%8A%B5%EB%8B%88%EB%8B%A4)  