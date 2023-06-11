# 2.7 소켓 프로그래밍: 네트워크 애플리케이션 생성
네트워크 애플리케이션은 2개의 종단 시스템에 존재하는 클라이언트 프로그램과 서버 프로그램으로 구성된다

두 프로그램을 수행하면 클라이언트와 서버 프로세스가 생성되고,
두 프로세스가 소켓으로부터 읽고(read), 소켓에 쓰기(write)를 통해 서로 통신한다

클라이언트-서버 애플리케이션에는 두 가지 형태가 있다
1. RFC에 정의된 표준 프로토콜을 구현하는 클라이언트-서버 애플리케이션이다. 클라이언트와 서버 프로그램은 RFC에 정의된 규칙을 따라야 한다. RFC의 규칙을 철저히 따른다면 제대로 상호작용할 것이다
2. 개인의 독점적인 네트워크 애플리케이션이다. 클라이언트와 서버 프로그램은 RFC 또는 다른 곳에 공식적으로 출판되지 않은 애플리케이션 계층 프로토콜을 채택한다. 다른 독립 개발자는 상호작용하는 코드를 개발할 수 없다

애플리케이션이 TCP / UDP 중 무엇을 이용하는지 결정하는 것이 중요하다

RFC에 정의된 프로토콜을 구현할 때, 프로토콜과 연관된 잘 알려진 `포트 번호`를 사용해야 한다
또한, 독점적인 애플리케이션을 구현할 때, 포트 번호를 사용하지 않도록 유의해야 한다

## 2.7.1 UDP를 이용한 소켓 프로그래밍
- 송신 프로세스가 데이터의 패킷을 소켓 문밖으로 밀어내기 전에, UDP를 사용할 때 먼저 패킷에 `목적지 주소`를 붙여넣어야 한다
- 패킷이 송신자의 소켓을 통과한 후, 인터넷은 이 목적지 주소를 이용하여 그 패킷을 수신 프로세스에 있는 소켓으로 `라우트`한다
- 패킷이 수신 소켓에 도착하면, 수신 프로세스는 소켓을 통해 그 패킷을 추출하고 패킷의 콘텐츠를 조사하고 적절한 동작을 취한다

### 목적지 주소
패킷에 목적지 주소를 포함함으로써 인터넷 라우터는 목적지 호스트로 인터넷을 통해 패킷을 라우트할 수 있다.

호스트는 각자 `IP 주소`를 갖는다.
그러나, 목적지 호스트 내의 특정한 소켓을 식별할 필요가 있다.

소켓이 생성될 때 `포트 번호(port number)`라고 하는 식별자가 소켓에 할당된다.

즉, 송신 프로세스는 목적지 호스트의 `IP 주소`와 목적지 소켓의 `포트 번호`로 구성된 목적지 주소를 붙인다

UDP에서는 하부 운영체제가 송신자의 출발지 주소(출발지 호스트의 IP 주소와 출발지 소켓의 포트 번호)를 자동으로 붙여 실행한다

### UDPClient.py
```
# socket 모듈이다. 프로그램 내에서 소캣을 생성할 수 있다
from socket import *

# 서버의 IP 주소 혹은 서버의 호스트 이름을 할당한다. 호스트 이름을 사용하는 경우 IP 주소를 얻기 위해 DNS 검색이 자동으로 수행된다
serverName = 'hostname'
serverPort = 12000

# 클라이언트 소캣을 생성한다. AF_INEF는 IPv4를 사용하고 있음을 나타내고, SOCK_DGRAM은 UDP 소켓임을 의미한다
client Socket = socket(AF_INET, SOCK_DGRAM)

# 메시지를 입력 받는다
message = input('Input lowercase sentence:')

# encode()는 문자열 타입의 메시지를 바이트 타입으로 변환한다.
# sendto()는 목적지 주소 (serverName, serverPort)를 메시지에 붙이고, 그 패킷을 프로세스 소켓인 clientSocket으로 보낸다
# 자동으로 클라이언트 주소도 패킷에 붙여 보내진다
clientSocket.sendto(message.encode(), (serverName, serverPort))

# 패킷 데이터는 modifiedMessage에 할당되고 패킷의 출발지 주소(서버의 IP 주소와 포트 번호)는 serverAddress에 할당된다
# recvfrom()은 2048의 버퍼 크기로 받아들인다
modifiedMessage, serverAddress = clientSocket.recvfrom(2048)

# 메시지를 바이트에서 문자열로 변환한 다음 modifiedMessage를 출력한다
print(modifiedMessage.decode())

# 소켓를 닫는다
clientSocket.close()
```

### UDPServer.py
```
from socket import *

# 포트 번호
serverPort = 12000

# UDP 소켓 생성
serverSocket = socket(AF_INET, SOCK_DGRAM)

# 12000 포트 번호를 소켓에 할당. 서버 IP 주소의 12000 포트로 패킷을 보내면 해당 소켓으로 패킷이 전달된다
serverSocket.bind(('', serverPort))

print("The server is ready to receive")

while True:
  
  # 패킷이 서버의 소켓에 도착하면, 패킷의 데이터는 message에 할당되고, 패킷의 출발지 주소는 clientAddress에 할당된다
  # 출발지 주소 정보를 통해 서버는 응답을 어디로 보내야할지 알게된다
  message, clientAddress = serverSocket.recvfrom(2048)
  
  # 클라이언트로부터 라인을 받아서 upper()를 이용하여 대문자로 변환한다
  modifiedMessage = message.decode().upper()
  
  #클라이언트의 주소(IP 주소와 포트 번호)를 대문자로 변환된 메시지에 붙이고, 그 결과로 만들어진 패킷을 서버의 소켓으로 보낸다
  serverSocket.sendto(modifiedMessage.encode(), clientAddress)
```

---
## 2.7.2 TCP 소켓 프로그래밍
TCP는 연결지향 프로토콜로, 클라이언트와 서버가 서로에게 데이터를 보내기 전에 먼저 `TCP 연결`을 설정할 필요가 있다

TCP 연결을 생성할 때 클라이언트 소켓 주소(IP 주소와 포트 번호)와 서버 소켓 주소(IP 주소와 포트 번호)를 연결한다

TCP 연결이 설정된 후, 데이터를 보내려면 소켓을 통해 데이터를 TCP 연결로 보내면 된다

### 환영 소켓과 연결 소켓
서버 프로세스가 수행되면 클라이언트 프로세스는 서버로의 TCP 연결을 시도하는데, 이는 클라이언트 프로그램에서 `TCP 소켓`을 생성함으로써 가능하다

**클라이언트는 TCP 소켓을 생성할 때** 서버의 있는 `환영(welcome) 소켓`의 주소, 즉 서버의 IP 주소와 소켓의 포트 번호를 명시한다

소켓을 설정한 후 클라이언트는 세 방향 핸드셰이크를 하고 서버와 TCP 연결을 설정한다
(세 방향 핸드셰이크는 프로그램에서 전혀 인식하지 못한다)

세 방향 핸드셰이크 동안 **서버는 해당 클라이언트에게 지정되는 새로운 소켓**을 생성한다. 이를 `연결 소켓(connectionSocket)`이라 한다

- 환영 소켓: 서버와 통신하고자 하는 모든 클라이언트가 초기에 접속하는 곳
- 연결 소켓: 각 클라이언트와 통신을 위해 차후에 서버쪽에 새롭게 생성되는 소켓

애플리케이션의 관점에서 클라이언트의 소켓과 서버의 연결 소켓은 `파이프(pipe)`에 의해 직접 연결된다

파이프를 통해 클라이언트 프로세스는 자신의 소켓으로 임의의 바이트를 보낼 수 있으며,
보낸 순서대로 서버 프로세스가 바이트를 (연결 소켓을 통해) 수신하도록 TCP가 보장한다

TCP는 클라이언트와 서버 프로세스 간에 신뢰적(reliable) 서비스를 제공한다

### TCPClient.py
```
from socket import *

serverName = 'serverName'
serverPort = 12000

# 클라이언트 소켓을 생성한다. SOCK_STREAM은 TCP 소켓임을 의미한다
# UDP와 마찬가지로 따로 출발지 주소를 명시하지 않는다 (운영체제가 대신 해준다)
clientSocket = socket(AF_INET, SOCK_STREAM)

# 클라이언트와 서버 간에 TCP 연결을 시작한다. connect()의 파라미터는 연결의 서버 쪽 주소다
# 이 라인이 수행된 후에 세 방향 핸드셰이크가 수행되고 TCP 연결이 설정된다
clientSocket.connect((serverName, serverPort))

# 사용자로부터 문장을 획득한다. 사용자가 리턴 키를 입력하여 문장을 마칠 때까지 문자를 계속해서 모은다
sentence = input('Input lowercase sentenct:')

# sentence를 클라이언트 소켓을 통해 TCP 연결로 보낸다. 패킷을 명시적으로 생성하지 않으며 목적지 주소를 붙이지 않는다
# 클라이언트 프로그램은 sentence에 있는 바이트를 TCP 연결에게 제공한다
clientSocket.send(sentence.encode())

# 서버로부터 온 문자를 modifiedSentence에 모은다
modifiedSentence = clientSocket.recv(1024)
print('From Server: ', modifiedSentence.decode())

# 소켓을 닫고 TCP 연결을 닫는다. 클라이언트의 TCP가 서버의 TCP에게 TCP 메시지를 보내게 한다
clientSocket.close()
```

### TCPServer.py
```
from socket import *

serverPort = 12000

# TCP 소켓 생성
serverSocket = socket(AF_INET, SOCK_STREAM)

# 서버의 포트 번호를 소켓과 연관시킨다
serverSocket.bind(('', serverPort))

# 서버가 클라이언트로부터의 TCP 연결 요청을 듣도록 한다
# 파라미터는 큐잉되는 연결의 최대 수이다
serverSocket.listen(1)

print('The server is ready to receive')

while True:
  # 클라이언트가 TCP 연결 요청을 하면 accept()를 시작해서 클라이언트에게 지정된 connectionSocket인 새로운 소켓을 서버에 생성한다
  # TCP 연결이 설정되었으므로 클라이언트와 서버는 이 연결을 통해 서로에게 바이트를 보낼 수 있다
  connectionSocket, addr = serverSocket.accept()
  
  sentence = connectionSocket.recv(1024).decode()
  
  capitalizedSentence = sentence.upper()
  
  # 클라이언트에게 수정된 문장을 보낸다
  connectionSocket.send(capitalizedSentence.encode())
  
  # 연결 소켓을 닫는다.
  # 그러나 serverSocket이 열려 있기 때문에 다른 클라이언트가 서버에 연결을 요청할 수 있고 서버에게 수정할 문장을 보낼 수 있다
  connectionSocket.close()
```
