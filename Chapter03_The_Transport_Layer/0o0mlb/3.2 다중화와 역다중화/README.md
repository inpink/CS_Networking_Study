# 3.2 다중화와 역다중화
> 트랜스포트 계층의 다중화와 역다중화란 네트워크 계층이 제공하는 `호스트 대 호스트 전달` 서비스에서 호스트에서 동작하는 애플리케이션에 대한 `프로세스 대 프로세스 전달` 서비스로 확장하는 것이다.

트랜스포트 계층은 호스트에서 동작하는 해당 애플리케이션 프로세스에게 이 **세그먼트의 데이터를 전달**하는 의무를 진다

>`소켓(socket)`은 네트워크에서 프로세스로 데이터를 전달하고, 또한 프로세스로부터 네트워크로 데이터를 전달하는 출입구 역할을 한다.

그러므로 수신 측 호스트의 **트랜스포트 계층**은 실제로 데이터를 직접 프로세스로 전달하지 않고 중간 매개자인 **소켓에게 전달**한다.

수신 측 호스트에 **하나 이상의 소켓**이 있을 수 있으므로, 각각의 소켓은 **하나의 유일한 식별자**를 갖는다

수신 측 트랜스포트 계층은 수신 소켓을 식별하기 위해 세그먼트에 필드를 검사한다.
그리고 이 세그먼트를 해당 소켓으로 보낸다.

트랜스포트 계층 **세그먼트의 데이터를 올바른 소켓으로 전달**하는 작업을 `역다중화(demultiplexing)`라고 한다

출발지 호스트에서 소켓으로부터 데이터를 모으고, 이에 대한 세그먼트를 생성하기 위해 각 데이터에 헤더 정보로 캡슐화하고, 그 **세그먼트들을 네트워크 계층으로 전달**하는 작업을 `다중화(multiplexing)`라고 한다

트랜스포트 계층 다중화에는 두 가지 요구사항이 있다
1. 소켓은 유일한 식별자를 갖는다
2. 각 세그먼트는 세그먼트가 전달될 적절한 소켓을 가리키는 **출발지 포트 번호 필드**와 **목적지 포트 번호 필드**를 갖는다

각각의 포트번호는 `16비트` (0~65535)이고, 0~1023까지의 포트 번호는 **잘 알려진 포트 번호**라고 하여 사용을 엄격하게 제한하고 있다.

HTTP(포트 번호 80)와 FTP(포트 번호 21)처럼 잘 알려진 애플리케이션 프로토콜에서 사용되도록 예약되어 있다

- 호스트의 각 소켓은 `포트 번호`를 할당받는다.
- 세그먼트가 호스트에 도착하면, 트랜스포트 계층은 세그먼트 안의 `목적지 포트 번호를 검사`하고 상응하는 소켓으로 세그먼트를 보낸다
- 세그먼트의 데이터는 소켓을 통해 해당되는 프로세스로 전달된다

### 비연결형 다중화와 역다중화
프로세스들은 각각 `UDP 소켓`과 그와 연관된 `포트 번호`를 갖는다.
네트워크로부터 UDP 세그먼트들이 도탁하면, 수신 호스트는 `세그먼트의 목적지 포트`를 검사하여 세그먼트를 적절한 소켓으로 보낸다 : **역다중화**

UDP 소켓은 `목적지 IP 주소`와 `목적지 포트 번호`로 구성된다

출발지 포트 번호는 `회신 주소`의 한 부분으로 사용되어 수신 호스트가 송신 호스트에게 다시 보내기 원할 때 목적지 포트 번호를 출발지 포트 번호로부터 가져온다

### 연결지향형 다중화와 역다중화
TCP 소켓은 `출발지 IP 주소`, `출발지 포트 번호`, `목적지 IP 주소`, `목적지 포트 번호`로 구성된다.
네트워크로부터 호스트에 TCP 세그먼트가 도착하면, 호스트는 해당되는 소켓으로 세그먼트를 전달(역다중화)하기 위해 4개의 값을 모두 사용한다.

즉, 송신 호스트에서 여러 개의 세그먼트를 같은 수신 호스트에게 보내더라도 **출발지 포트 번호**가 다르기 때문에 올바르게 역다중화할 수 있다

### 웹 서버와 TCP
프로세스는 각자 `연결 소켓`을 가지며, 이 연결 소켓을 통해 HTTP 요청을 수신하고 HTTP 응답을 전송한다

실제로, 오늘날의 많은 고성능 웹 서버는 하나의 프로세스만을 사용한다.
그러면서, 각각의 새로운 클라이언트 연결을 위해 새로운 연결 소켓과 함께 새로운 스레드를 생성한다
