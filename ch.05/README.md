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
