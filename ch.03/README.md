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
