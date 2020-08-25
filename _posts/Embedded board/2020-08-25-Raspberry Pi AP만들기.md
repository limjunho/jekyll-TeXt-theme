---
title: Raspberry Pi AP(Access Point)만들기
tags: Embeddedboard
---

라즈베리파이를 AP로 만드는 방법을 소개한다.  
쉽게말해 wifi를 만들 수 있는데 즉 스마트폰이든 노트북이든간에 접속이 가능하다.  
파일을 실행 시킬 수 있고, 로봇으로 동작시킬 수 있는등 활용이 무궁무진하다.    

[send me email](mailto:jewel7492@gmail.com) if you have any questions.

<!--more-->

---

```bash
$ sudo apt update
$ sudo apt upgrade -y
```
**update와 upgrade 진행**  

```bash
$ sudo apt-get install hostapd -y
$ sudo apt-get install dnsmasq -y
```
**hostapd는 RPI(Raspberry Pi)를 wifi ap mode로 설정할 때 사용한다.**  
**dnsmasq는 DHCP나 DNS를 설정할 때 사용한다.**  

```bash
$ sudo systemctl stop hostapd
$ sudo systemctl stop dnsmasq
```
**설정이 되지 않은 채 실행되지 않도록 정지한다.**  

```bash
$ sudo vi /etc/dghcpcd.conf
```
**dhcpcd 구성 파일을 수정한다.**  

```
interface wlan0
static ip_address=192.168.2.1/24
nohook wpa_supplicant
```
**파일의 가장 아래에 위의 내용을 추가한다.**  

```bash
$ sudo service dhcpcd restart
```
**dhcpcd를 재시작한다. (수정내용 적용)**  

```bash
$ sudo vi /etc/dnsmasq.conf
```
**/etc/dnsmasq.conf 파일을 수정한다.**  

```
interface=wlan0
dhcp-range=192.168.2.2,192.168.2.20,255.255.255.0,24h
```
**위의 내용을 /etc/dnsmasq.conf에 입력**  
**interface종류, IP할당 범위, 서브넷마스크, DHCP서버 동기시간등을 설정한다.**  

```bash
$ sudo vi /etc/hostapd/hostapd.conf
```
**무선 네트워크 설정을 하기 위해 위의 파일을 수정한다.**  

```
interface=wlan0
driver=nl80211
ssid=원하는 AP 이름
hw_mode=g
channel=7
wmm_enabled=0
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=원하는 AP 비밀번호
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
```
**ssid는 AP의 이름이며 wpa_passphrase는 비밀번호이다.**  
다른 내용은 드라이버나 채널, 암호화 방식등의 설정.  

```bash
$ sudo vi /etc/default/hostapd
```
**직전에 수정한 hostapd.conf파일의 위치를 시스템에게 알려주기 위해 /etc/default/hostapd파일을 수정한다.**  

```
DAEMON_CONF="/etc/hostapd/hostapd.conf"
```
**주석을 제거하고 경로를 기입한다.**  

```bash
$ sudo systemctl start hostapd
$ sudo systemctl start dnsmasq
```
**먼저 RPI를 reboot하고 위의 명령을 실행하여 hostapd와 dnsmasq를 시작한다.**  

###Failed to start hostapd.service: Unit hostapd.service is masked. 오류 발생 시
*** 오류가 발생하지 않으면 생략**  
```bash
$ sudo systemctl unmask hostapd.service
$ sudo systemctl start hostapd
```
**hostapd.service를 unmask하고 다시 hostapd를 시작**  

```bash
$ sudo vi /etc/sysctl.conf
```
**패킷 포워딩을 활성화 하기 위해 /etc/sysctl.conf를 수정**  

```
#net.ipv4.ip_forward=1
```
**위의 내용을 찾아 주석 제거**  

```bash
$ sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
$ sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"
```
**RPI가 연결한 인터넷을 RPI에 연결한 모든 기기들이 같이 이용할 수 있도록 함.**  
**그리고 설정을 저장**  

```bash
$ sudo vi /etc/rc.local
```
**부팅 시 이 설정을 적용하기 위해 /etc/rc.local파일을 수정**  

```
iptables-restore < /etc/iptables.ipv4.nat
sudo systemctl start hostapd
```
**위의 내용을 /etc/rc.local파일 맨 아래 exit 0 바로 윗줄에 입력**  
**설정을 저장하고 부팅 시 자동으로 hostapd를 시작하기 위한 내용이다.**  

<br />
![그림1](/assets/Embedded_board/RPI_to_AP/1.jpg){: width="70% height="70%"}  

**AP가 정상적으로 작동 확인**  

<br />
![그림2](/assets/Embedded_board/RPI_to_AP/2.jpg){: width="70% height="70%"}    

**AP에 연결**    

<br />
![그림3](/assets/Embedded_board/RPI_to_AP/3.jpg){: width="70% height="70%"}   

**AP에 연결 확인**  