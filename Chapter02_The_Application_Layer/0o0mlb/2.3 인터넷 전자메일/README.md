# 2.3 인터넷 전자메일
- 사용자 에이전트(user agent)
- 메일 서버(mail server)
- SMTP(Simple Mail Transfer Protocol)

### 사용자 에이전트
- 사용자 에이전트는 사용자가 메시지를 읽고, 응답하고, 전달하고, 저장하고, 구성하게 해준다
- 사용자 에이전트는 메시지를 메일 서버로 보내고, 거기서 메시지는 메일 서버의 출력 메시지 큐에 들어간다
- 사용자 에이전트는 메일 서버에 있는 메일박스에서 메시지를 가져온다

### 메일 서버
- 전자메일 인프라스트럭처의 중심이다
- 각 수신자는 메일 서버 안에 `메일박스`를 갖고 있다
- 일반 메시지는 송신자의 사용자 에이전트에서 전달이 시작되고, 송신자의 메일 서버를 거친 후에 수신자의 메일 서버로 전달된다. 이때 수신자의 메일 박스에 저장된다
- 전자메일 박스에 있는 메시지를 보려면 메일 서버는 사용자 계정과 비밀번호를 이용하여 이용자를 인증해야한다
- 만약 메일을 수신자의 메일 서버로 전달할 수 없다면, 그 메시지를 `메시지 큐(message queue)`에 보관하고 나중에 그 메시지를 전달하기 위해 다시 시도한다 

### SMTP
> SMTP는 인터넷 전자메일을 위한 주요 애플리케이션 계층 프로토콜이다

- SMTP는 메일을 송신자의 메일 서버로부터 수신자의 메일 서버로 전송하는 데 TCP의 신뢰적인 데이터 전송 서비스를 이용한다
- SMTP의 `클라이언트와 서버 모두`가 모든 메일 서버에서 수행되고, 상대 메일로 송신할 때는 클라이언트가 되고 수신할 때는 서버가 된달

## 2.3.1 SMTP
SMTP에서 모든 메일 메시지의 몸체는 단순한 7비트 ASCII여야 한다

SMTP는 두 메일 서버가 먼 거리에 떨어져 있더라도 중간 메일 서버를 사용하지 않는다.
즉, 메시지를 보낼때 보내는데 실패하더라도 중간 메일 서버에 저장되는 것이 아니라 **송신자의 메일 서버에 남아있다**

### SMTP 기본 동작 과정
1. 클라이언트 STMP는 서버 SMTP의 25번 포트로 TCP 연결을 설정한다. 만약 서버가 죽어있다면, 클라이언트는 나중에 시도한다
2. 서버와 클라이언트는 애플리케이션 계층 핸드셰이킹을 수행한다. 이때, SMTP 클라이언트는 송신자의 전자메일 주소와 수신자의 전자메일 주소를 제공한다
3. 클라이언트는 메시지를 보낸다. TCP의 신뢰적인 데이터 전송 서비스에 의존한다
4. 보낼 다른 메시지가 있으면, 클라이언트는 이 과정을 같은 TCP 연결상에서 반복하고, 그렇지 않으면 TCP 연결를 닫는다

### SMTP 클라이언트(C)와 서버(S) 사이의 메시지 전달 과정 예시
- 클라이언트는 `HELO`, `MAIL FROM`, `RCPT TO`, `DATA`, `QUIT` 5개의 명령을 내린다
- 클라이언트는 하나의 점(.)으로 된 라인을 송신한다. 이는 서버에게 메시지의 끝을 나타낸다
- 서버는 각 명령에 응답하며, 각 응답에는 응답 코드와 영문 설명이 있다

**SMTP는 `지속 연결`을 사용한다** : 송신 메일 서버가 같은 수신 메일 서버로 보내는 여러 메시지를 갖고 있다면, `같은 TCP 연결`을 통해 모든 메시지를 전달할 수 있다

telnet 명령어는 로컷 호스트와 메일 서버 사이에 TCP연결이 설정된다

---
## 2.3.2 메일 메시지 포맷
전자 메일을 보낼 때 주변 정보가 포함된 `헤더`가 `메시지 몸체` 앞에 오게 된다

- 헤더 라인과 메시지 몸체는 빈 줄(CRLF)로 분리된다
- 모든 헤더는 From: 헤더 라인과 To: 헤더 라인을 반드시 가져야 한다
- 헤더는 Subject: 헤더와 다른 옵션 헤더 라인을 가질 수도 있다

---
## 2.3.3 메일 접속 프로토콜
> 메일 서버가 메일 박스를 관리하고 SMTP의 클라이언트와 서버 측 모두를 수행한다

일반 사용자는 로컬 호스트에서 사용자 에이전트를 수행하고 늘 켜져 있는 공유 메일 서버에 저장된 메일박스에 접근한다.
메일 서버는 보통 사용자들과 공유한다.

### 송신자 사용자 에이전트에서 수신자 메일 서버까지
송신자의 사용자 에이전트는 수신자의 메일 서버로 직접 대화하지 않는다

1. 송신자의 사용자 에이전트는 송신자의 메일 서버로 전자메일 메시지를 `SMTP` 또는 `HTTP`를 이용하여 보낸다
2. 수신자의 메일 서버는 `SMTP`를 이용하여 수신자의 메일 서버로부터 전자메일 메시지를 중계한다

두 단계 절차인 이유는 송신자 메일 서버를 통해 중계하지 않으면 송신자의 사용자 에이전트는 목적지 메일 서버에 도달할 수 없기 때문이다

### 수신자 메일 서버에서 수신자 사용자 에이전트까지
SMTP는 `push`프로토콜인 반면에 메시지를 얻는 것은 `pull`동작이기 때문에 다른 프로토콜을 사용하여야 한다

두 가지의 대표적인 방법
1. HTTP: 웹 기반 전자메일이나 스마트폰 앱, 수신자의 메일 서버는 송신자 메일 서버와 통신하기 위해 SMTP 인터페이스와 HTTP 인터페이스를 갖고 있어야 한다
2. `인터넷 메일 접근 프로토콜(IMAP)`: 메일 클라이언트(ex. 아웃룩)
