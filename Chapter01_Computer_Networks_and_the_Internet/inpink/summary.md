> 목차

[이 책의 특징](#이-책의-특징)　   
[네트워크의 발전](#네트워크의-발전)　   

　   
　   
　   
　   
　   
# 들어가며

## 이 책의 특징 

1. 네트워크 계층을 상향식이 아닌 '하향식'으로 설명한다.　   
상향식보다 하향식으로 설명하는 것이 더 효과적이다.　   

2. 컴퓨터 네트워킹에서 가장 크게 성장하는 분이얀 **애플리케이션 계층**을 강조한다.　   
웹, 미디어 등 혁신적인 일들은 주로 애플리케이션 게층에서 일어났다.　   
　   
3. 인터넷 중심(Featuring the Internet)　   
인터넷의 발전은 혁명적인 많은 일들을 발생시켰으며 네트워크의 발전도 이끌었기 때문이다.　   
　   
4. **원리**를 설명한다　   
　   
　   
　   
## 네트워크의 발전　   

1. 컴퓨터 개발　   
2. 근접한 컴퓨터끼리의 연결　   
3. 컴퓨터의 광범위한 보급 　   
4. 단순히 근접한 컴퓨터의 연결뿐만 아니라, 먼 지역에서도 글로벌한 연결성이 요구됨　   
5. 패킷 교환 방식(가상 회선 방식, 데이터그램 방식)과 인터넷 프로토콜(TCP/IP 등) 등이 개발됨　   
6. 현대에는 텍스트를 넘어 오디오, 비디오, 멀티미디어 등의 용량이 큰 데이터를 고속으로 전송하기 위해 물리적, 응용적 기술이 연구, 개발되고 있음　   
7. 컴퓨터, 서버들뿐만 아니라 핸드폰, 사물들도 네트워크에 연결되며 확장되고 있음. 　   
　   
　   
# 1. 컴퓨터 네트워크와 인터넷

> 이번 단원의 목적

**컴퓨터 네트워크**와 **인터넷**의 큰 숲을 본다　   
　   
　   
## 1.1 인터넷이란 무엇인가?

두 가지 방법으로 설명할 수 있다.　   
1. 인터넷을 구성하는 기본적인 '하드웨어'와 '소프트웨어'적인 구성요소를 설명 [(1.1.1)](#111-구성요소로-본-인터넷)　   
2. 네트워킹 인프라스트럭쳐(infrastructure) 관점에서 설명 [(1.1.2)](#112-서비스-측면에서-본-인터넷)　   
　   
　   
### 1.1.1 구성요소로 본 인터넷

> 인터넷

전 세계의 수십억 개의 장치들을 연결하는 **컴퓨터 네트워크**　　   
'하드웨어'와 '소프트웨어'의 관점에서 인터넷을 보자.　   
　   
　   
> 호스트(host), 종단 시스템(end system)　   

네트워크에 참여하는 모든 각 **장치**를 의미한다.　   
과거에는 컴퓨터, 서버 위주로 네트워크가 이루어졌지만, 현재에는 핸드폰, 사물들도 네트워크에 연결되며 확장되고 있다.　   
　   
　   
> 통신 링크(commmunication link)와 패킷 스위치(packet switch)　   

각 호스트들은 통신 링크와 패킷 스위치를 이용하여 **네트워크에 연결**된다.　   
　   
💬 통신 링크 : 동축 케이블, 구리선, 광케이블, 라디오 스펙트럼 등 **물리 매체(physical media)** [(1.2)](12-네트워크의-가장자리)　   
wifi같은 무선 통신 링크(Wireless Communication Link)도 있다.　   
　   
💬 패킷 교환기(패킷 스위치) : 입력받은 패킷을 다른 패킷 교환기로 전송하는 역할을 한다.　   
라우터(router), 링크 계층 스위치(link-layer switch)가 대표적인 교환기이다. 　   
　   
💬 네트워크 경로(route, path) : 패킷이 **송신 종단 시스템**에서 시작해서 **송신 종단 시스템**까지 가며 거친 **통신 링크와 패킷 스위치**를 이은 길　   
　   
　   
😀 　   
각 호스트에서, 데이터는 **패킷**으로 변환되어 전송되는데,　   
데이터 -> 여러 개의 세그먼트들로 나누어짐　   
각 세그먼트+헤더들(headers) -> 패킷(packet)　   
패킷들 -> 차례로 통신 링크, 패킷 스위치를 타고 전송됨　   
bps(bit per second, 초당 비트 수)의 전송률(transmission rate)로 전송된다.　   
**송신 종단 시스템**에서 보낸 패킷은, **수신 종단 시스템**에서 받아서, 데이터로 분해하고 조립해서 사용한다.　   
　   
　   
> **3p 그림 1.1**

다양한 종류의 호스트들이 있다. 　   
전체 네트워크는 여러 개의 작은 네트워크(홈 네트워크, 기업 네트워크, 이동 네트워크,   들이 연결된 구조이다. 　   
각 호스트들은 **통신 링크**를 타고 패킷형태로 전송되고 있다. 유선도 있고, 무선도 있다.　   
이 그림에서 패킷들의 전송은 **패킷 스위치(링크 계층 스위치, 라우터)** 를 이용하여 다른 전달되고 있다. 그리고 라우터가 라우터끼리 서로 연결되며 네트워크를 구성하고 있다.　   
　   
　   
> **ISP(internet service provider)**

인터넷 서비스 제공자(isp)는, 인터넷 회사를 뜻한다.　   
SKT, LG 등 말 그대로 인터넷을 제공해주는 provider를 뜻한다.　   
다양한 종류의 네트워크 접속을 지원해준다.　   
각 isp는 **패킷 스위치**와 **통신 링크**로 이루어진다.　   
　   
**isp끼리도 연결되어야 한다**　   
하위 계층 isp들이 서로 연결되어 상위 계층(국가 단위) isp로 연결된다. 그럼, 상위 계층 isp끼리 연결되는 구조이다. 　   
거리가 먼 국가 단위는, 해저 광 케이블(통신 링크의 일종) 등을 이용한다. 　   
isp와 관련된 내용은 [(1.3)](#13-네트워크-코어)에서 더 알아본다.　   
　   
　   
> 표준을 정하다

인터넷은 규모가 굉장히 크다. 그런만큼 **표준**이 꼭 필요하다.　   
각각의 프로토콜이 무엇을 해야하는지 **표준 프로토콜**을 만들어 놓고 그 표준을 따라서 개발해야, 수 많은 제품들이 상호호환이 가능하다.　   
프로토콜을 번역하면 "규약"이라는 뜻을 가진다.　   

인터넷 표준은 **IETF(Internet Engineering Task Force)** 에서 개발하며 **RFC(requests for comment) 표준 문서** 를 매우 기술적이고 상세하게 작성해놓는다. TCP, IP, HTTP, SMTP 등의 프로토콜이 규정되어있다.　   
　   
이더넷과 무선 와이파이(WiFi)에 대한 표준은 **IEEE 802 LAN 표준위원회**에서 개발한다.　   
이렇듯, 다양한 프로토콜을 다양한 기구들이 표준을 정하고 기술해놓고 있다.　   
　   
　   
　   
### 1.1.2 서비스 측면에서 본 인터넷

> 인터넷

네트워킹 인프라스트럭쳐(infrastructure) 관점에서 인터넷을 보자.

웹, 게임, 넷플릭스, sns 등이 **인터넷 애플리케이션**다. 
그리고 이러한 인터넷 애플리케이션이 돌아갈 수 있도록 **인프라스트럭쳐(infrastructure)** 역할을 하는 것이 **인터넷**이다.



## 1.2 네트워크의 가장자리







![image](https://github.com/inpink/CS_Networking_Study/assets/108166692/28f59c4a-674a-4a47-a4e0-2fc53b75a394)


![image](https://github.com/inpink/CS_Networking_Study/assets/108166692/f2d2f88c-1d72-4dca-b81c-16fe22138b8d)