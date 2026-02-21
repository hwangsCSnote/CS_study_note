# TCP/IP 4계층 모델  

**네트워크의 기능**  
- 애플리케이션에 목적에 맞는 통신 방법 제공  
- 신뢰할 수 있는 데이터 전송 방법 제공  
- 네트워크 간의 최적의 통신 경로 결정  
- 목적지 데이터 전송  
- 노드 사이 데이터 전송  

네트워크의 기능을 수행하기 위해서는 **약속된 통신 방법/규칙**이 필요하다.  

**네트워크 프로토콜**(Network Protocol)  
> 네트워크 통신을 위해 통신 주체들이 따르는 형식, 절차, 규약  

네트워크의 모든 기능을 하나의 프로토콜로 구현할 수 없다.  
애플리케이션에서 다양한 기능들을 각각 ‘모듈화’ 하는 것 처럼  
네트워크의 각 기능을 **계층 구조**(Layer Architecture)로 나눌 수 있다.  

<br>

**인터넷 프로토콜 스위트**(Internet Protocol Suite)  
> 인터넷에서 컴퓨터들이 서로 정보를 주고받는데 쓰이는 프로토콜의 집합으로  
> **TCP/IP 4계층** 모델 또는 **OSI 7계층** 모델로 설명한다.  

<br>

### 계층 구조  

<img width="486" alt="image" src="https://github.com/user-attachments/assets/7da8a7f8-89a7-4282-b82a-2238ce13bf99" />

**OSI 7계층**  
- `애플리케이션 계층` (Application Layer)  
- `표현 계층` (Presentation Layer)  
- `세션 계층` (Session Layer)  
- `전송 계층` (Transport Layer)  
- `네트워크 계층` (Network Layer)  
- `데이터 링크 계층` (Data Link Layer)  
- `물리 계층` (Physical Layer)  

**TCP/IP 4계층**  
- `애플리케이션 계층` (Application Layer)  
- `전송 계층` (Transport Layer)  
- `인터넷 계층` (Internet Layer)  
- `링크 계층` (Link Layer)  

<br>

4. `애플리케이션 계층` (Application Layer)  
> FTP, HTTP, SSH, SMTP, DNS 등 응용 프로그램이 사용되는 프로토콜 계층  
- 웹 서비스, 이메일 등 서비스를 실질적으로 사용자에게 정보를 제공하는 층  

※ FTP(File Transfer Protocol) : 장치와 장치 간 **파일 전송** 시 사용되는 프로토콜  
※ SSH(Secure Shell) : 보안되지 않은 네트워크를 안전하게 운영하기 위한 **암호화** 네트워크 프로토콜  
※ HTTP(Hyper Text Transfer Protocol) : **웹 데이터 통신**을 위한 프로토콜  
※ SMTP(Simple Mail Transfer Protocol) : **전자 메일 전송**을 위한 인터넷 표준 통신 프로토콜  
※ DNS(Domain Name System): **도메인 이름과 IP 주소를 매핑**해주는 시스템  

<br>

3. `전송 계층` (Transport Layer)  
> 송신자와 수신자를 연결하는 통신 서비스를 제공하는 프로토콜 계층  
- 연결 지향 데이터 스트림 지원, 신뢰성, 흐름 제어를 제공  
- 대표적으로 TCP, UDP가 있음  

※ **TCP**(Transmission Control Protocol)  
: 기기 간 데이터를 안전하고, 정확하며, 순서대로 전달하기 위한 프로토콜 (연결 지향형)  
- 신뢰성, 연결형, 느린 속도  
- ex) 파일 전송, 웹 브라우징  
- **가상회선 패킷 교환 방식** : 각 패킷에 가상회선 식별자가 포함되며 모든 패킷 전송 시 가상회선이 해제되고 패킷들은 전송된 순서대로 도착하는 방식  
  1. 송신측과 수신측 사이 논리적인 가상 통신 회선을 미리 설정  
  2. 설정된 경로에 따라 패킷들을 순서대로 전송  
  3. 송신 순서와 수신 순서가 동일하여 신뢰성이 보장됨  
  <img width="507" alt="image 2" src="https://github.com/user-attachments/assets/71d2b99d-6c54-4d9d-bded-518060b3df95" />


※ **UDP**(User Datagram Protocol)  
: 연결 설정 없이 데이터를 일방적으로 빠르게 전달하기 위한 프로토콜 (비연결 지향형)  
- 비신뢰성, 비연결형, 빠른 속도  
- ex) 실시간 스트리밍  
- **데이터그램 패킷 교환 방식** : 패킷이 독립적으로 이동하며 최적의 경로를 선택하여 전송하는 방식  
  1. 송신측에서 수신측으로 패킷을 전송  
  2. 패킷이 3 - 2 - 1 순서로 송신되지만 인접 노드들의 트래픽(전송량) 상황을 고려하여  
     각각 최적의 경로를 설정  
  3. 패킷이 따로 이동하며 순서가 다르게 도착  
  <img width="486" alt="datagram_packet" src="https://github.com/user-attachments/assets/0fccc378-ea5c-4019-aef0-71c34c5d89d4" />

<br>

**TCP 연결 성립 과정**  
- TCP는 신뢰성을 확보하기 위해 **3-웨이 핸드셰이크**(3-way Handshake) 작업을 진행하여 연결 성립 시킴  
- IP 패킷에서 IP 헤더가 분리되는 역캡슐화가 이뤄지며 **TCP세그먼트**가 만들어짐  
- TCP 헤더의 플래그들 중 **ACK**와 **SYN** 플래그를 사용하는데 이런 플래그들을 컨트롤 비트라고 지칭  
- **일련 번호**는 데이터의 순서를 지칭  
- **확인 응답 번호**는 수신한 데이터의 (일련 번호 + 1) 값  
<img width="486" alt="datagram_packet" src="https://github.com/user-attachments/assets/51e93f3e-5750-4f11-93b5-82aca07a8f34" />
<br>
<img width="313" alt="스크린샷 2026-02-21 16 25 06" src="https://github.com/user-attachments/assets/3c7185a7-b4cd-47b5-8dbd-3ab393066abc" /> <img width="339" alt="스크린샷 2026-02-21 16 25 22" src="https://github.com/user-attachments/assets/1bd07cd4-4282-4bc8-879b-bb78ed72fe12" />
<br>
출처 : [TCP 세그먼트 TCP 헤더/TCP Segment TCP Header](https://www.youtube.com/watch?v=H2ogjDL9E-o)  

<br>
<br>

**3-웨이 핸드셰이크**(3-way Handshake) 과정  
<img width="862" height="312" alt="img1 daumcdn" src="https://github.com/user-attachments/assets/ac22d8c8-325e-4d60-ab44-13cdc13bf74f" />

1. 클라이언트는 서버에 클라이언트의 ISN을 담아 **SYN**(SYNchronization, 요청 플래그)을 전송  
   : **SYN** (연결 요청)  
   ※ ISN(Initial Sequence Number) : 첫 패킷에 할당된 임의의 시퀸스 번호  
2. 서버는 SYN을 수신하고 서버의 ISN을 보내며, 일련 번호로 클라이언트의 (ISN + 1) 를 전송  
   : **SYN + ACK** (연결 요청 수락)  
3. 클라이언트는 서버의 일련 번호(ISN + 1) 값을 담아 **ACK**(ACKnowledgement, 응답 플래그)를 서버에 전송 : **ACK** (확인)  

<br>

**TCP 연결 해제 과정**  
- TCP가 연결을 해제할 때는 **4-웨이 핸드셰이크**(4-Way Handshake) 과정이 발생  

**4-웨이 핸드셰이크**(4-Way Handshake) 과정  
<img width="862" height="294" alt="img1 daumcdn 2" src="https://github.com/user-attachments/assets/1a455566-2199-4d3f-8dee-d21713f826e3" />

1. 클라이언트가 연결을 종료하려 할 때 **FIN 플래그**를 서버 측에 송신하고 응답을 기다린다. (FIN_WAIT1)  
   : **FIN** (연결 해제 요청)  
2. 서버는 클라이언트의 FIN 플래그 응답을 받으면 종료 대기(CLOSE_WAIT) 상태로 전환 후 **ACK 플래그**를 전송 : **ACK** (해제 요청 응답)  
   ※ 만일 서버에서 클라이언트로 보낼 데이터가 남아 있을 경우, 나머지를 모두 전송시킴  
3. ACK를 받은 클라이언트는 종료 대기(FIN_WAIT2) 상태로 변환되며, 일정 시간 이후에 서버는 클라이언트에게 **FIN 플래그**를 전송 : **FIN** (서버 측 연결 종료 요청)  
4. 클라이언트는 TIME_WAIT 상태가 되고, 다시 서버로 ACK 플래그를 보내며, ACK 플래그를 수신한  
   서버는 CLOSED 상태로 연결 종료 : **ACK** (서버 측 연결 종료 요청 확인 + 서버 연결 종료)  
   ※ 이후 클라이언트는 일정 시간을 대기 후 연결 종료.  
   ※ TIME_WAIT : 소켓이 바로 소멸되지 않고 일정 시간 유지되며 지연 패킷에 대비  
   - 만약 패킷(데이터)이 FIN 플래그 보다 늦게 도달하여 처리하지 못한다면 클라이언트에서 데이터의  
     일부가 누락되는 데이터 무결성 문제가 발생  
   <img width="508"" alt="스크린샷 2026-02-21 18 07 31" src="https://github.com/user-attachments/assets/45bf01b8-f3d8-490e-9612-17b97760a387" />

<br>

2. `인터넷 계층` (Internet Layer)  
> 전달 받은 네트워크 패킷을 IP 주소로 지정된 목적지로 전송하기 위해 사용되는 계층  
- **IP**(Internet Protocol), **ARP**(Address Resolution Protocol), **ICMP**(Internet Control Message Protocol) 등이 있으며 패킷을 수신해야 할 상대의 주소를 지정하여 데이터를 전달  
- **비연결적인 특징**을 가져 데이터가 랜덤한 순서로 도착할 수 있고, 도착을 보장하지 않음  
- 해당 계층의 헤더에는 송신 측 IP주소, 수신 측 IP 주소가 저장됨  

<br>

3. `링크 계층` (Link Layer)  
> 전선, 광섬유, 무선 등으로 실질적인 데이터를 전달하며 장치 간에 신호를 주고받는 계층  

<br>

**계층 간 데이터 송수신 과정**  
<img width="349" alt="image 3" src="https://github.com/user-attachments/assets/ee40f321-fe98-4b4a-9df4-7cf58f7a250d" />

- 계층 간 데이터는 **클라이언트**와 **서버** 사이에서 요청(Request) 값에 해더를 추가하고, 상위 계층의 데이터에 헤더를 삽입하는 **캡슐화**를 통해 전달되고, 전달 받은 캡슐화 된 데이터를 받기 위해 링크 계층 부터 상위 계층으로 올라오며 **비캡슐화**로 헤더를 벗겨내는 작업을 통해 최종적으로 데이터를 전달받는다.  

<br>

### PDU(Protocol Data Unit)  
> 네트워크의 특정 계층에서 다른 계층으로 데이터가 전달될 때의 단위  

- PDU는 일반적으로 제어 관련 정보들이 포함된 ‘**헤더**’, 데이터를 의미하는 ‘**페이로드**’로 구성  
  (계층마다 명칭이 조금씩 다름)  
  - 애플리케이션 계층 : 메시지  
  - 전송 계층 : 세그먼트(TCP), 데이터그램(UDP)  
  - 인터넷 계층 : 패킷  
  - 링크 계층 : 프레임(데이터 링크 계층), 비트(물리 계층)  
<img width="532" alt="스크린샷 2026-02-21 07 39 09" src="https://github.com/user-attachments/assets/a3809462-57b5-4b55-b7ce-7dc947e10e23" />
<img width="532" alt="스크린샷 2026-02-21 07 39 28" src="https://github.com/user-attachments/assets/cf7c756a-77db-415a-a055-df271bdf2af1" />
