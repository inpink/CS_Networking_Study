# 4.3 인터넷 프로토콜(IP): IPv4, 주소체계, IPv6 등

## 4.3.1 IPv4 데이터그램 포맷
인터넷 네트워크 계층 패킷은 `데이터그램`이라고 부른다

IPv4 데이터그램의 주요 필드는 다음과 같다
- 버전 번호: `4비트`로 IP 프로토콜 버전을 명시한다
- 헤더 길이: `4비트`로 IP 데이터그램에서 실제 **페이로드**가 시작하는 곳을 결정한다
- 서비스 타입: 각기 다른 유형의 IP 데이터그램을 구별한다
- 데이터그램 길이: `16비트`로 바이트로 계산한 IP 데이터그램의 전체 길이다
- 식별자, 플래그, 단편화 오프셋: IP 단편화와 관련이 있다
- TTL(Time-To-Live): 네트워크에서 데이터그램이 무한히 순환하지 않도록 한다(라우팅 루프)
- 프로토콜: IP 데이터그램에서 데이터 부분이 전달될 목적지의 트랜스포트 계층의 특정 프로토콜을 명시한다
- 헤더 체크섬: 라우터가 수신한 IP 데이터그램의 비트 오류를 탐지한다
- 출발지와 목적지 IP 주소: 자신의 IP 주소를 출발지에 목적지 IP 주소를 목적지에 삽입한다
- 옵션: IP 헤더를 확장한다
- 데이터(페이로드): 트랜스포트 계층 세그먼트나 ICMP 메시지를 담는다

IP 데이터그램은 총 `20바이트`의 헤더를 갖는다

---
## 4.3.2 IPv4 주소체계
호스트 IP가 데이터그램을 보낼 때 링크를 통해 데이터링크를 보낸다.
호스트와 물리적 링크 사이의 경계를 **인터페이스**라고 부른다

각 링크마다 하나의 인터페이스를 갖고, 하나의 라우터는 여러 개의 인터페이스를 갖는다
따라서 **IP 주소는 인터페이스**와 관련이 있다

### IP 주소
IP 주소는 `32비트(4바이트)`이다. 
이 주소는 주소의 각 바이트를 십진수로 표현하고 주소의 다른 바이트와 **점(.)으로 구분하는 십진 표기법**을 사용한다

전 세계의 인터넷에서 모든 호스트와 라우터의 각 인터페이스는 고유한 IP 주소를 갖는다.

### 서브넷
IP 용어로 호스트들이 인터페이스와 하나의 라우터 인터페이스로 연결된 네트워크는 `서브넷(subnet)`을 구성한다고 말한다

IP 주소 체계는 서브넷에 `223.1.1.0/24`라는 주소를 할당하는데, 여기서 `/24`는 **서브넷 마스크**라고 부르고, 32비트 주소의 **왼쪽 24비트**가 서브넷 주소를 가리킨다

서브넷은 호스트 인터페이스들(`223.1.1.1`, `223.1.1.2`, `223.1.1.3`)과 하나의 라우터 인터페이스(`223.1.1.4`)를 구성한다

네트워크 `223.1.1.0/24`에 새로 부착할 호스트에는 `223.1.1.xxx` 형식의 주소가 필요하다

서브넷을 결정하려면 먼저 호스트나 라우터에서 각 인터페이스를 분리하고 고립된 네트워크를 만드는데, 이 고립된 네트워크 각각을 **서브넷**이라고 부른다

### CIDR
`CIDR`는 서브넷 주소체계로서, 32비트 IP 주소는 두 부분으로 나누고, 이것은 다시 점으로 된 십진수 형태의 `a.b.c.d/x`를 가진다

`a.b.c.d/x`형식 주소에서 최상위 비트를 의미하는 `x`는 IP 주소의 **네트워크** 부분을 구성한다. 이를 해당 주소의 **프리픽스(prefix)** 또는 **네트워크 프리픽스**라고 부른다

한 기관은 통상 **공통 프리픽스를 갖는 주소 범위 주소**를 블록에 할당 받으므로, 외부 기관의 라우터는 목적지 주소가 내부 기관인 데이터그램을 전달할 때, 단지 **앞의 x비트만을** 고려한다

주소의 나머지 32-x비트들은 기관 **내부에** 같은 네트워크 프리픽스를 갖는 모든 장비를 구별함으로써 기관 **내부의** 라우터에서 패킷을 전달할 때 사용된다

### 주소 블록 획득
네트워크 관리자는 먼저 이미 할당받은 주소의 큰 블록에서 주소를 제공하는 **ISP**와 접촉해야 한다

또한 ISP와 다른 조직에 주소 블록을 할당하는 최상위 국제기관인 ICANN에서 IP주소를 관리한다.
비영리 단체인 ICANN의 역할은 IP 주소 할당과 DNS 루트 서버 관리다. 도메인 이름을 할당하고 도메인 이름 분쟁을 해결한다

### 호스트 주소 획득: 동적 호스트 구성 프로토콜 (DHCP)
한 기관은 ISP로부터 주소 블록을 획득하여, 개별 IP 주소를 기관 내부의 라우터 인터페이스와 호스트에 할당한다
- 라우터 인터페이스 주소에 대해 관리자는 라우터 안에 IP 주소를 할당한다
- 호스트 주소에 대해 관리자는 **동적 호스트 구성 프로토콜(DHCP)**을 사용하여 IP 주소를 할당한다.

호스트가 네트워크에 접속하고자 할 때마다 `동일한 IP 주소`를 받도록 하거나, 다른 `임시 IP 주소`를 할당하도록 **DHCP**를 설정한다

DHCP의 특징
- `호스트 IP 주소 할당`, `서브넷 마스크`, `첫 번째 홉 라우터(디폴트 게이트웨이) 주소`, `로컬 DNS 서버 주소`와 같은 추가 정보를 제공한다
- 네트워크에서 자동으로 호스트를 연결해주는 능력 때문에 **플러그 앤 플레이 프로토콜**이나 **제로 구성 프로토콜**이라고도 한다
- 클라이언트 - 서버 프로토콜이다. DHCP 서버가 서브넷에서 이용 가능하다

DHCP 프로토콜의 4단계 과정은 다음과 같다
1. DHCP 서버 발견: **DHCP 발견 메시지**를 사용하며, 클라이언트는 `포트 67번`으로 UDP 패킷을 보낸다. 목적지 IP 주소를 `255.255.255.255`로 설정하고 출발지 IP 주소를 `0.0.0.0`으로 설정한다
2. DHCP 서버 제공: **DHCP 제공 메시지**를 서버가 클라이언트로 응답한다. 이때도 IP 브로트캐스트 주소 `255.255.255.255`를 사용한다. 각각의 DHCP 제공 메시지는 트랜잭션 ID, 클라이언트에 제공된 IP 주소, 네트워크 마스크, IP 주소 임대 기간을 포함한다
3. DHCP 요청: 클라이언트는 여러개의 서버 제공자 중에서 선택하여 제공자에게 **DHCP 요청 메시지**로 응답한다
4. DHCP ACK: 서버는 요청된 파라미터를 확인하는 **DHCP ACK 메시지**로 응답한다

---
## 4.3.3 네트워크 주소 변환 (NAT)
**사설 주소를 갖는 권역**이란 네트워크 주소들이 네트워크의 **내부에** 있는 장비에게만 의미가 있고 홈 네트워크를 **벗어나** 글로벌 인터넷으로 가는 패킷 전달은 이 주소들을 사용할 수 없다

`NAT 라우터`는 외부에서 데이터그램이 도착하면 **NAT 변환 테이블**을 사용하여 `IP 주소와 포트 번호`를 변경한다. 이것은 외부에서 들어오는 홈 네트워크의 상세한 사항을 숨긴다

---
## 4.3.4 IPv6
32비트 IPv4 주소 공간이 빠른 속도로 고갈이 IPv6의 개발의 주된 동기이다

### IPv6 데이터그램 포맷
IPv6에 도입된 중요한 변화는 다음과 같다

- 확장된 주소 기능: IPv6는 `128비트`로 확장했으므로 IP 주소가 고갈되는 일은 발생하지 않을 것이다. 또한 새로운 주소 형태인 **애니캐스트 주소**가 도입되었다
- 간소화된 40바이트 헤더: IPv4의 많은 필드가 생략되거나 옵션으로 남겨져 라우터가 IP 데이터그램을 더 빨리 처리하게 해준다
- 흐름 레이블링: IPv6는 정의하기 어려운 **흐름**을 갖고 있다

IPv6 데이터그램 필드는 다음과 같다

- 버전: `4비트` 필드는 IP 버전 번호를 인식한다
- 트래픽 클래스: `8비트` 필드는 특정 애플리케이션 데이터그램에 우선순위를 부여하는 데 사용한다
- 흐름 레이블: `20비트` 필드는 데이터그램의 흐름을 인식하는 데 사용한다
- 페이로드 길이: `16비트`값은 고정 길이 40바이트 패킷 헤더 뒤에 나오는 바이트 길이다
- 다음 헤더: 데이터그램의 내용이 전달될 프로토콜을 구분한다
- 홉 제한: 라우터가 데이터그램을 전달할 때마다 `1`씩 감소한다. 0보다 작아지면 라우터는 데이터그램을 버려야 한다
- 출발지와 목적지 주소
- 데이터: IPv6 데이터그램의 `페이로드` 부분이다

IPv6에는 더 이상 존재하지 않는 필드는 다음과 같다
- 단편화/재결합: IPv6은 단편화와 재결합을 출발지와 목적지만이 수행한다
- 헤더 체크섬: 체크섬 기능이 반복되므로 생략했다
- 옵션: IPv6에서 다음 헤더가 중 하나가 될 수 있다

### IPv4에서 IPv6로의 변환
이미 IPv4로 구축된 시스템은 IPv6 데이터그램을 처리할 수 없다. 이에 해결책은 다음과 같다

- 플래그 데이: 업그레이드하는 시간과 날짜를 정하는 것이다
- 터널링: 두 IPv6 노드가 IPv6 데이터그램을 사용해서 작동하는 것이다
