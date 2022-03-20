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

# URI, URN, URL
## URI(Uniform Resource Idntifier)
>
인터넷 상에서 어떤 자원을 식별하기 위한 구성이며 URL과 URN의 상위 개념이다.

![](https://images.velog.io/images/hong-brother/post/d02584a2-e2a7-438f-8fb7-e245e2bc2978/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-19%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%209.12.38.png)

## URL(Uniform Resource Locator)
> 웹에서 접근가능한 자원의 주소르 일괄되게 표현하는 형식

![](https://images.velog.io/images/hong-brother/post/a1c8d6fb-9a86-44dc-a305-8386bf9a7e98/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-20%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%209.32.09.png)

- protocol: 웹에서 서버와 클라이언트간에 어떤 방법으로 접근할지 명시(ex. http, https...)
- host: 웹서버의 위치를 지정. 도메인이나 IP주소를 사용한다.
- port: 웹서버에서 자원을 접근하기 위한 사용되는 관문 HTTP포트는 80번, HTTPS포트는 443번 포트를 사용하고 생략 가능하다.
- path: 웹서버에서 자원에 대한 접근 경로
- query: Query, Parameters라고 불리며 key와 value로 짝을 이룬다.
- fragment: fragment라고 하며 URL이 지정한 자원의 세부 부분을 지정할때 쓴다.

## URN(Uniform Resource Name)
>
인터넷상에서 어떤 자원을 식별할 때 고유한 이름만으로 특정 자원을 식별하려는 목적

- urn:isbn:8960777331
- urn : isbn : 1234567891234 (국제표준도서번호)

# 웹 브라우저 요청 흐름
![](https://images.velog.io/images/hong-brother/post/4ec9ceef-edec-445a-945d-fe7b9a211913/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-20%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%209.40.48.png)
![](https://images.velog.io/images/hong-brother/post/038d6eec-3cf5-4748-8105-0cfaa323cec7/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-20%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%209.41.16.png)

  ![](https://images.velog.io/images/hong-brother/post/72113079-e073-4743-baaf-a2b6ab66b9b9/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-20%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%209.42.40.png)
  
<p>
  <img src="https://images.velog.io/images/hong-brother/post/b8b8e4b2-a072-4410-87ed-70175d3a28ea/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-20%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%209.38.34.png" alt="1" style="height:100%;"/>
</p>

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
- 무상태 프로토콜(stateless)
	- 스케일 아웃(수평확장)에 유리
- 비연결성
- HTTP 메시지 (HTML, TEXT, IMAGE 등 거의 모든 형태의 데이터 전송 가능)
- 단순함과 확장 가능

## HTTP Request 구조
![](https://images.velog.io/images/hong-brother/post/45e8304f-a141-4445-9ed9-167cb68ca2d8/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-20%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%209.47.24.png)

### start-line
>
ex) GET /Search HTTP/1.1

- HTTP Method
해당 request의 HTTP 메서드 (GET, POST, PUT, DELETE)
- request target
해당 request가 전동되는 URL
- HTTP Version
HTTP version
### header
해당 request에 대한 추가 정보(additional information)를 담고 있는 있음.

### body
해당 request에 대함 메시지를 담고 있다.

## HTTP 메서드 종류
실무에서는 주로 GET, POST, PUT, PATCH 정도 사용한다.
### GET
- 리스트 조회
- 서버에 전달하고 싶은 데이터를 query(쿼리 파라미터, 쿼리 스트링)를 통해서 전달
- **메시지 바디를 사용해서 데이터를 전달 할 수 있지만, 지원하지 않는 곳이 많아서 권장하지 않는다.**

### POST
- **메시지 바디를 통해 서버로 요청 데이터 전달**
- 주로 전달된 데이터로 신규 리소스 등록, 프로세스 처리에 사용
- 다른 메서드로 처리하기 애매한 경우 사용
	- ex) JSON으로 조회 데이터를 넘겨야 하는데, GET 메서드를 사용하기 어려운 경우
    - **어지간히 애매하면 POST 사용~**

### PUT
- 리소스를 대체, 해당 리소스가 없으면 생성(덮어버림...)

### PATCH
- 리소스 부분 변경

### DELETE
- 리소스 삭제

### HEAD
- GET과 동일하지만 메세지 부분을 제외하고, 상태 줄과 헤더만 반환

### OPTIONS
- 대상 리소스에 대한 통신 가능 옵션(메서드)을 설멍(주로 CORS에서 사용)

### CONNECTION
- 대상 자원으로 식별되는 서버에 대한 터널을 설정

### TRACK
- 대상 리소스에 대한 경로를 따라 메시지 루프백 테스트를 수행

## HTTP Status
- 1xx(informational): 요청이 수신되어 처리중
- 2xx(Successful): 요청 정상 처리
- 3xx(Redirection): 요청을 완료하려면 추가 행동이 필요
- 4xx(Client Error): 클라이언트 오류, 잘못된 문법등으로 서버가 요청을 수행할 수 없음
- 5xx(Server Error): 서버 오류, 서버가 정상 요청을 처리하지 못함
### 1xx(informational)
거의 사용하지 않음.

### 2xx(Successful)
클라이언트의 요청을 성공적으로 처리

- 200 OK
	- 요청 성공
- 201 Created
	- 요청 성공해서 새로운 리소스가 생성됨
- 202 Accepted
	- 요청이 접수되었으나 처리가 완료되지 않았음
    - 배치 처리 같은 곳에서 사용
    - ex) 요청 접수 후 1시간 뒤에 배치 프로세스가 요청을 처리함.
- 204 NO Content
	- 서버가 요청을 성공적으로 수행했지만, 응답 페이로드 부분에 보낼 데이터가 없음
    - ex) 웹 문서 편집기에서 save 버튼
    - save 버튼의 결과로 아무 내용이 없어도 된다.

### 3xx(Redirection)
요청을 완료하기 위해 유저 에이전트의 추가 조치 필요
웹 브러우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 자동 이동
![](https://images.velog.io/images/hong-brother/post/af6d38d7-a150-45bb-b047-97c618f50129/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-20%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2010.30.14.png)
- 300 Multiple Choices 
	- 사용하지 않음
- 301 Moved Permanently
	- 영구 리다이렉션
    - 리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있음
- 302 Found
	- 일시적인 리다이렉션
    - 리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있음
    - **현실적으로 이미 많은 애플리케이션 라이브러리들이 302를 기본값으로 사용**
- 303 See Other
	- 일시적인 리다이렉션
   	- 리다이렉트시 요청 메서드가 GET으로 변경
- 304 Not Modified
	- 캐시를 목적으로 사용
    - 클라이언트에게 리소스가 수정되지 않았음을 알려준다. 따라서 클라이언트로는 로컬PC에 저정된 캐시를 재사용한다.(캐시로 리다이렉트 한다.)
- 307 Temporary Redirect 
	- 일시적인 리다이렉션
    - 리다이렉트시 요청 메서드와 본문 유지(요청 메서드를 변경하면 안된다.)
- 308 Permanent Redirect
	- 영구 리다이렉션
    - 리다이렉트시 요청 메서드와 본문 유지(처음 POST를 보내면 리다이렉트도 POST도 유지)


### 4xx(Client Error)
클라이언트의 요청에 잘못된 문법등으로 서버가 요청을 수행 할 수 없음
오류의 원인이 클라이언트에 있다.

- 400(Bad Request)
	- 요청 구문, 메시지 등등에 대한 오류
    - 클라이언트는 요청 내용을 다시 검토하고 보내야 한다.
    - ex) 요청 파라미터가 잘못 되었거나, API 스펙이 맞지 않을 때, 보내는 json형식이 맞지 않을때
    
- 401(Unauthorized)
	- 클라이언트가 해당 리소스에 대한 인증이 필요함
	- 인증(Authentication)되지 않음

- 403(Forbidden)
	- 서버가 요청을 이해 했지만 승인을 거부함
    - 주로 인증 자격 증명은 있지만, 접근 권한이 불충분한 경우
    - ex) 어드민 등급이 아닌 사용자가 로그인 했지만 어드민 리소스에 접근하는 경우
>
Authentication -  인증 
Authorization - 인가

- 404(Not Found)
	- 요청 리소스가 서버에 없음
    - 또는 클라이언트가 권한이 부족한 리소스에 접근할 때 해당 리소스를 숨기고 싶을 때

### 5xx(Server Error)
서버 문제로 오류 발생

- 500(Internal Server Error)
	- 서버 내부 문제로 오류 발생
    - 애매하면 500 오류
    
- 503(Service Unavailable)
	- 서버가 일시적인 과부하 또는 예정된 작읍으로 잠시 요청을 처리할 수 없음
	- Retry-After 헤더 필드로 얼마뒤에 복구 되는지도 보낼 수도 있음.
    
## HTTP Header - 추가 작성중
- HTTP 전송에 필요한 모든 부가정보(메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트, 서버 정보, 캐시 관리 정보)
- [표준 헤더 참조](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields)
- 필요시 Custom 헤더 추가 가능




# 참고문헌
[스프링 핵심 원리 - 기본편](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC)
