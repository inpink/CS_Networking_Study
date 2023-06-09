# 1.7 컴퓨터 네트워킹과 인터넷의 역사

## 1.7.1 패킷 교환 개발: 1961~1972
1960년대 초의 세계 주요 통신 네트워크는 **전화망**으로 `회선 교환` 사용하였다

각 사용자가 만드는 트래픽은 집중적(bursty), 원격 컴퓨터에 명령을 내리는 활동과 응답을 기다리고 응답을 검토하는 비활동 사이의 기간이 일정하지 않았다

3개의 연구 그룹이 `패킷 교환`의 개념을 창안하였다

1967년, 로렌스 로버츠는 **ARPAnet**에 대한 대략의 계획을 발간함, 첫 번째 패킷 교환 컴퓨터 네트워크이자 오늘날 공중 인터넷의 직계 원조
1969년, 첫 번째 패킷 스위치가 UCLA에 설치되었다
1972년, ARPAnet은 약 15개의 노드로 커졌고, 종단 시스템 간에 NCP인 첫 번째 호스트 간 프로토콜 완성되었다

---
## 1.7.2 독점 네트워크와 인터네트워킹: 1972~1980
초기 ARPAnet은 `단일 폐쇄 네트워크`, 호스트와 통신하기 위해서는 다른 ARPAnet IMP에 접속해야했다

네트워크 수가 증가함에 따라 네트워크를 연결하는 포괄 구조의 원리인 `TCP 프로토콜`이 구체화되었다

TCP 초기 버전은 종단 시스템의 재전송을 통한 데이터의 **신뢰적인 전송**과 전달 기능을 결합한 것이다

패킷 음성 같은 애플리케이션을 위한 **비신뢰적이고 흐름 제어가 없는** 종단 간의 전송 서비스의 중요성에 대한 인식과 결합하여 TCP에 대한 초기 실험은 **TCP에서 IP를 분리**하여 `UDP 프로토콜`을 개발하였다

ALOHA 프로토콜은 지리상 분산된 사용자를 하나의 방송통신매체를 공유하게 하는 `최초의 다중 접속 프로토콜`이다

PC 혁명과 네트워크 폭발이 있기 훨씬 전인 25년 전, 이더넷 프로토콜인 오늘날의 PC LAN의 기초가 연구되었다

---
## 1.7.3 네트워크 확산: 1980~1990
1983년 1월 1일, ARPAnet의 새로운 표준 호스트 프로토콜인 TCP/IP가 공식 설치됨. 모든 호스트는 같은 날 동시에 TCP로 전환해야 했다

1980년대 후반, 호스트 기반의 혼잡 제어를 구축하기 위해 TCP에 중요한 확장인 `도메인 네임 시스템(DNS)`이 개발되었다

1980년대 초, 프랑스에서 데이터 네트워킹을 모든 가정으로 보급하려는 공중 패킷 교환 네트워크인 미니텔 프로젝트를 시작했다

---
## 1.7.4 인터넷 급증: 1990년대
1. 인터넷의 원조인 ARPAnet이 더 이상 존재하지 않게 되었다
2. `월드와이드웹(WWW)` 출현하였다

웹은 검색, 인터넷 상거래, 소셜 네트워크 등을 포함하는 수백 가지의 새 애플리케이션을 만들고 보급하는 플랫폼으로 등장했다

여러 연구자가 GUI 인터페이스형 웹 브라우저를 개발하고 있었으며, 크고 작은 회사가 웹 서버를 운영했고 웹을 통한 상거래 시작했다

---
## 1.7.5 새 천 년
- 가정에 `광대역 인터넷 접속`의 공격적인 구축
- `고속 무선 인터넷` 접속 의 빠른 보급
- `온라인 소셜 네트워크`는 인터넷상에 거대한 사람들의 네트워크 생성했다
- 온라인 서비스 제공자는 자신의 커다란 `사설 네트워크` 구축. 하위 계층 ISP와 직접 연결함으로써 가능한 한 많은 인터넷 우회하는 데 사용된다
- `클라우드` 회사는 애플리케이션에 확장 가능한 컴퓨팅과 저장 환경을 제공할 뿐만 아니라, 고성능 사설 네트워크 접속도 제공한다
