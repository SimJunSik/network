# ch.02

# 물리 계층

## 물리계층의 역할과 기능

---

- 물리계층이란 ?

  - 역할
    - OSI 7 Layer의 1계층으로 하드웨어로 표현
    - 네트워크 장치의 전기적, 기계적 속성 및 전송하는 수단을 정의
    - 상위 계층인 데이터 링크 계층의 프레임을 신호로 인코딩하여 네트워크 장치로 전송
    - 통신 장치와 커넥터, 인코딩(Bit -> Signal), 송수신을 담당하는 회로 등의 요소가 있다

- Signaling 의 종류

  - 전기
    - Copper 케이블을 사용하며 전화선, UTP, 동축케이블 등이 이에 속함
  - 광(빛)
    - Optical Fiber 케이블이 이에 속하여 빛의 패턴을 신호로 사용

  ```
  * IEEE 802.3
    - 이더넷에서 물리계층과 데이터 링크 계층의 매체 접근 제어를 정의, 케이블이 이에 속함
    - 주로 쓰이는 1 Gigabit 이더넷 규격
  ```

  |            | UTP        | STP         | Fiber       |             |
  | ---------- | ---------- | ----------- | ----------- | ----------- |
  | 규격       | 802.3ab    | 802.3z      |
  | 구분       | 1000BASE-T | 1000BASE-CX | 1000BASE-SX | 1000BASE-LX |
  | Max Length | 100m       | 25m         | 500m        | 5km         |

  ```
    - 첫번째 숫자는 Speed를 의미
    - 두번째 BASE는 Baseband라는 전송방식을 의미
    - 세번째는 숫자일 경우 전송 거리, 영문자일 때는 케이블 종류 또는 광 타입
  ```

  - 전파
    - 무선이 이에 속하며 마이크로파 패턴을 신호로 사용

  ```
  * IEEE 802.11
    - 무선랜 규격
  ```

  |              | 802.11b | 802.11a | 802.11g | 802.11n      | 802.11ac     | 802.11ax     |
  | ------------ | ------- | ------- | ------- | ------------ | ------------ | ------------ |
  | Max rate     | 11Mbps  | 54Mbps  | 54Mbps  | 600Mbps      | 6.7Gbps      | 9.6Gbps      |
  | Frequency    | 2.4GHz  | 5GHz    | 2.4GHz  | 2.4/5GHz     | 5GHz         | 2.4/5/6GHz   |
  | Indoor range | 35m     | 35m     | 38m     | 70m          | 35m          | 30m          |
  | Release      | 1999    | 1999    | 2003    | 2009 Wi-Fi 4 | 2013 Wi-Fi 5 | 2019 Wi-Fi 6 |

- 통신 방식

  - 방식
    - OSI 7 Layer 2계층의 Frame은 아래와 같은 형태로 전달
    ```
    [Data Link Layer - Frame(PDU)]
    [인코딩] bit 0101010100 [인코딩]
    [신호]   _-_-_-_-_-_    [신호]
    [Media]-----------------[Media]
           전기, 빛 또는 전파
    ```

- Wrap up

  - 물리계층은 네트워크 장치의 전기적, 기계적 속성 및 전송하는 수단을 정의
  - 전송하는 수단인 Signaling은 전기, 광, 무선 등이 있다
  - IEEE 802.3 케이블, IEEE 802.11 무선 표준이 있다
  - 물리계층은 bit를 인코딩하여 2계층(Data Link)과 통신한다

  ```
  [Data Link Layer - Frame(PDU)]
  [인코딩] bit 0101010100 [인코딩]
  [신호]   _-_-_-_-_-_    [신호]
  [Media]-----------------[Media]
          전기, 빛 또는 전파
  ```

## 물리계층 장비와 케이블

---

- 물리계층 장비

  - 허브와 리피터
    - 허브는 전기신호를 증폭하여 포트에 연결된 PC들끼리 통신이 가능하게 한다
    - 리피터는 현재 거의 쓰이지 않는 장비(허브도 마찬가지)로 신호의 세기를 증폭하여 좀 더 먼거리 까지 통신이 가능
  - 허브의 동작방식
    - 단순 중계기의 역할로, 허브에 연결된 PC 1이 다른 PC 2에게 데이터를 보내려 하면 허브에 연결된 모든 PC들에게 그 데이터를 전달하게 한다
      - 브로드캐스팅 통신 1 -> All
      - 유니캐스팅 통신 1 -> 1
      - 멀티캐스팅 통신 1 -> n
  - CSMA/CD (Carrier Sense Multiple Access/Collision Detection): 허브의 통신방식
    - 송신노드는 데이터를 전송하고, 다음 채널에서 다른 노드의 데이터 충돌 발생을 계속 감지
    - 충돌 발생시에는 모든 노드에게 충돌 발생을 통지하고 재전송을 시도
    1. Carrier Sensing: 데이터를 보내기 전에 다른 노드에서 데이터를 보내는 중인지 확인
    2. Multiple Access: 데이터를 보내는 곳이 없다면 전송 시작
    3. Collision Detection: 동 시간대에 데이터를 보내게 되면 충돌이 일어나고 정지
       그 이후 특정 시간이 지나면 다시 첫번째 단계로 반복
    - Half Duplex: 반이중 전송방식
  - 전송방식
    1. Simplex: 단방향 통신으로 수신측은 송신측에 응답 불가
    2. Half Duplex: 반이중 전송방식으로 양방향 통신이나 송수신 시간은 정해짐, 무전기
    3. Full Duplex: 전이중 전송방식으로 동시 양방향 통신이 가능, 전화기

- 케이블과 커넥터

  - 종류
    - 전송 장치에 신호를 전달하는 통로, 주요 케이블로 TP, 동축, Fiber 등이 있다
    - TP(Twisted Pair)
      - 총 8가닥의 선으로 구성되며 두개의 선을 서로 꼬아놓는다
      - 선을 꼬은 이유는 자기장 간섭을 최소화 하여 성능(속도와 거리)을 향상
      - UTP(Unshielded Twisted Pair)
        - 실효성 때문에 STP 보다 더 많이 쓰임
      - STP(Shielded Twisted Pair)
    - 동축(Coaxial)
      - 선 중앙에 심선이 있으며 그 주위를 절연물과 외부 도체로 감싸고 있다
      - 전화 또는 회선망 등 광범위하게 사용
    - 광(Fiber)
      - 전기신호의 자기장이 없는 빛으로 통신하기 때문에 장거리 고속 통신이 가능
      - 2개의 모드(Single, Multi)와 주요 커넥터 타입(LC, SC)이 있다
    - 광 트랜시버
      - 광통신에 사용되는 네트워크 인터페이스 모듈 커넥터로 SFP, GBIC가 있다
      - SFP(Small Form-factor Pluggable transceiver)
      - GBIC(Gigabit Interface Connector)

- 단위와 성능

  - bit & Byte
    - bit
      - 2진수는 Binary 0,1로 이루어지며 True & False 등 신호를 표현
    - 1 Byte = 8 bit
    - bit는 일반적으로 회선 Speed, Byte는 Data Size에 쓰인다
    - 100Mbps 속도 = 100 Mega bit per second, SSD 50GB = 50 Giga Byte
    - Kilo, Mega, Giga, Tera
    - 전산학에서는 2진수 기반이기 때문에 2의 10승은 1024로 표현
    - 1024K = 1M, 1024M = 1G, 1024G = 1T
  - Performance
    - Bandwidth(대역폭)
      - 주어진 시간대에 네트워크를 통해 이동할 수 있는 정보의 양
    - Throughput(처리량)
      - 단위 시간당 디지털 데이터 전송으로 처리하는 양
    - 대역폭이 8차선 도로라면 처리량은 그 도로를 달리는 자동차의 숫자(양)와 같다
    - BackPlane
      - 네트워크 장비가 최대로 처리할 수 있는 데이터 용량
    - CPS(Connection Per Second): 초당 커넥션 연결수, L4
    - CC(Concurrent Connection): 최대 수용가능한 커넥션
    - TPS(Transaction Per Seconds): 초당 트랜젝션 연결수, L7, 주로 HTTP 성능

- Wrap up

  - 물리계층의 대표적 장비로는 허브와 리피터가 있다
  - 허브는 브로드캐스팅 통신과 CSMA/CD 방식을 사용하며 Half Duplex 모드다
  - 주로 사용되는 케이블의 종류는 Coxial, Fiber
  - 데이터 단위는 bit & Byte가 있으며 Kilo, Mega, Giga, Tera로 표현
  - 장비의 Capacity는 Bandwidth, Throughput, Backplace 로 설명

## UTP 케이블과 Wi-Fi

---

- UTP 케이블이란 ?

  - 정의
    - Unshielded Twisted Pair, 주로 근거리 통신망(LAN)에서 사용되는 케이블
    - 이더넷 망 구성시 가장 많이 보게되는 케이블
    - 알렉산더 그레이엄 벨이 AT&T에서 발명

- 코드 배열

  - 8P8C
    - 8개의 선 배열에 따라 다이렉트 또는 크로스 케이블로 구성한다
    - RJ-45 커넥터 사용
    - ex> TIA-568A, TIA-568B
    - Direct Cable(568B-568B): PC to Hub -> DTE to DCE
    - Cross Cable(568A-568B): PC to PC, Hub to Hub -> DTE to DTE, DCE to DCE
    - DTE: Data Terminal Equipment
    - DCE: Data Communication Equipment
  - Standard
    - ISO / IEC 11801
    - Copper & Fiber 케이블 등을 정의
    - TIA-568(Telecommunications Industry Association)
      - 통신 제품 및 서비스를 위한 상업용 케이블 스펙을 정의
    - EIA-568(Electronic Industries Alliance)
      - 최초 통신 시스템 케이블링의 표준을 정의했고 이후 TIA로 이관
  - Auto MDI-X(Automatic Medium Dependent Interface Crossover)
    - 어떤 노드의 연결인지에 따라서 다이렉트와 크로스 케이블을 선택 -> 불편
    - 케이블 타입에 관계없이 노드 상호간 자동으로 통신이 가능하게 하는 기술
    - MDI 포트 -> DTE & MDIX 포트 -> DCE, 송신과 수신의 관계

- UTP 카테고리

  - 정의

    - UTP 케이블의 전송 가능한 대여폭을 기준으로 분류

    | 이름   | 최대 가능 속도 | 최대 케이블 길이 | 설명                       |
    | ------ | -------------- | ---------------- | -------------------------- |
    | Cat 3  | 10 Mbps        | 100m             | 주로 전화선                |
    | Cat 4  | 16 Mbps        | 100m             | 거의 안쓰임                |
    | Cat 5  | 100 Mbps       | 100m             | 주로 100M LAN 환경         |
    | Cat 5e | 1 Gbps         | 100m             | 최근 100M - 1G LAN 환경    |
    | Cat 6  | 10 Gbps        | 100m             | 10G 통신시 55m 까지만 가능 |

- Wi-Fi란 ?

  - 정의
    - 비영리 기구인 Wi-Fi Aliance의 상표로 전자기기들이 무선랜에 연결할 수 있게 하는 기술
    - 1999년 몇몇 회사들이 브랜드에 상관 없이 무선 네트워킹 기술의 발전을 위해 협회 결성
    - 2000년 Wi-Fi 용어 채택
    - 수십개 나라에서 수백개 회사가 참여
    - 802.11n Wi-Fi 4, 802.11ac Wi-Fi, 802.11ax Wi-Fi 6로 불림

- 무선랜 구성

  - 인터넷 - ISP - 라우터 - WIPS - AP - 컴퓨터(무선랜카드)
  - WIPS(Wireless IPS)
  - AP(Access Point)
  - WIPS - AP - 컴퓨터 를 한쌍으로 보면 됨
  - 고려사항
    - AP의 반경과 동시접속 단말기의 개수
    - 802.11 무선랜 규격 확인

- Wrap up

  - UTP(Unshielded Twisted Pair, 주로 근거리 통신망(LAN)에서 사용되는 케이블)
  - RJ-45 커넥터를 사용하며 TIA-568A & TIA-568B 배열을 통해서 다이렉트 & 크로스 케이블로 구분
  - Auto MDI-X는 케이블 타입에 관계없이 자동으로 통신이 가능한 기술
  - UTP는 회선 속도 및 쓰이는 용도에 따라서 분류
  - Wi-Fi는 비영리 기구인 Wi-Fi Aliance의 상표로 전자기기들이 무선랜에 연결할 수 있게 하는 기술
  - 무선랜 구성시 WIPS(보안), AP(무선 Hub)가 필요, AP반경과 동시접속 단말의 수를 고려해야 함
