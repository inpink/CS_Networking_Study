# 5.4 인터넷 서비스 제공업자(ISP) 간의 라우팅: BGP
패킷이 여러 AS를 통과하도록 라우팅할 땐, 자율 시스템 간 라우팅 프로토콜이 필요하다.


인터넷의 모든 AS는 경계 `게이트웨이 프로토콜`이라고 불리는 동일한 AS 간 라우팅 프로토콜을 사용한다. 일반적으로 `BGP`라고 더 알려져 있다

## 5.4.1 BGP의 역할
`같은 AS 내에 있는 목적지`에 대해서는 라우터의 포워딩 테이블 엔트리들이 해당 AS의 AS 내부 라우팅 프로토콜에 의해 결정된다.


하지만, 목적지가 AS 외부에 있는 경우, `BGP`가 필요하다.

BGP에서는 패킷이 `CIDR` 형식으로 표현된, `주소의 앞쪽 프리픽스(prefix)`를 향해 전달된다.

각 프리픽스는 **서브넷이나 서브넷의 집합**을 나타낸다.


라우터의 포워딩 테이블은 `(x, I)` 같은 형식의 엔트리를 갖게 된다
- `x`: 주소 프리픽스
- `I`: 라우터 인터페이스의 인터페이스 번호

AS 간 라우팅 프로토콜로서 BGP는 각 라우터에게 다음과 같은 수단을 제공한다
- 이웃 AS를 통해 도달 가능한 서브넷 프리픽스 정보를 얻어, 각 서브넷이 자신의 존재를 인터넷 전체에 알릴 수 있도록 한다
- 서브넷 주소 프리픽스의 가장 좋은 경로를 설정한다

---
## 5.4.2 BGP 경로 정보 알리기
각 AS에서 각각의 라우터들은 `게이트웨이 라우터` 또는 `내부 라우터`이다


**게이트웨이 라우터(gateway router)**
- AS의 경계에 있는 라우터
- 다른 AS들에 있는 하나 또는 여러 개의 라우터와 직접 연결된다

**내부 라우터(internal router)**
- 자신의 AS 내에 있는 호스트 및 라우터와만 연결된다

자율 시스템은 서로 메시지를 보내지 않고 **라우터가 보낸다**

**BGP 연결**
- BGP에서 라우터의 쌍들은 포트 번호가 `179`이고, `반영구적인 TCP 연결`을 통해 라우팅 정보를 교환한다
- TCP 연결을 통해 모든 BGP 메시지가 전송된다
- `외부 BGP 연결(eBGP)`: **2개의 AS**를 연결하는 BGP 연결
- `내부 BGP 연결(iBGP)`: **같은 AS 내의 라우터**를 연결하는 BGP 연결
- 보통 각기 다른 AS에 속하는 게이트웨이 라우터들을 직접 연결하는 링크에는 `eBGP 연결`이 존재한다
- 각 AS 내부 라우터 간에는 `iBGP 연결`도 존재하지만, 물리적인 링크와 항상 일치하지는 않는다

---
# 5.4.3 최고의 경로 결정
라우터가 BGP 연결을 통해 주소 프리픽스를 알릴 때 몇몇 `BGP 속성`을 함께 포함한다.
BGP의 용어로 **프리픽스와 그것의 속성**을 `경로`라고 한다


**AS-PATH**
- 알림 메시지가 통과하는 AS들의 리스트를 담는다
- 메시지의 루프를 감지하고 방지하기 위해 활용한다

**NEXT-HOP**
- AS-PATH가 시작되는 라우터 인터페이스의 IP 주소

### 뜨거운 감자 라우팅
> 가능한 모든 경로 중, 경로 각각의 시작점인 **NEXT-HOP 라우터까지의 경로 비용이 최소가 되는 경로**를 선택한다

1. 여러 게이트웨이를 통해 서브넷 x에 도달할 수 있다는 사실을 AS 간 프로토콜로부터 알게 된다
2. 각 게이트웨이까지의 최소 비용 경로를 정하기 위해 AS 내부 프로토콜을 통해 얻은 라우팅 정보를 이용한다
3. 뜨거운 감자 라우팅: 가장 적은 비용의 게이트웨이를 선택한다
4. 포워딩 테이블로부터 최소 비용 게이트웨이로의 인터페이스 I를 결정한 후, 포워딩 테이블에 `(x, I)`를 추가한다

뜨러운 감자 라우팅은 오로지 자신의 경로 중에서 **자기 AS 내부 비용만 줄이려는** 이기적인 알고리즘이다. 이를 사용하면 한 AS 내 2개의 라우터가 동일한 목적지 주소에 대해 각기 다른 AS 경로를 선택할 수 있다

### 경로 선택 알고리즘
> 하나의 목적지에 대해 2개 이상의 경로가 존재한다면, BGP는 **하나의 경로가 남을 때까지** 다음의 `제거 규칙`을 계속 수행한다

뜨거운 감자 라우팅에서 `지역 선호도`가 추가되어 경로에 할당되었다. 한 경로의 지역 선호도는 라우터에 의해 설정되었거나, 같은 AS 내부의 다른 라우터로부터 학습된 것이다

1. `최고 지역 선호 값`을 가진 경로가 선택된다
2. 최고 지역 선호 값을 가진 경로가 여러 개 있다면, 이들 중 `최단 AS-PATH`를 가진 경로가 선택된다
3. 같은 최고 지역 선호 값과 같은 AS-PATH 길이를 가진 모든 남은 경로들에 대해 `뜨거운 감자 라우팅`을 수행한다
4. 만일 아직도 하나 이상의 경로가 남아 있다면, 라우터는 `BGP 식별자`를 사용하여 경로를 선택한다

---
## 5.4.4 IP 애니캐스트
> 애니캐스트란 송신 노드가 네트워크에 연결된 수신 가능한 노드 중에서 가장 가까운 한 노드에만 데이터를 전송한다 (IPv6 기반으로 작동)

BGP는 `IP 애니캐스트 서비스`를 구현하는 데도 활용된다

많은 애플리케이션에서 (1) 같은 콘텐츠를 **지리적으로 분산된 다른 많은 서버에 복제하고,** (2) 각 사용자를 가장 가까운 서버의 콘텐츠로 접근하게 하려고 하는 경우, BGP의 경로 선택 알고리즘을 사용할 수 있다

1. IP 애니캐스트 설정 단계에서 (1) CDN 사업자가 자신의 서버 여러 대에 **동일한** IP 주소를 할당하고 (2) 표준 BGP를 활용하여 이 주소를 서버 각각으로부터 알린다

2. 각 라우터는 라우터 테이블을 설정하면서 `BGP 경로 선택 알고리즘`을 수행하여, 해당 IP 주소로의 최고의 경로를 골라낸다
3. 사용자가 비디오를 요청하면 CDN은 사용자가 어디에 위치해 있든, 지리적으로 분산되어 있는 서버들이 **공통적으로 사용하는 IP 주소**를 사용자에게 돌려준다
4. 사용자가 그 주소로 요청을 보내면 인터넷 라우터는 그 요청 패킷을 BGP 경로 선택 알고리즘이 정의한 `가장 가까운` 서버로 전달한다

---
## 5.4.5 라우팅 정책
라우터가 목적지까지의 경로를 선택하려고 할 때, `AS 라우팅 정책`은 최단 AS-PATH나 뜨거운 감자 라우팅 등의 다른 모든 고려사항보다 우선시된다

---
## 5.4.6 조각 맞추기: 인터넷 존재 확인하기
작은 회사를 설립했다고 가정한다
1. 먼저 `지역 ISP`와 계약하여 인터넷 연결을 해야한다
- 회사는 `게이트웨이 라우터`를 갖게 되는데, 이는 지역 ISP의 라우터에 연결될 것이다
- 지역 ISP는 특정 범위의 `IP 주소`를 제공한다

2. 회사의 `도메인 이름`을 얻기 위해 인터넷 등록 기관과 계약을 해야 하며, `DNS 시스템`에 등록해야 한다
- 회사의 `DNS 서버`의 IP 주소를 등록 기관에 제공해야 한다
- 등록 기관은 `.com 최상위 도메인 서버`에 회사의 DNS 서버를 추가한다

3. 웹 서버 IP 주소를 검색할 수 있도록, 회사의 `DNS 서버`에 `웹 서버 호스트 이름`과 `IP 주소의 사상 항목`을 포함시켜야 한다

4. 전 세계의 외부인이 접근할 수 있도록 하기 위해서는, `라우터`가 회사 주소 범위에 해당하는 `IP 주소 프리픽스` 24비트의 존재를 알고 있어야 한다
- 이는 `BGP`를 통해서 이루어지게 된다
- 회사가 지역 ISP와 계약하고 주소 프리픽스를 할당 받을 때, 지역 ISP는 BGP를 사용하여 자신과 연결되어 있는 ISP들에게 회사의 주소 프리픽스를 알린다
- 연결된 ISP들 역시 BGP를 활용하여 이 알림 정보를 전파한다
