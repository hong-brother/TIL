# 인터넷 네트워크
## IP(인터넷 프로토콜)
- 지정한 IP 주소(IP Address)에 데이터 전달
- 패킷(Packet)이라는 통신 단위로 데이터 전달

>
패킷이란. 수하물을 뜻하는 패키지와 덩어리를 뜻하는 버킷의 합성어.

## TCP, UDP

### 인터넷 프로토콜 스택의 4계층
![](https://images.velog.io/images/hong-brother/post/baad3408-7482-4d5a-8507-1d802f392d92/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-19%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%208.34.05.png)
>
OSI 7계층 참고

### 프로토콜 계층
![](https://images.velog.io/images/hong-brother/post/40f7e893-c3ad-4c8c-984b-30615e2feab7/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-19%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%208.39.04.png)



### TCP(Transmission Control Protocol, 전송 제어 프로토콜)
>
인터넷 상에서 데이터를 메세지의 형태로 보내기 위해 IP와 함께 사용하는 프로토콜

#### 특징
1. **연결지향** - TCP 3 way handshake(가상 연결)
2. 데이터 전달 보증
3. 순서 보장
4. 신뢰할 수 있는 프로토콜
5. 현재 대부분 TCP 사용

![](https://images.velog.io/images/hong-brother/post/9d756bee-7432-4296-b13d-7222e1ddc3ac/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-19%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%208.43.05.png)

### UDP(User Datagram Protocal)
>
데이터를 데이터그램 단위로 처리하는 프로토콜

### 특징
1. **비연결형** - 데이터그램 방식을 제공
2. 정보를 주고 받을때 정보를 보내거나 받는다는 신호절차를 거치지 않음
3. UDP헤더의 CheckSum 필드를 통해 최소한의 오류만 검출
4. 신뢰성이 낮음
5. TCP보다 속도가 빠름

# URI와 웹 브라우저 요청 흐름
## URI(Uniform Resource Idntifier)
>
인터넷 상에서 어떤 자원을 식별하기 위한 구성이며 URL과 URN의 상위 개념이다.

![](https://images.velog.io/images/hong-brother/post/d02584a2-e2a7-438f-8fb7-e245e2bc2978/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-19%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%209.12.38.png)

## URL(Uniform Resource Locator)
> 웹에서 접근가능한 자원의 주소르 일괄되게 표현하는 형식

![](https://images.velog.io/images/hong-brother/post/21a83476-22c7-4a6e-ac33-449c03f3ed14/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-19%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%208.55.25.png)

- 프로토콜: 웹에서 서버와 클라이언트간에 어떤 방법으로 접근할지 명시(ex. http, https...)
- 호스트: 웹서버의 위치를 지정. 도메인이나 IP주소를 사용한다.
- 포트: 웹서버에서 자원을 접근하기 위한 사용되는 관문 HTTP포트는 80번, HTTPS포트는 443번 포트를 사용하고 생략 가능하다.
- 경로: 웹서버에서 자원에 대한 접근 경로
- 매개변수: Query, Parameters라고 불리며 key와 value로 짝을 이룬다.
- 부분 식별자: fragment라고 하며 URL이 지정한 자원의 세부 부분을 지정할때 쓴다.

## URN(Uniform Resource Name)
>
인터넷상에서 어떤 자원을 식별할 때 고유한 이름만으로 특정 자원을 식별하려는 목적

- urn:isbn:8960777331
- urn : isbn : 1234567891234 (국제표준도서번호)

# HTTP
>
HTTP - HyperText Transfer Protocol

- http는 HTML, 텍스트, image, 음성, 영상, 파일 JSON, XML 등 거의 모든 형태의 데이터 전송이 가능
- 서버간에 데이터를 주고 받을 때도 대부분 HTTP를 사용

## 기반 프로토콜
- TCP: HTTP/1.1(현재 주로 사용), HTTP/2
- UDP: HTTP/3

## 특징
- 클라이언트-서버 구조(Request / Response)
- 무상태 프로토콜(stateless), 비연결성
- HTTP 메시지 (HTML, TEXT, IMAGE, ... 거의 모든 형태의 데이터 전송 가능)
![](https://images.velog.io/images/hong-brother/post/3e6e7700-d114-4647-98ff-d41fc14fc595/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-19%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%209.24.30.png)
![](https://images.velog.io/images/hong-brother/post/c9a1cbb5-f59e-468d-a717-ecebd2aab7af/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-19%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%209.25.07.png)
- 단순함과 확장 가능






"-" 는 이중적인 이미 inside-logo
