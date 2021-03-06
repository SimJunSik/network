# ch.01

# 네트워크 소개

## 네트워크 구조

---

- 네트워크 구조

  - 규모
    회사나 학교 등의 집단 크기에 따라 구분 - 사용자, 대역폭
  - 업종
    공공기관, 제조, 금융, 게임 등 업종에 따른 서비스 중요도
  - 통신 방식과 경로
    Server & Client, Peer to Peer
  - 토폴로지
    Star, Ring, Mesh, Bus, Tree, Redundancy - Tree를 가장 많이 사용

- 홈 네트워크

  - 인터넷 - ISP(Internet Service Provider) - 모뎀 - 공유기 - 컴퓨터

- 기업용 네트워크

  - ISP - 전용선 - 라우터 - 방화벽
    - L3 백본 - L2 스위치 - 서버, 컴퓨터
    - L4 로드밸런서 - DMZ

- 클라우드 네트워크(AWS 기준)

  - 인터넷 - Route53 - IGW - VPC - ELB - Auto Scaling - Security Group - EC2
    - DNS : URL -> IP주소 변경해주는 서비스
      AWS 에서는 Route53

- Wrap up

  - 네트워크의 구조는 크게 규모, 서비스, 통신방식, 구현방식 등을 통해서 설계된다
  - 네트워크는 Star, Ring, Mesh, Bus, Tree, Redundancy 형태가 있다
  - 네트워크는 기본적으로 홈, 기업, 클라우드 형태가 있다
    기업의 일반적인 구성은 아래와 같다 - ISP - 전용선 - 라우터 - 방화벽 - L3 백본 - L2 스위치 - 서버, 컴퓨터 - L4 로드밸런서 - DMZ

## 네트워크 정의 및 역사

---

- 네트워크란?

  - 분산되어 있는 컴퓨터들이 자원을 공유할 수 있게 통신망으로 연결한 것

- 네트워크의 중요성

  - 3차 산업혁멍
    - 디지털 + 인터넷(공유) = 지식 정보화 사회 -> 급격한 기술의 발전
  - 4차 산업혁명
    - 정보통신 기술의 융합 : 대표 요소 기술로 빅데이터, 인공지능, 사물인터넷 등 7가지
    - On Network : 대부분 요소 기술들은 네트워크 위에서 동작
    - 블록체인 기술은 신뢰 가능한 인터넷
  - FAANG
    - Facebook, Apple, Amazon, Netflix, Google
    - Network 연결에서 동작하는 서비스
  - 삼성
    - 한국에서 세계최초 모바일 5G 서비스 상용화
  - 현대
    - 제조에서 ICT 기업으로, 자율주행차 투자
  - LG
    - 자율주행 및 클라우드 서비스
  - 구글 GCP
    - 서울에 데이터센터 구축

- 네트워크의 역사

  - 미국
    - 1940 : 전신기를 통해 계산기에 명령어 전달
    - 1962 : 컴퓨터에 전신기 연결
    - 1964 : 시분할 시스템 개발 / 전화 연결 및 제어
    - 1968 : 패킷 스위칭 / 네트워크 시스템 제안
    - 1969 : 아파넷 발족 / 4개 대학으로 망 구성
  - 대학민국
    - 1982 : 서울대와 구미 연구소 패킷 스위칭 연결
    - 1986 : LGU+ PC 통신 천리안
    - 1994 : KT 인터넷 상용 서비스
    - 1998 : SK브로드밴드 초고속 인터넷 도입
    - 2010 : 인터넷 보급률 95%

- 네트워크 형태

  - LAN(Local Area Network) : 근거리 통신망
    - 사무실 또는 학교 등의 가까운 지역을 한데 묶은 네트워크
  - WAN(Wide Area Network) : 장거리 통신망
    - 각각 떨어진 LAN 망을 연결, 규모가 큰 네트워크, ISP로 연결
  - VPN(Virtual Private Network) : 가상 사설망
    - 공중망을 사설망처럼 사용, 암호화

- 네트워크 표준

  - 네트워크 표준 기구
    - ISO 국제표준화 기구
    - IEEE 미국전기전자협회(주로 LAN)
    - ITU-T 국제전기통신연합 통신표준본부(주로 WAN)
  - 인터넷 표준 기구
    - IETF 인터넷 엔지니어 테스크포스
    - RFC - 프로토콜 정의 문서
  - 이더넷 -> IEEE 802.3
  - TCP/IP -> RFC 1122 & 1123, HTTP/1.1 -> RFC 2616
  - OSI 7 Layer -> ISO Standard Model

- Wrap up

  - 네트워크는 분산된 컴퓨터들이 자원을 공유할 수 있게 연결한 것
  - 네트워크는 4차 산업혁명 요소 기술들과 긴밀하게 연결
    - 글로벌 기업과 대기업의 중요한 인프라 기술
  - 1940년 미국에서 전신기를 통한 계산기에 명령어 전달로 시작
    1969년 아파넷을 통한 대학교간 망 구성 -> 인터넷으로 진화
  - 1982년 국내 최초 서울대와 구미 전자연구소 연결
    2010년 인터넷 보급률 95%로 성장
  - 네트워크 형태
    - 물리적으로는 LAN & WAN 이 있고
    - 논리적으로는 VPN 이 있다
  - 네트워크 표준기구
    - ISO, IEEE, ITU-T, 인터넷 표준기구 IETF, 프로토콜 정의 문서 RFC

## OSI 7 Layer 모델

---

- OSI 7 Layer ?

  - 정의
    - 네트워크 프로토콜과 통신을 7 계층으로 표현한 것
  - 목적
    - 프로토콜을 기능별로 나누고 계층 별로 구분
    - 벤더간 호환성을 위한 표준 필요 -> 쉬운 접근성으로 기술의 발전
  - 역사
    - 1970년대 초 네트워크는 정부 또는 특정 벤더에서 독점 개발, 공개형 모델 필요
    - 1970년대 말 ISO(국제 표준화 기구)에 의해 관리
    - 1984년 ISO 7498 릴리즈

- OSI 7 Layer 모델

  - 아래로 갈수록 기계가 이해하는 언어
    위로 갈수록 사람이 이해하는 언어
  - Physical(Layer 1) ~ Application(Layer 7)
    - Application
      - 응용 서비스 : HTTP(웹), SMTP(메일) ..
    - Presentation
      - 인코딩 / 암호화 / 압축
    - Session
      - TCP / IP 통신 연결을 수립 / 유지 / 중단
    - Transport
      - TCP / UDP
      - 각각의 IP 통신을 하고나서 어떠한 서비스를 하게 될지를 정함 EX> 웹 서비스, 메일 서비스, DNS ..
      - 각각의 서비스는 각각의 포트가 있고 이러한 것들을 정의해주는 계층
    - Network
      - IP 통신, 라우팅
    - Data Link
      - 이더넷, 랜카드, Mac 통신, 에러검출/재전송
    - Physical
      - 네트워크 하드웨어 전송 기술

- 각 계층별 기능

  - 1 계층 : Physical
    - 기능
      - 장치와 통신 매체 사이의 비정형 데이터의 전송을 담당
      - 디지털 bit(0 or 1)를 전기, 무선 또는 광 신호로 변환
    - 전송되는 방법, 제어 신호, 기계쩍 속성 등을 정의
    - 케이블, 인터페이스, 허브, 리피터 등이 이에 속함
  - 2 계층 : Data Link
    - 기능
      - `동일 네트워크 내`에서 데이터 전송, 링크를 통해서 연결을 설정하고 관리
      - 물리계층에서 발생할 수 있는 오류를 감지하고 수정
    - IEEE 802 에서 정의
    - MAC(Media Access Control), LLC(Logical Link Control)
    - 모뎀, 스위치
  - 3 계층 : Network
    - 기능
      - `다른 네트워크`로 데이터 전송, IP(Internet Protocol) 주소로 통신
      - 출발지 IP에서 목적지 IP로 데이터 통신 시 중간에서 라우팅 처리
      - 데이터가 큰 경우 분할 및 전송 후 목적지에서 재조립하여 메시지 구현
      - `IP통신`과 `라우팅`
    - L3 스위치, 라우터
  - 4 계층 : Transport
    - 기능
      - 호스트 간의 데이터(서비스) 전송
      - 오류 복구 및 흐름 제어, 완벽한 데이터 전송을 보장
    - TCP / UDP
    - L4 계층을 특정 하드웨어로 구분하기 모호
    - Port를 제어한다는 의미로 L4 로드 벨런서가 있음
  - 5 계층 : Session
    - 기능
      - 로컬 및 원격 어플리케이션 간의 IP / Port 연결을 관리
      - Session Table 로 관리(cmd > cetstat - an)
  - 6 계층 : Presentation
    - 기능
      - 사용자 프로그램과 네트워크 형식간에 데이터를 변환하여 표현과 독립성을 제공
      - 인코딩, 디코딩, 암호화, 압축
      - ASCII, JPG, MPEG
  - 7 계층 : Application
    - 기능
      - 사용자와 가장 밀접한 소프트웨어
      - 이메일 서비스 SMTP, 파일전송 FTP 등

- Wrap up

  - OSI 7 Layer 는 네트워크 프로토콜과 통신을 7 계층으로 표현한 표준 모델
  - 벤더간 호환성을 위해 표준 제정 -> 기술의 발전
  - 1970년대 말 ISO(국제 표준 기구)에 의해 관리되었고 1984년에 ISO 7498 릴리즈
  - OSI 7 Layer
    - Application
      - 응용 서비스 : HTTP(웹), SMTP(메일) ..
    - Presentation
      - 인코딩 / 암호화 / 압축
    - Session
      - TCP / IP 통신 연결을 수립 / 유지 / 중단
    - Transport
      - TCP / UDP
      - 각각의 IP 통신을 하고나서 어떠한 서비스를 하게 될지를 정함 EX> 웹 서비스, 메일 서비스, DNS ..
      - 각각의 서비스는 각각의 포트가 있고 이러한 것들을 정의해주는 계층
    - Network
      - IP 통신, 라우팅
    - Data Link
      - 이더넷, 랜카드, Mac 통신, 에러검출/재전송
    - Physical
      - 네트워크 하드웨어 전송 기술

## TCP/IP 모델 비교와 캡슐화

---

- TCP/IP 란 ?

  - 정의
    - 네트워크 프로토콜의 모음으로 패킷 통신 방식의 IP와 전송 조절 프로토콜인 TCP로 이루어져 있다
  - 역사
    - 1960년대 말 방위고등연구계획국(DARPA)이 연구
    - 1990년대 네트워크 표준이 ISO 모델과 TCP/IP 모델로 좁혀짐
    - 1990년대 말 TCP/IP 모델이 자주 쓰이면서 가장 일반적인 모델이 됨

- TCP/IP 모델

  - Network Interface(Layer 1) ~ Application(Layer 4)
  - Application
    - 응용 프로그램간 표준화 된 데이터 교환
  - Transport
    - TCP/UDP
  - Network(Internet)
    - 패킬을 처리하고 다른 네트워크로 연결
  - Network Interface
    - 물리계층으로 네트워크 노드들을 상호 연결

- TCP/IP 와 OSI 7 Layer 비교

  - 도식화 비교

- 캡슐화

  - 인캡슐레이션
    - 사용자가 다른 사용자에게 어떤 Data를 보낼 때 이루어지는 과정
    - Application , Presentation 단계에서 Host Data
    - Session 단계에서는 위의 Host Data를 컴퓨터가 이해할 수 있는 정형화 된 Data로 변환
    - Transport 단계에서는 단위가 Segment
      - 보내는 Data가 어떤 Port를 사용하는지 등을 TCP Header에 담아서 Session 단계에서 받은 Data 앞에 붙임
    - Network 단계에서는 단위가 Packet
      - 보내는 Data가 어떤 IP로 보내는지 등을 IP Header에 담아서 Transport 단계에서 받은 Segment 앞에 붙임
    - Data Link 단계에서는 단위가 Frame
      - 보내는 Data의 목표 MAC, LLC Header를 Network 단계에서 받은 Packet 앞에 붙임
      - 오류 검출
    - Physical 단계에서는 단위가 bit
      - Data Link 단계에서 받은 Frame을 전기신호로 변환
  - 디캡슐레이션
    - Data를 받는 입장에서는 인캡슐레이션의 반대 과정으로, Physical Layer가 전기 신호 bit를 받아서 Header를 detach하며 진행

- Wrap up

  - TCP/IP 모델은 패킷 통신 방식의 IP와 전송 조절 프로토콜인 TCP로 이루어져 있다
  - 1960년대 말 개발되어 1990년대 말 네트워크 통신의 일반적인 모델이 됨
  - TCP/IP와 OSI 7 Layer 비교
  - 네트워크는 캡슐화를 통해서 통신한다
