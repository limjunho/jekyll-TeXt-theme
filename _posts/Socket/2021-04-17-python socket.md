---
title: Python TCP/UDP socket
tags: Network Python Socket
---

## 개요  

TCP와 UDP의 전송 방식이 다른것처럼, 소켓통신에도 방식이 다르다. 데이터 타입은 TCP는 Stream 방식이고 UDP는 Datagram 방식이다.  
본 문서에서는 클라이언트와 서버를 만들어 TCP/UDP Socket 통신을 구현한다.

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

## TCP Socket  

**TCP Socket 통신 과정**  
1. 서버에서 소켓을 열고 클라이언트의 연결 요청 대기.  
2. 클라이언트에서 소켓을 열고 서버에 연결 요청.
3. 서버에서 클라이언트의 소켓 연결 요청을 수락.
4. 데이터 송/수신

### Server code  

```python
from socket import *                    

host = "127.0.0.1"                      ### 사용할 임의 IP와 Port를 지정한다.
port = 7001                                     

serverSocket = socket(AF_INET, SOCK_STREAM)   ### TCP 소켓을 지정한다 (STREAM)	          
serverSocket.bind((host,port))          ### IP,Port Mapping                
serverSocket.listen(1)                  ### 최대 연결 수를 지정한다       
print("Waiting for Connection..")

connectionSocket,addr = serverSocket.accept()           
print(str(addr),"by Connetcion..")                  

data = connectionSocket.recv(1024)      ### 수신할 데이터 크기
print("Server : Data Received :", data.decode("utf-8"))     

connectionSocket.send("Hello, i'm server!".encode("utf-8"))   ### 데이터 송신

serverSocket.close()  
```

* socket(AF_INET, SOCK_STREAM) : 소켓 객체를 만든다.
* socket.bind(address) : 만들어진 서버 소켓을 호스트 IP 및 Port를 튜플 형태로 맵핑한다.
* socket.listen() : 요청 대기 상태를 설정한다,.
* socket.accept() : 요청이 연결되면 소켓을 반납한다.
* socket.send(byte) or socket.sendall(byte) : 데이터 송신
* socket.recv(bufsize) : 데이터 수신 (bufsize는 한번에 수신되는 데이터 크기이다)
* socket.close() : 연결 종료

### Client code  

```python
from socket import *                           

ip = "127.0.0.1"                               
port = 7001

clientSocket = socket(AF_INET, SOCK_STREAM)		### TCP 소켓을 지정한다 (STREAM)	
clientSocket.connect((ip,port))					### IP,Port Mapping

print("Connection Success..")
clientSocket.send("Hello, i'm client!".encode("utf-8"))		### 데이터 송신

data = clientSocket.recv(1024)					### 수신할 데이터 크기 

print("Client : Data Received : ",data.decode("utf-8"))          
clientSocket.close()   
```

* socket.connect(address) : IP를 튜플형태로 지정해, 서버소켓에 연결한다.
* socket.send(byte) or socket.sendall(byte) : 데이터 송신
* socket.recv(bufsize) : 데이터 수신 (bufsize는 한번에 수신되는 데이터 크기이다)

## UDP Socket  

**UDP Socket 통신 과정**
1. 서버/클라이언트 모두 소켓을 연다.
2. 클라이언트에서 서버 호스트의 포트로 데이터전송
3. 서버에서 생성된 데이터그램을 읽어 소켓을 이용해 클라이언트 호스트와 포트로 반환
4. 클라이언트에서 소켓으로 받은 데이터그램을 읽어 값을 획득

### Server code

```python
from socket import *

sock = socket(AF_INET, SOCK_DGRAM)             ### UDP 소켓을 지정한다 (DGRAM)
sock.bind(('127.0.0.1',7001))	                 ### IP 포트번호 지정

data, addr = sock.recvfrom(200)                ### 데이터 수신 대기 (크기)
sock.sendto(data, addr)                         ### 데이터를 클라이언트에 송신

sock.close()    
```
* socket(AF_INET, SOCK_DGRAM) : 소켓 객체를 만든다. (Diagram)
* socket.bind(address) : 만들어진 서버 소켓을 호스트 IP 및 Port를 튜플 형태로 맵핑한다.
* sock.recvfrom() : 데이터 수신대기를 한다.
* data.decode() : Client에서 문자데이터를 보낸다면 bytes type으로 메시지가 오게 되는데 받은 메시지를 출력하고 싶다면 받은문자열.decode()를 통해 decoding 해주어야한다.
* socket.close() : 연결 종료

### Client code

```python
from socket import *

sock = socket(AF_INET, SOCK_DGRAM)    ### UDP 소켓을 지정한다 (DGRAM)
sock.sendto("Hello!".encode(), ('127.0.0.1',7001))    ### 데이터 송신

data, addr = sock.recvfrom(200)                            ### Server로 부터 수신한 데이터와 주소
print(data.decode(), addr)
```

* socket(AF_INET, SOCK_DGRAM) : 소켓 객체를 만든다. (Diagram)
* socket.bind(address) : 만들어진 서버 소켓을 호스트 IP 및 Port를 튜플 형태로 맵핑한다.
* sock.recvfrom() : 데이터 수신대기를 한다.
* data.decode() : Client에서 문자데이터를 보낸다면 bytes type으로 메시지가 오게 되는데 받은 메시지를 출력하고 싶다면 받은문자열.decode()를 통해 decoding 해주어야한다.
* socket.close() : 연결 종료

## 참고

> [TCP/UDP 소켓 만들며 개념 익히기 (Python) - loanshark](https://velog.io/@bhs9610/TCPUDP-%EC%86%8C%EC%BC%93-%EB%A6%AC%EB%A7%88%EC%9D%B8%EB%93%9C-Python-%EC%9E%91%EC%84%B1%EC%A4%91)  