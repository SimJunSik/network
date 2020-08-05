# ch.03

# L2 스위치

## 데이터 링크 계층의 역할과 기능

---

- 데이터 링크 계층이란 ?

  - 역할
    - OSI 7 Layer의 2계층으로 인접한 네트워크 노드끼리 데이터를 전송하는 기능과 절차를 제공
    - 물리계층에서 발생할 수 있는 오류를 감지하고 수정
    - 대표적인 프로토콜로 이더넷이 있으며 장비로는 스위치가 있다
    - 단위는 Frame
  - 2개의 부 계층으로 구성
    - MAC(Media Access Control)
      - 물리적인 부분으로 매채간의 연결방식을 제어하고 1계층과 연결
    - LLC(Logical Link Control)
      - 논리적인 부분으로 Frame을 만들고 3계층과 연결
  - MAC 주소
    - 명령어 cmd > ipconfig /all 또는 네트워크 설정에서 확인
    - 48bit(6 Bytes)로 6자리로 구성, 각 16진수로 표현
    - 앞에 3자리는 OUI(Oranization Unique Identifier)
      제조사 식별코드 - http://standardsoui.ieee.org/oui.txt
    - 나머지 3자리는 제조사 내 일련번호

- 주요 기능

  - Framing
    - 데이터그램을 캡슐화하여 프레임 단위로 만들고 헤더와 트레일러를 추가
    - 헤더는 목적지, 출발지 주소 그리고 데이터 내용을 정의
    - 트레일러는 비트 에러를 감지
    - 1계층(Physical Layer)와 3계층(Network)사이에서 Packet 과 bit 를 Framing
  - 회선 제어
    - 신호간의 충돌이 발생하지 않도록 제어
    - ENQ/ACK 방법
      - 전용 전송 링크 1:1
    - Polling 방법
      - 1:다
      - Select 모드: 송신자가 나머지 수신자들을 선택하여 전송
        - ENQ/ACK 와는 다르게 동일한 Packet을 다양한 PC로 동시에 전송 가능
      - Poll 모드: 수신자에게 데이터 수신 여부를 확인하여 응답을 확인하고 전송
        - multiport
  - 흐름 제어
    - 송신자와 수신자의 데이터를 처리하는 속도 차이를 해결하기 위한 제어
    - Feedback 방식의 Flow Control이며 상위 계층은 Rate 기반
    - Stop & Wait 기법
      - 데이터를 보내고 ACK 응답이 올 때까지 기다린다
      - 간단한 구현, 비효율적
      - Frame을 전달하고 ACK이 회선 문제로 응답하지 않는 문제가 발생될 수 있음
        - ACK이 오지 않으면 일정시간 후 다시 보낸다
        - Retransmit frame
      - Frame을 재전송하게 되면 Duplicate frame 문제가 발생될 수 있음
        - Sequence number(1 bit)를 사용하여 동일 frame인지 구분하여 상위 계층으로 전달
    - Sliding window 기법
      - ACK 응답 없이 여러 개의 프레임이 연속으로 전송 가능
      - Window size는 전송과 수신측의 데이터가 저장되는 버퍼의 크기
      - Sender
        - Frame 전송 후 Window size 축소
        - ACK 수신 후 Window size 확장
      - Receiver
        - 현재 ACK - 이전에 보낸 ACK = Window size
  - 오류 제어
    - 전송 중에 오류나 손실 발생 시 수신측은 에러를 탐지 및 재전송
    - ARQ(Automatic Repeat Request)
      - 프레임 손상 시 재전송이 수행되는 과정
    - Stop & Wait ARQ
      - 전송 측에서 NAK을 수신하면 재전송
      - 주어진 시간에 ACK이 안오면 재전송
    - Go Back n ARQ
      - Sliding window 방식
      1. 전송측 Frame 6개 전송
      2. 수신측 3번 Frame 문제 발생
      3. 수신측 NAK 3으로 손상 응답
      4. 전송측 3번이 포함된 345 재전송
      - 3번 Frame에만 문제가 발생했는데 345를 다 다시보내는 비효율이 발생
    - Selective Repeat ARQ
      - Go Back n ARQ 에서 조금 더 개선된 방법
      - 손상된 Frame만 선별해서 재전송

- 이더넷 프레임 구조

  - Ethernet v2
    - 데이터 링크 계층에서 MAC(Media Access Control) 통신과 프로토콜의 형식을 정의
    ```
    Preamble | Dest Addr | Source Addr | Type   | Data | FCS
    8 byte   | 6 byte    | 6 byte      | 2 byte |      | 4 byte
    ```
    - Preamble
      - 이더넷 프레임의 시작과 동기화
    - Dest Addr
      - 목적지 MAC 주소
    - Src Addr
      - 출발지 MAC 주소
    - Type
      - 캡슐화되어 있는 패킷의 프로토콜 정의
    - Data
      - 상위 계층의 데이터로 46 ~ 1500바이트의 크기, 46바이트보다 작으면 뒤에 패딩이 붙는다
    - FCS(Frame Check Sequence)
      - 에러 체크

- Wrap up

  - 데이터 링크 계층은 인접한 네트워크 노드끼리 데이터를 전송하는 기능과 절차를 제공
  - 2개의 부계층 MAC, LLC로 구성
  - 주요 기능으로 Framing, 회선 제어, 흐름 제어, 오류 제어 등이 있다
  - 이더넷 프레임 구조는 아래와 같다

  ```
  Preamble | Dest Addr | Source Addr | Type   | Data | FCS
  8 byte   | 6 byte    | 6 byte      | 2 byte |      | 4 byte
  ```

## 스위치와 ARP

---

- L2 스위치

  - 정의

    - 2계층의 대표적인 장비로, MAC주소 기반 통신
    - 허브의 단점을 보완
      - Half duplex -> Full duplex
        ex> 일방 통신만 가능한 무전기 -> 쌍방 통신이 가능한 전화기
      - 1 Collision Domain -> 포트별 Collision Domain
    - 라우팅 기능이 있는 스위치를 L3 스위치라고도 부른다

  - 동작 방식

    - 목적지 주소를 MAC 주소 테이블에서 확인하여 연결된 포트로 프레임 전달

    1. Learning
       - 출발지 주소가 MAC 주소 테이블에 없으면 해당 주소를 저장
    2. Flooding -> Broadcasting
       - 목적지 주소가 MAC 주소 테이블에 없으면 전체 포트로 전달
    3. Forwarding
       - 목적지 주소가 MAC 주소 테이블에 있으면 해당 포트로 전달
    4. Filtering - Collision Domain
       - 출발지와 목적지가 같은 네트워크 영역이면 다른 네트워크로 전달하지 않음
    5. Aging
       - MAC 주소 테이블의 각 주소는 일정 시간 이후에 삭제

  - Learning

    - 4개의 PC는 스위치에 각 포트에 연결되고 프레임이 스위치에 전달
    - 스위치는 해당 포트로 유입된 프레임을 보고 MAC 주소를 테이블에 저장

  - Flooding

    - PC1은 목적지 aa:bb:cc:dd:ee:05 주소로 프레임 전달
    - 스위치는 해당 주소가 MAC Table에 없어서 전체 포트로 전달

  - Forwarding

    - PC1은 목적지 aa:bb:cc:dd:ee:05 주소로 프레임 전달
    - 스위치는 해당 주소가 MAC Table에 존재하므로 해당 프레임을 PC5로 전달

  - Filtering

    - PC1은 목적지 aa:bb:cc:dd:ee:02 주소로 프레임 전달
    - 스위치는 해당 주소가 동일 네트워크 영역임을 확인하여 다른 포트로 전달하지 않음
    - 필터링은 각 포트별 Collision Domain을 나누어 효율적 통신이 가능하다

  - Aging

    - 스위치는 MAC 주소 테이블은 시간이 지나면 삭제
    - 삭제되는 이유는 테이블 저장 공간을 효율적으로 사용하기 위함
    - 해당 포트에 연결된 PC가 다른 포트로 옮겨진 경우도 발생
    - 기본 300초(Cisco 기준) 저장, 다시 프레임이 발생되면 다시 카운트

  - 정리
    1. 프레임 유입
    2. 신규: 출발지 주소 저장 -> `Learning`
       기존: `Aging` 타이머 재시작 -> Refresh
    3. 목적지가 Unknown -> `Flooding`
       목적지가 MAC Table에 존재 -> `Forwarding`
       목적지가 출발지와 같은 포트에 존재 -> `Filtering`

- ARP

  - 역할
    - ARP(Address Resolution Protocol)
    - IP주소를 통해서 MAC 주소를 알려주는 프로토콜
    - 컴퓨터 A가 컴퓨터 B에게 IP통신을 시도하고 통신을 수행하기 위해 목적지 MAC 주소를 알아야 한다
    - 목적지 IP에 해당되는 MAC 주소를 알려주는 역할을 ARP가 해준다
  - 동작 과정
    1. PC1은 동일 네트워크 대역인 목적지 IP 172.20.10.9로 패킹 전송을 시도
       목적지 MAC 주소를 알기 위해서 우선 자신의 ARP Cache Table을 확인(cmd -> apr -a)
    2. ARP Cache Table에 있으면 패킷 전송, 없으면 ARP Request 전송 - Broadcasting
    3. IP 172.20.10.9에서 목적지 MAC 주소를 ARP Reply로 전달
    4. 목적지 MAC 주소는 ARP Cache Table에 저장되고 패킷 전송
  - ARP 헤더 구조

  ```
  |       Hardware Type           |       Protocol Type       |
  |Hardware Length|Protocol Length|       Operation           |
  |               Sender Hardware Address                     |
  |               Sender Protocol Address                     |
  |               Target Hardware Address                     |
  |               Target Protocol Address                     |
  ```

  - Hardware Type: ARP가 동작하는 네트워크 환경, 이더넷
  - Protocol Type: 프로토콜 종류, 대부분 IPv4
  - Hardware & Protocol Length: MAC 주소 6Byte, IP주소 4Byte
  - Operation: 명령코드, 1 = ARP Request, 2 = ARP Reply
  - Hardware Address = MAC, Protocol Address = IP

- Wrap up

  - L2 스위치는 2계층의 대표적인 장비로 MAC 주소 기반 통신
  - 스위치의 동작방식은 아래단계가 있다
    Learing - Flooding - Forwarding - Filtering - Aging
  - ARP는 IP 주소를 통해서 MAC 주소를 알려주는 프로토콜이다
  - ARP 헤더 구조

  ```
  |       Hardware Type           |       Protocol Type       |
  |Hardware Length|Protocol Length|       Operation           |
  |               Sender Hardware Address                     |
  |               Sender Protocol Address                     |
  |               Target Hardware Address                     |
  |               Target Protocol Address                     |
  ```

## 스패닝트리 프로토콜과 루핑

---

- Looping

  - 정의
    - 같은 네트워크 대역 대에서 스위치에 연결된 경로가 2개 이상인 경우게 발생
    - PC가 브로드캐스팅 패킷을 스위치들에게 전달하고 전달 받은 스위치들은 Flooding을 한다
    - 스위치들끼리 Flooding된 프레임이 서로 계속 전달되어 네트워크에 문제를 일으킨다
    - 회선 및 스위치 이중화 또는 증축 등에 의해 발생
    - 물리적인 포트 연결의 실수 또는 잘못된 이중화 구성으로 L2에서 가장 빈번히 발생하는 이슈
  - 구조
    1. PC1은 Switch 1에게 브로드캐스팅
    2. Switch 1은 모든 포트에 브로드캐스팅 전송
    3. 전달받은 브로드캐스팅 프레임을 Switch 2, 3도 모든 포트에 전송
    4. Switch 1은 Switch 2, 3에게 다시 전달 받은 브로드캐스팅을 다시 모든 포트에 전송

- STP

  - STP(Spanning Tree Protocol): 스패닝 트리 프로토콜
    - 자동으로 루핑을 막아주는 알고리즘 -> 스패닝 트리 알고리즘
    - 스패팅 트리 알고리즘에 사용되는 프로토콜 -> STP
    - IEEE 802.1d
    - STP는 2가지 개념을 가지고 있다
    1. Bridge ID
       - 스위치의 우선순위로 0 ~ 65535로 설정, 낮을수록 우선순위가 높다
       - 예전에 L2에 Bridge라는 장비가 있었지만 해당 기능을 스위치가 대체
    2. Path Cost
       - 링크의 속도(대역폭), 1000/링크속도 로 계산되며 작을수록 우선순위가 높다
       - 1Gbps 속도가 나오면서 계산법이 적합하지 않아 IEEE 에서 각 대역폭 별 숫자 정의
       - 10Mbps = 100, 100Mbps = 19, 1Gbps = 4
  - STP의 요소
    - Root Bridge: 네트워크 당 1개 선출
    - Root Port: Root Bridge가 아닌 스위치들은 1개 포트 선출
    - Designated Port: 각 세그먼트별 1개 포트 지정
  - BPDU(Bridge Protocol Data Unit)
    - 스패팅 트리 프로토콜에 의해 스위치간 서로 주고받는 제어 프레임
    1. Configuration BPDU: 구성관련
       - Root BID: 루트 브리지로 선출될 스위치 정보
       - Path Cost: 루트 브리지까지의 경로 비용
       - Bridge ID, Port ID: 나머지 스위치와 포트의 우선순위
    2. TCN(Topology Change Norification) BPDU
       - 네트워크 내 구성 변경시 통보
    - 우선수위: 낮은 숫자가 더 높은 Priority를 가진다
      - 누가 더 작은 Root BID ?
      - 루트 브리지까지 더 낮은 Path Cost ?
      - 연결된 스위치 중 누가 더 낮은 BID ?
      - 연결된 포트 중 누가 더 낮은 Port ID ?
  - Root Bridge 선출
    1. 각 스위치는 고유의 BID를 가진다
       - 2바이트(우선순위) + 6바이트 MAC 주소
    2. 서로 BPDU를 교환하고 가장 낮은 숫자가 루트 브리지가 된다
    3. 우선순위 숫자는 명령어로 설정 가능하다
  - Root Port 선출
    1. 나머지 스위치들은 루트 브리지와 가장 빠르게 연결되는 루트 포트를 선출한다
    2. 루트 포트는 가장 낮은 Root Path Cost 값을 가진다
  - Designated Port 선출
    1. 각 세그멘트별 루트 브리지와 가장 빠르게 연결되는 포트를 Designated Port로 선출
    2. 우선순위는 루트 브리지 ID > Path Cost > 브리지 ID > 포트 ID
  - 상태변화
    - 스위치의 포트는 스패닝 트리 프로토콜 안에서 5가지 상태로 표현된다
    1. Disabled
       - 포트가 Shut Donw인 상태로 데이터 전송 불가, MAC 학습 불가, BPDU 송수신 불가
    2. Blocking
       - 부팅하거나 Disabled 상태를 Up 했을 때 첫번째로 거치는 단계, BPDU만 송수신
    3. Listening - 15초
       - Blocking 포트가 루트 또는 데지그네이티드 포트로 선정되는 단계, BPDU만 송수신
    4. Learning - 15초
       - 리스닝 상태에서 특정 시간이 흐른 후 러닝 상태가 됨. MAC 학습 시작, BPDU 송수신
    5. Forwarding
       - 러닝 상태에서 특정 시간이 흐른 후 포워딩 상태가 됨, 데이터 전송 시작, BPDU 송수신

- RSTP & MST

  - RSTP(Rapid Spanning Tree Protocol)
    - IEEE 802.1w
    - STP를 적용하면 포워딩 상태까지 30 ~ 50초 걸림, 이 컨버전스 타임을 1 ~ 2초 내외로 단축
    - Learning & Listening 단계가 없음
  - MST(Multiple Spanning Tree)
    - IEEE 802.1s
    - 네트워크 그룹이 많아지면 STP or RSTP BPUD 프레임이 많아지고 스위치 부하 발생
    - 여러개의 STP 그룹들을 묶어서 효율적으로 관리

- Wrap up

  - Looping은 같은 네트워크 대역 대에서 스위치에 연결된 경로가 2개 이상인 경우에 발생
  - STP(Spanning Tree Protocol)는 루핑 방지를 자동으로 하기 위한 프로토콜이다
  - 구성 요소로는 Root Bridge, Root Port, Designated Port, Path Cost가 있다
  - 상태 변화는 아래와 같다
    Disabled - Blocking - Listening - Learning - Forwarding
  - 그 외 컨버전스 타임을 개선한 RSTP, 부하를 줄이고 효율적 관리를 위한 MST가 있다
