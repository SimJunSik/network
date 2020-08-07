# ch.05

# 동적 라우팅

## 동적 라우팅의 개요

---

- 라우팅 프로토콜

  - 개요
    - 라우팅 프로토콜은 정적(Static) & 동적(Dynamic)으로 구분된다
    - 정적 라우팅
      - 경로 정보를 라우터에 미리 저장하여 패킷 전송
    - 동적 라우팅
      - 경로 정보가 네트워크 상황에 따라 더 빠른 경로로 변경되어 패킷 전송
      - AS(Autonomous System)
        - 라우터의 집단
      - EGP(Exterior Gateway Protocol)
        - AS - AS
        - BGP
      - IGP(Interior Gateway Protocol)
        - AS 내에서
        - RIP, OPSF

- 라우팅 알고리즘

  - 역할
    - 목적지까지 최적 경로를 계산하고 라우팅 테이블에 업데이트
    - 동적으로 라우팅 테이블을 유지 및 관리하는 알고리즘
    - Distance Vector & Link State routing 으로 구분한다
      - Distance Vector
        - 분산 업데이트, 각 라우터들의 의해 최소 비용 경로 계산 -> 인접 노드와 교환
        - 소규모 네트워크에 적합
        - 주기적이며 비동기 방식
      - Link State
        - 중앙 집중형 업데이트, 네트워크 전체 정보를 통해서 최소 비용 경로 계산
        - 대규모 네트워크에 적합
        - 이벤트 기반의 라우팅 테이블 관리
  - Distance Vector 라우팅
    - 거리 + 방향
    - 목적지 IP까지의 거리 = Hop 카운트 = 라우터와 라우터 사이의 거리 + 인터페이스 방향
    - 인접 라우터들과 주기적으로 라우팅 테이블을 교환하여 확인 및 관리
    - 인접 라우팅 테이블만 관리 -> 메모리 절약
    - 비교적 구성이 간단
    - 주기적 라우팅 테이블 업데이트 -> 무의미한 트래픽 발생 가능
    - Convergence time(라우팅 테이블 업데이트 시간)이 느리다
    - 소규모 네트워크에 적합
    - 1969년 Bellman-Ford 알고리즘에 기반하여 설계, APANET 최초의 라우팅 알고리즘
  - Bellman-Ford 알고리즘
    - 최단 경로 문제를 풀어주는 알고리즘
    - 주기적 업데이트
      - 연결 링크의 비용 변경
      - 최단 거리의 변경
      - Listening -> Change -> Estimate -> Notify -> Update
      - 기다림 -> 최단 거리값 & 연결 링크 비용 변경 -> 인접 노드로 전달
  - Link State 라우팅
    - 링크 상태
    - 즉, 회선의 대역폭을 고려하여 가중치를 부여
    - 네트워크 토폴로지 경로를 모든 라우터들에게 전달
    - 라우팅 정보가 변경되는 이벤트 건에 대해서만 전파 -> 네트워크 트래픽 감소
    - 전체 네트워크 상의 라우터들의 테이블 정보가 동일하게 유지
    - 각 라우터들은 최상의 경로를 계산 -> Dijkstra's algorithm
      - 1959년 컴퓨터 과학자 Dijkstra(1972년 튜링상 수상)가 발표
    - 1980년 ARPANET에서 개발 -> 1989년 OSPF 발표
  - Dijkstra's 알고리즘
    - 주어진 출발지와 목적지 사이의 최단 경로 문제를 푸는 알고리즘

- Wrap up

  - 동적 라우팅은 네트워크 상황에 따라 경로 정보가 변경되어 패킷을 전송
  - Distance Vector 라우팅
    - 거리(hop count) + 방향이며 분산형으로 주기적으로 업데이트
    - Bellman-Ford 알고리즘을 사용하며 APANET 최초의 라우팅 알고리즘
  - Link State 라우팅
    - 회선의 대역폭을 고려하며 중앙 집중형으로 이벤트 기반으로 업데이트
    - Dijkstra's 알고리즘을 사용하며 1980년 ARPANET에서 개발

## 동적 라우팅 BGP & RIP

---

- 동적 라우팅 구분

  - 개요
    - 동적 라우팅 프로토콜은 AS(Autonomous System)에 따라 구분된다
    - IGP(Interior Gateway Protocol)
      - AS 내에서 동작하는 라우팅 프로토콜
      - RIP - Distance Vector
      - OSPF - Link State
    - EGP(Exterior Gateway Protocol)
      - AS 와 AS 간의 라우팅 프로토콜
      - BGP
  - AS(Autonomous System)
    - 하나의 회사 또는 단체 안에서 동일한 정책으로 관리되는 라우터들의 집단

- EGP - BGP

  - BGP(Border Gateway Protocol)
    - 현재 인터넷에서 쓰이는 가장 대표적인 EGP 라우팅 프로토콜
    - ISP to ISP 연결 간 사용
    - 경로 벡터 라우팅 프로토콜을 사용 - 루핑 방지
    - 2006년 BGP4 릴리즈 - RFC 4271
    - 유니캐스트로 라우팅 정보 전송 - TCP 179
    - 변경 또는 추가 된 부분만 업데이트
    - 빠른 속도 보다는 조직 또는 단체간 맺어진 정책에 의거하여(안정성 추구) 최적 경로 설정
  - BGP 구성
    - eBGP(external)
      - 서로 다른 AS 간의 연결 및 라우팅 정보 교환
    - iBGP(internal)
      - 동일 AS 내에서 BGP 라우팅 정보 교환
  - BGP 설정
    - Router ID, Neighbor, Network 설정
    - Router ID
      - 라우터 별 식별용 IP 설정
    - Neighbor
      - 자동 탐지 불가, 수동으로 인접 라우터의 AS 번호를 설정
      - Connected 인터페이스로 Next hop 설정
    - Network
      - 전파할 네트워크 대역
  - BGP 메시지 4가지
    - 인접 라우터 관계 확인 및 라우팅 정보 교환
    - OPEN
      - 인접 라우터와 연결된 후 보내는 메시지
      - BGP 버전, AS 번호, Hold Time, Operation parameter
    - UPDATE
      - 경로에 대한 속성 값
      - Unreachable Route, Path Attribute, Network Layer Rechablility
    - NOTIFICATION
      - 에러가 감지 되면 에러 코드를 보내고 BGP 연결 종료
    - KEEPALIVE
      - 주기적으로 인접 라우터와의 연결을 확인
  - BGP FSM(Finite State Machine)
    - 피어 라우터와의 동작을 결정하기 위해 6가지 유한 상태 머신 사용
    1. Idle
       - 모든 자원을 초기화하고 피어 연결 준비 상태
    2. Connect
       - 연결이 완료되기를 기다리는 상태
    3. Active
       - 연결 실패 이후 다시 연결을 시도하는 상태
    4. Open Sent
       - OPEN 메시지를 보내는 상태
    5. Open Confirm
       - OPEN 메시지를 받은 상태
    6. Established
       - KEEPALIVE 메시지를 받은 상태

- IGP - RIP

  - RIP(Routing Information Protocol)
    - Distance Vector 기반의 IGP용 라우팅 프로토콜
    - 속도가 아닌 거리(라우터의 홉)기반 경로 선택
    - 주기적으로 전체 라우팅 테이블 업데이트 - 30초
    - 최대 홉 카운트는 15
    - 구성이 간단, 적은 메모리 사용, 소규모 네트워크에서 주로 사용
    - RIPv1
      - Classful 라우팅, 라우팅 업데이트시 서브넷마스크 정보를 전달하지 않음
      - 브로드캐스팅
    - RIPv2
      - Classless 라우팅, 라우팅 업데이트시 서브넷마스크 정보 전달
      - 멀티캐스팅, Triggerd Update 설정 가능
  - RIP 메시지 포맷
    - Command
      - 명령 1 Request, 2 Response
    - Version
      - 1 or 2
    - Family
      - 프로토콜 정보, IP = 2
    - IP Address
      - 목적지 주소, Subnetmask, Next Hop
    - Distance
      - 홉 카운트
  - RIP 동작
    1. 요청 메시지
       - 라우터가 초기화 또는 라우팅 테이블의 특정 엔트리 타이머 종료시
       - 특정 네트워크 주소 또는 전체 라우팅 정보를 요청
    2. 응답 메시지
       - 요청 메시지 수신 후 응답 또는 주기적(30초)으로 자신의 라우팅 정보를 전파
       - 일정시간(180초) 동안 특정 경로에 대한 응답이 없으면 홉 카운트 16으로 설정
    - RIP 메시지 수신
      - 신규 목적지 -> 라우팅 테이블에 추가
      - Next Hop 정보가 수정된 경우 -> Next Hop 정보 변경
      - Hop Count 비교 -> 숫자가 작으며 변경, 크면 무시

- Wrap up

  - 동적 라우팅은 AS(Autonomous System)에 따라 IGP(Interior Gateway Protocol) & EGP(Exterior Gateway Protocol) 구분
  - AS는 하나의 회사 또는 단체 안에서 동일한 정책으로 관리되는 라우터들의 집단
  - BGP(Border Gateway Protocol)는 현재 인터넷에서 가장 널리 쓰이는 EGP 라우팅 프로토콜
    - BGP4
  - RIP(Routing Information Protocol)은 Distance Vector 기반의 IGP용 라우팅 프로토콜
    - RIPv1, RIPv2

## 동적 라우팅 OSPF

---

- IGP - OSPF

  - OSPF(Open Shortest Path First)
  - 정의
    - 링크 스테이트 라우팅 알고리즘을 사용하는 IGP용 라우팅 프로토콜
    - 1998년 RFC 2338 OSPFv2
    - 2008년 RFC 5340 OSPFv3 for IPv6
    - RIPv1의 단점을 보완
      - 홉 카운트의 제한이 없음
      - VLSM(Variable-Length Subnet Mask) 사용하여 효율적 IP 관리
      - 변경된 정보만 전파, 적은 양의 라우팅 트래픽 유발
      - 단순 라우터의 홉이 아닌 링크의 상태로 경로 설정
      - Convergence 타임이 빠름
  - 구성
    - 계층적 구조, 여러개의 Area로 나뉘고 각 영역은 독립적으로 라우팅 수행
    - ASBR(AS Boundary Router)
      - 다른 AS에 있는 라우터와 라우팅 정보 교환
    - Backbone Roueter
      - AS 내의 여러 Area를 모두 연결
      - OSPF 도메인 내에서 모든 링크 상태 정보를 취합하고 분배
    - ABR(Area Border Router)
      - 각 Area와 백본 Area 0을 연결
  - OSPF 메시지
    - 프로토콜 ID 89, 인접 라우터의 발견 및 관계 유지, 멀티캐스트 사용
    1. Hello
       - 인접 라우터 및 로컬 링크 상태 검색
       - 관계를 설정하고 주요 매개변수 전달
       - 일정 간격으로 인접 라우터들의 상태(Keepalive)를 확인
    - LSDB(Link State Database)
      - 각 OSPF Area 내 전체 망 정보, 링크 상태 및 경로 정보
      - LSA(Link State Advertisement) 패킷들에 의해 구축
      - LSU & DD 메시지를 통해 전달
    - LSDB 정보 업데이트 및 관리 메시지
    2. DBD(Database Description)
       - OSPF 정보 구축을 위해 LSDB 내용을 전달
    3. LSR(Link State Request)
       - 상대 라우터에게 링크 상태 정보를 요청
    4. LSU(Link State Update)
       - 네트워크 변화 발생시 인접 라우터에게 상태 전달
    5. LSAck(Link State Acknowledgment)
       - 수신 확인
       - 신뢰성 확보
  - 테이블 종류
    1. OSPF 네이버 테이블
       - 네이버를 성립한 인접 라우터 정보 관리
       - 네이버 라우터 ID 확인
    2. OSPF DB 테이블
       - 네이버에게 수신한 라우팅 업데이트 정보를 관리
       - LSA 메시지를 이용하여 LSDB 동기화
       - LSDB 정보를 기반으로 최적 경로를 선출
    3. 라우팅 테이블
       - 최적 경로 등록
       - Inter Area 라우팅 정보
       - 다른 Area 업데이트 정보
       - 외부 AS 업데이트 정보
  - 네이버 테이블의 라우터 상태 변화
    1. Down
       - Power off
    2. init
       - Hello 메시지를 받으면 init 상태, 인식
    3. 2 way
       - Hello 메시지로 Neighbor 확인
       - 확인된 내용들 Neighbor List에 업데이트
    4. Exstart (실행)
       - DBD 메시지를 통해 마스터/슬레이브 선출
    5. Exchange (교환)
       - DBD 메시지를 통해 링크 상태 정보 교환
    6. Loading (전송)
       - LSR을 통해 완전한 정보 요청
       - LSU를 통해 상대방에게 업데이트를 보낸다
    7. Full
       - 인접한 네이버 라우터들의 정보를 유지
  - 링크 종류
    1. Point to Point
       - 라우터와 라우터가 1:1로 직접 연결
    2. Transient
       - 여러개의 라우터가 동일한 Area에서 버스를 통해서 연결
    3. Stub
       - 하나의 Area에 1개 라우터만 연결
    4. Virtual
       - 물리적으로 백본영역과 연결이 어려운 상태에서 가상으로 연결
  - DR & BDR
    - DR(Desiganated Router): 대장 라우터
    - BDR(Back-up Desiganated Router): 부대장 라우터
    - DR과 BDR은 중복되는 LSA 교환을 방지하고자 선출
    - LSA(Link State Advertisement)
      - 라우팅 기초 정보가 담겨진 패킷으로 링크 상태, 인접 관계 형성, 요약 정보
      - 네트워크/링크의 경로 비용 포함
    - 그 외 라우터들은 LSA 정보를 교환하지 않고 Hello만 교환하고 네이버 관계 형성
    - DR/BDR은 다른 라우터들과 LSA 정보를 교환하여 인접 네이버 관계를 형성
    - OSPF Priority가 가장 높은 라우터를 DR로 선출
    - Priority가 동일하면 라우터 ID로 선출
  - 동작 과정
    1. R1이 새로 OSPF 라우팅으로 구성
    2. Hello 메시지로 인텁 라우터 확인
    3. DR & BDR 주소 확인
    4. LSA 정보를 DR & BDR 에게 전달
    5. BDR은 타이머 세팅, DR 수행 감시
    6. DR은 LSA 정보를 다른 모든 라우터들에게 전달
       - 모든 라우터들에게 ack을 수신
    7. DR이 BDR 타이머 동안 제대로 수행 못하면, BDR이 DR로 선출되고 추가로 BDR 선출
    8. 링크 다운시 R2는 DR에게 알리고 DR은 다시 모든 라우터들에게 전달

- Wrap up

  - OSPF는 링크 스테이드 라우팅 알고리즘을 사용하는 IGP용 라우팅 프로토콜
  - RIPv1의 단점을 보완했고 계층적 구조로 여러개의 Area로 나뉜다
  - OSPF 메시지는 Hello, DBD, LSR, LSU, LSAck의 5가지로 구성된다
  - OSPF 테이블은 네이버, DB, 라우팅 테이블로 구성된다
  - 네이버 테이블은 Down -> Init -> 2 way -> Exstart -> Exchange -> Loading -> full 상태로 표현
  - DR과 BDR을 선출하고 LSR은 DR에게만 보내 중복 패킷을 방지한다
  - LSA는 링크 상태, 인접 관계 형성, 요약 정보 등 네트워크/링크의 경로 비용을 포함한 패킷
