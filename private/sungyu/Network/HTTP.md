# HTTP  

**HTTP**(Hyper Text Transfer Protocol)  
> 웹 상에서 클라이언트와 서버 간 통신을 위한 프로토콜  

- 네트워크의 **애플리케이션 계층**(Application Layer)에서 사용되는 프로토콜  
- **HTTP/0.9 ~ /2** 는 기본적으로 TCP 전송 프로토콜을 사용  

<br>

### HTTP/1.0  

**단일 연결 - 단일 요청, 응답**  
- 커넥션(Connection) 하나에 요청(Request) 하나 당, 하나의 응답(Response)만 처리 가능  
  → 매번 새로운 연결로 성능 저하  
  → 서버 부하 비용 증가  
<img width="352" alt="스크린샷 2026-03-14 19 56 16" src="https://github.com/user-attachments/assets/23921cbb-375c-464a-9c2d-34472c735dd9" />

<br>
<br>

### HTTP/1.1  

**지속적인 연결**(Persistent Connction)  
- 지정한 timeout 동안 커넥션을 닫지 않는 방식을 표준화  
<img width="420" alt="스크린샷 2026-03-14 20 04 47" src="https://github.com/user-attachments/assets/34d927ec-0e8e-487b-84ac-e607480ebe46" />

<br>

**파이프라이닝**(Pipelining)  
- 서버의 응답 여부와 관계없이 여러 요청을 한번에 보낼 수 있는 방식  
<img width="420" alt="스크린샷 2026-03-14 20 24 00" src="https://github.com/user-attachments/assets/61330adc-71c1-4922-a491-16486094bfd8" />

<br>

- **HOL Blocking**(Head Of Line Blocking) 문제  
  - 1번 요청 응답이 길어지면 다음 요청을 짧게 처리할 수 있음에도 Blocked 처리된다.  
- **중복 헤더** 문제  
  - 헤더에 포함되는 많은 메타데이터들 중 중복되는 요소들을 요청마다 중복으로 전송한다.  
<img width="430" alt="스크린샷 2026-03-14 20 27 27" src="https://github.com/user-attachments/assets/153aeff2-a46b-43bd-8d63-0e0bf85406cd" />

<br>
<br>

### HTTP/2  
> HTTP/1.x 에서 지연 시간을 줄이고 응답 시간을 개선한 버전의 프로토콜  

**바이너리 프레이밍**(Binary Framing)  
- 텍스트로 전송되던 데이터를 바이너리화하여 처리 속도를 향상 시켰다.  

**멀티 플렉싱**(Multiplexing)  
- 여러 개의 스트림(Stream), 즉 데이터 흐름을 통해 송수신 하는 방법
<img width="482" alt="스크린샷 2026-03-14 20 41 33" src="https://github.com/user-attachments/assets/f0cadcaf-6935-44ed-9a65-4f73d3c7961d" />
<br>
<img width="482" alt="image" src="https://github.com/user-attachments/assets/a745e680-4394-47c7-9a2d-d39a105b6cfe" />

<br>

- **단일 연결** 만으로 병렬로 여러 요청을 받을 수 있고 응답을 줄 수 있다.  
  → HOL Blocking 문제 해결  

- **허프만 코딩**(Huffman coding) 방식의 압축 방식으로 처리 되는 비트 수를 줄였다.  
- 정적 테이블을 통해 이전에 보냈던 헤더 정보 **캐싱**  
  → 중복 헤더 문제 해결  

<img width="336" alt="스크린샷 2026-03-14 20 50 57" src="https://github.com/user-attachments/assets/7a36324a-3476-46d7-b5bd-8bdde2d467f9" />

<br>
<br>

### HTTPS  
> 애플리케이션 계층과 전송 계층 사이에 SSL/TLS 신뢰 계층을 추가해 통신을 암호화 한다.  

- SEO(Search Engine Optimization) 최적화  

<br>

### HTTP/3  
> QUIC 계층에서 돌아가며 UDP 기반으로 작동된다.  
