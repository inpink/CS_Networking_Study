## ICMP(Internet Control Message Protocol)

**호스트와 라우터가 서로 네트워크 계층(3) 정보를 주고받기 위해 사용한다**　   
　   
ICMP 메시지는 IP 데이터그램(3)에 담겨 전송된다. 3계층의 프로토콜이라는 것이다. 　   
　   
ICMP 메세지에는 **타입 필드**와 **코드 필드**가 있다.　   
또, ICMP 메세지의 발생 원인이 된 **IP 데이터그램**의 **원본 IP 헤더 + 데이터부분의 첫 8바이트**를 담고 있다.　   
(송신자로 하여금 오류가 발생된 패킷을 알 수 있게 하기 위해서이다.)　   
　   
아래와 같은 ICMP 메시지 종류가 있다.　   
![image](https://github.com/inpink/CS_Networking_Study/assets/108166692/94d4a01b-d34b-414d-98f8-f80f240c03e8)　   
　   
=> **오류 발생**을 보고하기도 하고,　   
**ping 에코 요청** & **ping 에코 응답**, 　   
**혼잡 제어(출발지 억제 메세지)**, 　   
**TTL 만료에 따른 데이터그램 폐기** 　   
등에 사용된다.　   
　   
　   
> ping 프로그램

ping(Packet Internet Groper)은, 컴퓨터 네트워크 상태를 점검, 진단하라는 명령이다.　   
