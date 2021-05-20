---
title: INADDR_ANY
tags: Socket
---

## Summry  

네트워크 소켓 프로그래밍 시 INADDR_ANY의 의미

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## INADDR_ANY 의미

**INADDR_ANY는 자동으로 이 컴퓨터에 존재하는 랜카드 중 사용가능한 랜카드의 IP주소를 사용하라는 의미이다.**  

## INADDR_ANY 사용 이유

예를들어 2개의 랜카드가 설치되어 있고 각각의 ip 주소가 192.168.0.1, 192.168.0.2 라 가정할 때 외부에서 192.168.0.1 로 데이터를 보내나 192.168.0.2 로 데이터를 보내나 내 컴퓨터로 오는건 같다. 하지만 프로그램에서 ip 주소를 192.168.0.1 로 등록했다면 192.168.0.2 로 들어오는 데이터는 수신하지 않는다.  
즉, 같은 컴퓨터임에도 불구하고 ip 주소가 다르면 처리하지 않는다.  

**INADDR_ANY 를 사용하면 192.168.0.1 로 오는 데이터나 192.168.0.2 로 오는 데이터를 모두 처리한다. 즉 여러 ip주소에 들어오는 데이터를 모두 수신한다.**  


## Reference

> [네트워크 소켓 프로그래밍 시 INADDR_ANY의 의미 - Grind away](http://grindawayat.blogspot.com/2015/05/inaddrany.html)  