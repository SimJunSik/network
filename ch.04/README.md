# ch.04

# IP주소와 라우팅

## 네트워크 계층의 역할과 IP의 구조

---

- 네트워크 계층

  - 역할
    - OSI 7 Layer의 3계층으로 패킷 포워딩과 네트워크간 라우터를 통한 라우팅 수행
    - IP(Internet Protocol) 주소를 사용하여 통신, 계층적 구조
    - 대표적인 장비는 라우터, 또는 L3 스위치 라고 부른다

- IP 정의와 구조

  - IP(Internet Protocol)
    - 네트워크 계층에서 통신하는 주요 프로토콜로 라우팅을 구현하고 본질적인 인터넷을 구축하는 계기
    - 1974년 IEEE 논문 발표 "A Protocol for Packet Network Intercommunication"
    - 전송 제어 프로그램의 비연결 데이터그램 서비스로 시작 -> 연결 지향 서비스로 보완
    - RFC 760 -> RFC 761 IP, Conection
    - RFC 761 -> RFC 793 TCP, Connection-oriented service
    - TCP/IP 모델의 기원
    - 현재 사용중인 버전은 IPv4이며 후속 버전으로 IPv6 릴리즈
  - IP 주소 확인
    - Windows : CMD > ipconfig
    - Linux 계열 : ifconfig
    - 제어판 > 네트워크 및 인터넷 > 연결된 인터페이스 확인
  - IP 구조
    - IP는 헤더와 페이로드로 구성되어 있다
    - 헤더는 목적지 & 출발지 IP 주소 등을 포함, 페이로드는 전송되는 데이터를 의미
  - IPv4 헤더 구조
    - 최소 20바이트(옵션 미 지정시)
    - Version
      - IP 버전, IPv4
    - Header Length(HLEN)
      - 헤더의 길이
      - 4바이트 단위 최소 5(20바이트) ~ 15(60바이트)
    - Type of Service
      - 서비스 품질
    - Total Packet Length
      - IP 패킷 전체의 길이 - 바이트 단위 - 최대 65,535
    - Identifier, Flags, Offset
      - IP Fragment 필드로 단편화와 재조합
      - 큰 패킷이 작은 패킷으로 적용되는 경우
    - Time to Live
      - IP 패킷 수명
      - 잘못된 IP 거나 경로가 없는 경우가 있기 때문에 Looping이 돌게하지 않기 위해 TTL을 셋팅해놔야 함
    - Protocol ID
      - 데이터에 포함되어 있는 상위 계층의 프로토콜 정보
      - TCP 6, UDP 17
    - Header Checksum
      - 오류 검출
    - Source, Destination IP Address
      - 출발지 & 목적지 IP 주소
    - IP Header Options & Padding
      - 옵션, 거의 사용되지 않음
      - 시험/디버깅 용도
      - 통신에는 미관여

- IP 주소 클래스

  - IP 주소 구성
    - IP주소는 2진수 32비트로 구성
      예) 10101010.01101001.01010101.1001001
    - 총 2의 32승 = 42억 9천여개
    - 최초 IP주소 설계시 충분한 수량이었으나 현재는 거의 고갈된 상태
    - 한국인터넷정보센터에서 IP 주소 할당 확인
    - 2진수는 어렵기 때문에 일반적으로 10진수로 표현
      예) 168.126.63.1
    - 2의 8승은 256 = 10진수 한 옥텟은 최대 0 ~ 255 까지 가능
  - 2진수 -> 10진수 표햔
    - 2진수는 0 & 1, 즉 2개로 구분
    - 10진수는 0 ~ 9까지 총 10개로 표현
    - 2의 0승 = 1 = 00000001
    - 2의 1승 = 2 = 00000010
    - 2의 2승 = 4 = 00000100
    - 2진수 11001001 = 128 + 64 + 8 + 1 = 201
  - 네트워크와 호스트
    - IP주소는 네트워크 부분과 호스트 부분으로 나뉜다
    - 네트워크는 브로드캐스트 영역, 호스트는 개별 단말기
      예) 192.168.1.0 ~ 255 에서 192.168.1 = 네트워크, 0 ~ 255 = 호스트
    - 수원, 안양, 용인 = 네트워크 = 라우터
    - 철수, 민수, 영희 = 호스트 = PC
    - 시내 = 브로드캐스트 스위칭, 시외 = 라우팅
    ```
    수원.철수 = 192.168.1.1
    수원.민수 = 192.168.1.2
    안양.영희 = 192.168.2.1
    ```
  - IP 주소 클래스
    - IP주소는 네트워크의 크기에 따라 5개의 클래스(A, B, C, D, E)로 구분
    - A 클래스
      - 0.0.0.0 ~ 127.255.255.255
      - 호스트는 2의 24승
    - B 클래스
      - 128.0.0.0 ~ 191.255.255.255
      - 호스트는 2의 16승
    - C 클래스
      - 192.0.0.0 ~ 223.255.255.255
      - 호스트는 2의 8승
    - D & E 클래스
      - 멀티캐스트용(224.0.0.0 ~ 239.255.255.255)
      - 연구용(240.0.0.0 ~ 255.255.255.254)

- Wrap up

  - 네트워크 계층은 패킷 포워딩과 네트워크간 라우팅을 수행
  - 주요 프로토콜로 IP(Internet Protocol)가 있으며 1974년 IPv4 공개
  - IPv4 헤더 구조
  - IPv4의 주소는 32비트로 구성되며 2의 32승으로 약 42억 9천여개
  - IP는 네트워크와 호스트로 나뉘며 크기와 용도에 따라 5개의 클래스로 구분한다

## 라우터와 서브넷팅

---

- 라우터의 이해

  - 역할
    - 목적지 IP주소를 확인하고 하나 또는 그 이상의 네트워크 간의 패킷의 경로를 선택하여 전송
    - Rout`er`
      - 네트워크 간의 패킷을 전송해주는 장비
    - Rout`ing`
      - 네트워크 간의 패킷을 전달하는 경로를 선택하는 과정
      - Static & Dynamic
    - Rout`ed`
      - 라우어터가 라우팅을 해주는 대상
      - IP
  - 인터페이스
    - 라우터의 접속 가능한 포트로 통신용 & 관리용으로 구분
    - 통신용은 UTP, 광, 무선으로 구성, WAN(Router to Router) 연결은 시리얼 포트도 존재
    - 관리용은 보통 콘솔이라 부르며 원격에서 접속 불가 또는 장애시 장비에 직접 연결할 때 사용
    - RS232, 시리얼 포트

- 서브넷 마스크

  - 개념
    - 서브넷
      - 부분망, IP주소는 네트워크와 호스트로 구분
    - 할당된 네트워크 영역을 좀 더 효율적으로 사용하기 위해 서브넷으로 쪼개서 구성
    - 네트워크를 여러개의 작은 네트워크 서브넷으로 구분하는것
      - 서브넷 마스크
    - ex> 총 C클래스 254개의 IP할당 211.109.131.0 ~ 255
    - 네트워크 별 첫번째 숫자와 마지막 숫자는 Reversed로 사용하지 않음
      - 첫번째 숫자
        - 네트워크 영역을 알림
      - 마지막 숫자
        - 브로드캐스트 주소
    1. 254개의 IP주소를 할당했으나 R2망은 3개의 IP만 필요
    2. R2망은 총 6개 IP가 필요함을 확인
       - 3명(Bob, Kim, Linda) & `게이트웨이 주소` 1개
       - 네트워크 & 브로드캐스트 주소 2개
    3. 2의 3승 = 8개로 서브넷 구성이 가장 효율적
    4. 211.109.131.0 ~ 7, 211.109.131.0/29 로 구성
  - 상세
    - 디폴트 게이트웨이(라우터)
      - 다른 네트워크로 패킷 전송시 거쳐야 하는 거점
    - Prefix 표기법
      - 서브넷 마스크 표기를 간단히 표현
      - 네트워크 영역의 비트 '1'의 개수를 의미
      - A 클래스: 255.0.0.0 = /8
      - B 클래스: 255.255.0.0 = /16
      - C 클래스: 255.255.255.0 = /24
      - 비트 수가 올라갈수록 호스트 갯수는 줄어듬
    - 여러 경로를 가진 라우터의 경우는 보통 1개의 경로를 디폴트 게이트웨이로 설정
  - 계산법
    - 서브넷 마스크는 2진수로 '1'인 부분은 네트워크, '0'인 부분은 호스트가 된다
    - 1 AND 1 = 1 의 공식으로 네트워크 주소를 확인한다
    ```
    IP주소 : 209.217.12.11 ,서브넷 마스크 : 255.255.255.0
    11010001.11011001.00001100.00001011 = 209.217.12.11
    11111111.11111111.11111111.00000000 = 255.255.255.0
    11010001.11011001.00001100.00000000 = 209.217.12.0 -> 서브넷 네트워크
    ```
    - 255.255.255.0은 호스트영역 '0'이 8개이므로 2의 8승 = 256 개의 IP 할당, 209.217.12.0/24로 표현
  - 예제
    - IP주소 8.8.8.114, 서브넷 마스크 255.255.255.192의 네트워크 주소와 할당된 IP 개수는?
    ```
    00001000.00001000.00001000.01110010 = 8.8.8.114
    11111111.11111111.11111111.11000000 = 255.255.255.192
    00001000.00001000.00001000.01000000 = 8.8.8.64 -> 네트워크 주소
    ```
    - 호스트 '0'이 6개 = 2의 6승 = 64개 할당됨
    - 8.8.8.64 ~ 127까지 호스트 범위
    - 8.8.8.64/26
    - 서브넷 마스크는 LAN 설계시 C클래스(256개) 단위로 많이 나눔
    - 2의 n승을 외워두면 계산에 도움이 됨
      - 128, 64, 32, 16, 8 ,4, 2, 1
    - /24 = 256개를 기준으로,
      - /25 = 128, /26 = 64, /27 = 32, /28 = 16, /29 = 8, /30 = 4, /32 = 1
    - 서브넷 마스크 계산기
      - http://www.subnet-calculator.com/

- 라우터의 동작 방식

  - Static 라우팅
    - 가장 기본적인 라우팅 방식으로 수동으로 경로를 라우터에 설정하여 패킷을 처리한다
    - 경로는 `라우팅 테이블`에 목적지 IP주소 & 인터페이스 정보를 설정
  - Hop & TTL
    - 전세계 네트워크 호스트는 IP 라우팅을 통해서 연결
    - Hop
      - 소스와 목적지 간의 경로
    - TTL(Time To Live)
      - 패킷이 폐기되기 전 Hop 카운트
    - 각 라우터는 패킷이 인입되면 TTL값을 1씩 감소, TTL = 0이 되면 폐기, 부정확한 패킷의 루프 방지
  - Traceroute or Tracert
    - 라우팅 경로 확인 명령어
    - 출발지에서 목적지 IP까지 거치는 라우터 최적 경로 확인
    - ex) cmd > tracert -d www.fastcampus.co.kr

- Wrap up

  - 라우터는 목적지 IP주소를 확인하고 하나 또는 그 이상의 네트워크 간의 패킷의 경로를 선택하여 전송
  - 서브넷 마스크는 네트워크를 여러개의 작은 부분 네트워크로 구성하는 것
  - Static 라우팅은 라우팅 테이블의 목적지 IP & Next Hop을 참조하여 경로 선택
  - 라우팅 테이블은 커넥티드, 수동(정적), 디폴트 라우팅 설정이 포함된다
  - Tracerout 명렁어를 통해서 라우팅 경로 정보를 추적할 수 있다

## ICMP

---

- ICMP의 정의

  - ICMP(Internet Control Message Protocol)
    - 인터넷 제어 메시지 프로토콜
    - IP 통신은 목적지에 패킷을 정상적으로 전달하는 방법은 있지만 에러 발생시 처리 불가
    - ICMP는 IP 통신의 에러 상황을 출발지에 전달 & 메시지 제어 역할
    - RFC 792, 1981년 소개됨
    - ICMP는 IPv4 패킷으로 캡슐화
    - Protocol ID = 1
    - Ping & Tracerout 명령어를 사용

- ICMP의 기능

  - ICMP 포맷 구조
    - IP 패킷에 포함
    ```
                8          16              31
    |               IP 헤더                 |
    |   Type    |   Code    |   Checksum    |
    |   가변길이(Type & Code 에 따라 달라짐)  |
    ```
    - Type
      - ICMP 메시지 종류
    - Code
      - 메시지 Type 별 세부 코드 정보
    - Checksum
      - ICMP 헤더 손상 여부 확인
  - ICMP Type
    - 0 ~ 254 까지 정의
    - 주로 쓰이는 타입은 아래와 같으며 오류 보고 & 정보성으로 나눈다
    - 정보용: 8, 0, 9, 10
    - 오류 보고용: 3, 5, 11, 12
  - Type 8 & 0 / Echo Request & Reply
    - 네트워크 문제 진단시 사용
    - 출발지에서 목적지 IP로 ICMP Echo Request 메시지를 보내면 목적지는 Echo Reply로 응답
    - 목적지 도달 여부, RTT(Round-Trip delay Time), hop count 확인
    - TTL 값에 따라서 일반적인 OS 종류를 알 수 있다
    - Windows 계열 128, Linux 계열 64
  - Type 9 & 10 / 라우터 광고 & 정보 요청
    - 자신이 라우터 임을 응답 & 네트워크 진입시 라우터 정보 요청
    - MAC과 IP 정보가 있어야 함
  - Type 3 Destination Unreachable & 5 Redirect
    - 라우터가 IP 패킷을 라우팅 하지 못하는 경우에 발생
    - 0 = net unreachable
    - 1 = host unreachable
    - 2 = protocol unreachable
    - 3 = port unreachable
    - 4 = fragmentation needed and DF set
    - 5 = source route failed
    - Type 5 Redirect
      - 로컬 네트워크에 2개 이상의 경로가 있을 때 더 좋은 경로를 알려줌
  - Type 11 Time Exceeded & 12 Parameter Problem
    - 시간 초과, TTL 값이 '0'이 되면 출발지에게 응답
    - 0 = Time to Live Exceeded
    - 1 = Fragement Reassembly Time Exceeded
    - IP Fragmentation
      - IP 패킷을 작은 패킷으로 나누어서 전송하고 목적지에서 재조합
    - MTU(Maximum Transmission Unit)
      - IP 패킷을 전송할 수 있는 최대 크기
    - Type 12 Parameter Problem
      - IP 옵션을 잘못 사용하여 라우터에 패킷 폐기

- Wrap up

  - ICMP(Internet Control Message Protocol)
  - IP 통신의 에러 상황을 출발지에 전달 또는 메시지 제어 역할
  - 포맷

  ```
              8          16              31
  |               IP 헤더                 |
  |   Type    |   Code    |   Checksum    |
  |   가변길이(Type & Code 에 따라 달라짐)  |
  ```

  - 주 타입은 정보용(8, 0, 9, 10)과 오류 보고용(3, 5, 11, 12)으로 구분
  - Ping & Tracerout 명령어를 통해서 사용된다

## DHCP

---

- DHCP 정의

  - DHCP(Dynamic Host Control Protocol)
    - 동적 호스트 구성 프로토콜
    - DHCP 서버를 사용하여 클라이언트인 네트워크 장치에 IP 주소를 자동으로 할당
    - 1984년 RARP(Reverse Address Resolution Protocol) 도입 - RFC 903
    - 1985년 BOOTP(Bootstrap Protocol) - RFC 931
    - 1993년 DHCP - RFC 1541 -> RFC 2131
    - 요청에 의한 IP 할당으로 효율성 극대화
    - 잘못된 IP 설정으로 인한 장애 예방
    - IP 변경이 잦은 호스트의 관리
  - DHCP 메시지 포맷 설명
    - OpCode
      - 1 Request (Client -> Server)
      - 2 Reply (Server -> Client)
    - Hardware Type
      - 1, Ethernet
    - Hardware address length
      - 6, MAC address
    - Hop count
      - 0 에서 시작, 네트워크 망 이동시 증가
    - Transaction ID
      - 클라이언트가 선택하는 랜덤 수, 요청과 응답 매칭
    - Seconds
      - IP 할당 후 경과한 초의 수
    - Flags
      - 서버 응담에 대해서 0 - unicast 또는 1 - broadcast 응답 구분 값
    - Client IP
      - 최초 0.0.0.0
    - Your IP
      - 할당될 IP
    - Options
      - DHCP 메시지 타입 포함

- DHCP 동작 과정

  - IP 할당
    - 기본 네트워크 구성, Gateway - Switch - DHCP Server - PC
    1. DHCPDISCOVER
       - PC는 DHCP Server를 발견
    2. DHCPOFFER
       - DHCP Server는 PC에게 IP 제안
    3. DHCPREQUEST
       - PC는 제안 받은 IP 할당을 요청
    4. DHCPACK
       - DHCP Server는 요청 수락
  - IP 갱신
    - 지정된 IP 갱신 타임이 도래하면 갱신을 요청
    1. DHCPREQUEST
       - PC는 기존IP 재할당을 요청
    2. DHCPACK
       - DHCP Server는 IP 확인 후 요청 수락
  - IP 해제
    - 사용중인 PC가 전원 off 되는 경우
    1. DHCPRELEASE
       - PC는 더이상 IP 할당이 필요없음을 알림
  - DHCP 자동 할당 정보
    - cmd > ipconfig /all

- Wrap up

  - DHCP(Dynamic Host Control Protocol)
    - 동적 호스트 구성 프로토콜
  - DHCP 서버를 사용하여 클라이언트인 네트워크 장치에 IP 주소를 자동으로 할당
  - 여러 DHCP 메시지 타입이 정의되어 있다
  - DHCP 동작은 IP 할당, 갱신, 해제의 과정이 있다
