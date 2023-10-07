> 목차

👉[3.5 연결지향형 트랜스포트 TCP](#35-연결지향형-트랜스포트-TCP)　   
　   [3.5.1 TCP 연결](#351-TCP-연결)　   　   
　   [3.5.2 TCP 세그먼트 구조](#352-TCP-세그먼트-구조)　   
　   [3.5.3 왕복시간(RTT) 예측과 타임아웃](#353-왕복시간RTT-예측과-타임아웃)　   
　   [3.5.4 신뢰적인 데이터 전송](#354-신뢰적인-데이터-전송)　   
　   [3.5.5 흐름 제어](#355-흐름-제어)　   
　   [3.5.6 TCP 연결 관리](#356-TCP-연결-관리)　            
　   
　   

## 3.5 연결지향형 트랜스포트 TCP


### 3.5.1 TCP 연결

> TCP(Transmission Control Protocol)의 특징

1. 전이중 서비스(full-deplex service, 양방향)
2. Point-to-Point(1:1 통신), 멀티캐스팅 불가
   => A와 B가 통신한다면, A->B로도 데이터가 흐를 수 있고, B->로도 데이터가 흐른다.
3. **Three-way handshake**


> Three-way handshake

1. 클라이언트 -> 서버  "TCP SYN(연결요청)"
2. 서버-> 클라이언트 "TCP SYN+ACK(긍정 응답확인)"
3. 클라이언트 -> 서버 "TCP ACK"


### 3.5.2 TCP 세그먼트 구조

![image](https://github.com/inpink/CS_Networking_Study/assets/108166692/f98ce280-6174-458f-9e98-3cb29cdc98d9)
1. 16비트 **출발지 포트 번호** => 앞서 배움
2. 16비트 **목적지 포트 번호** => 앞서 배움
3. 32비트(8바이트) **순서 번호 필드(sequence number field)** => 앞서 배움(3.4절)
4. 16비트 **인터넷 체크섬** => 앞서 배움(3.4절)
5. 32비트 **확인 응답 번호 필드(ACK num field)** => ACK도 번호를 가지고 있다! 3.5.4절에서 자세히 보자.
6. 16비트 **수신 윈도(receive window)** => 3.5.5절 "흐름제어"에서 사용된다.
7. 4비트 **헤더 길이 필드(header length field)** => TCP "헤더"의 "길이"
8. 가변적인 **옵션 필드(option field)** => 옵션이 있으면 추가적으로 담아줄 수 있음. 보통은 TCP의 길이가 20Byte로 맞춰주도록 옵션을 넣지 않아 비어있음
9. 6비트 **플래스 필드(flag field)** => CWR, ECE, URG ... 같은 부분이다.
10. 16비트 **긴급 데이터 포인터 필드(urgent data pointer field)** => 실제로 사용되진 는다. 긴급 데이터가 있으면, 수신 측 상위 계층에 알리기 위함이다.



### 3.5.3 왕복시간(RTT) 예측과 타임아웃

> RTT(round-trip time)

세그먼트 전송 -> ACK 수신
까지 걸리는 시간을 뜻한다. 

"타이머"설정 시, "타임아웃" 시간을 RTT보다 크게 해야한다.
너무 적게 시간을 할당하면, 어느쪽에서든 불필요한 재전송을 하게 된다. 

RTT는 보통 다음과 같이 측정한다.
~~~
EstimatedRtt=0.875 * EstimatedRtt + 0.125 * SampleRTT
~~~

재전송 타임아웃 주기는 다음과 같이 측정한다.
~~~
TimeOutInterval = EstimatedRtt + 4* DevRtt
~~~


### 3.5.4 신뢰적인 데이터 전송



### 3.5.5 흐름 제어



### 3.5.6 TCP 연결 관리
    
