# 웹 애플리케이션 이해
## 웹 서버, 웹 애플리케이션 서버
### Web Server란?
- HTTP 기반으로 동작하여 정적 리소스(HTML, CSS, JS, IMAGE, ...등) 제공하는 소프트웨어 이다.
- 대표적으로는 NGINX, Apache가 있다.

### WAS(Web Application Server)란?
- HTTP 기반으로 동작하여 웹 서버 기능 뿐만 아니라 프로그램 코드를 실행해서 애플리케이션 로직을 수행 하도록 제공하는 소프트웨어 이다.
- 대표적으로는 Tomcat, Jetty, Undertow가 있다.

>Node.js(Express)는 WAS(Web Application Server) 인가?
Node.js는 V8엔진 기반의 JS런타임이며, WAS 기능을 구현 할 수 있습니다. Express는 미니멀 웹 프레임워크이고 NestJS는 웹 프레임워크이다.
**이 부분에 대해서 명확하게 아시는분 있으시면 설명 부탁드립니다..ㅠㅠ**

### Web Server VS WAS(Web Application Server) 차이
- 웹 서버는 정적 리소스(파일), WAS는 주로 애플리케이션 로직을 담당한다.
- 경우에 따라 웹 서버도 프로그램을 실행하는 기능을 포함하기도 해서 웹서버와 WAS의 경계가 모호하다.

### 웹 시스템 구성(WEB, WAS, DB)
![](https://media.vlpt.us/images/hong-brother/post/061acc8c-cf68-47d2-a942-c2d5ca91ee24/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-04-04%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.35.51.png)

- 구성 1(WAS, DB만으로 구성)
	- WAS만으로 정적 리소스, 애플리케이션 로직 모두 제공 가능하지만 WAS 장애시 모든 서비스가 죽어버리기 때문에 오류 화면도 노출이 불가능하다.

- 구성 2(웹서버, WAS, DB로 구성)
	- 정적 리소스는 웹서버에서 처리하고 애플리케이션 로직같은 동적인 처리가 필요하면 웹서버가 WAS에 요청을 위임하도록 한다.
	- 정적 리소스가 많아지면 웹서버를 증설, 애플리케이션 리소스가 많아 지면 WAS를 증설하도록 하여 효율적인 리소스 관리가 가능하다.

## 서블릿
### 서블릿이란?
- Servlet(Server + Applet)으로 확장 CGI 방식으로 Sun사에서 내놓은 기술이다. Java라는 언어를 기반으로 하여 동적인 컨텐츠를 생성하는 기술을 제공한다.
### 서블릿 컨테이너
![](https://media.vlpt.us/images/hong-brother/post/2c4af3ad-de30-48db-a498-4fc45b0c6402/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-04-04%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2010.27.27.png)

- Tomcat처럼 서블릿을 지원하는 WAS를 서블릿 컨테이너라고한다.
- 서블릿 컨테이너는 서블릿 생명주기(Life Cycle) 생성, 초기화, 호출, 종료하는 생명주기를 관리해준다.
- 서블릿 객체는 싱글톤으로 관리된다.
- JSP도 서블릿으로 변환되어서 사용된다.
- 동시 요청을 위한 멀티 쓰레드 처리를 지원한다.

## 동시 요청
### Thread란?
>
프로그램(프로세스) 실행의 단위 이며 하나의 프로세스는 여러개의 Thread로 구성이 가능하다.

### 단일 Thread
![](https://media.vlpt.us/images/hong-brother/post/0e5eb2ea-89d9-4f4e-b56e-56ddbe4e4696/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-04-04%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2011.06.39.png)

- 단일 Thread 구성 시 **요청2**는 **요청1**에서의 처리중인 스레드가 완료 될때까지 기다려야 한다. 결국 처리 지연으로 서비스에 문제가 발생한다.

### 요청 마다 Thread 생성
![](https://media.vlpt.us/images/hong-brother/post/636c583c-6a3e-49db-b1d7-957efd1b0490/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-04-04%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2011.05.45.png)
- 장점
	- 동시 요청을 처리 할 수 있다.
    - 리소스(CPU, 메모리)가 허용할 때 까지 처리가능하다.
    - 하나의 Thread가 지연 되어도 나머지 Thread는 정상 동작한다.
- 단점
	- Thread 생성 비용은 매우 비싸다.(사용자 요청이 올 때 마다 Thread 생성한다면 응답 속도가 느려진다.)
    - Thread는 컨텍스트 스위칭 비용이 발생한다.
    - Thread 생성에 제한이 없어 CPU, 메모리 임계점을 넘어가면 서버가 죽을 수도 있다.
    
### Thread 풀
![](https://media.vlpt.us/images/hong-brother/post/04390497-0624-4f58-af5e-01bf0383919c/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-04-04%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2011.13.53.png)

- 특징
	- 필요한 Thread를 Thread Pool에 보관하고 관리한다.
    - Thread Pool에 생성 가능한 Thread의 최대치를 관리한다.(tomcat은 최대 200개 시본 설정)
- 사용
	- Thread가 필요하면, 이미 생성되어 있는 Thread를 Thread Pool에서 꺼내서 사용한다.
    - 사용을 종료하면 Thread Pool에 해당 Thread를 반납한다.
    - 최대 Thread가 모두 사용중이어서 Thread Pool에 Thread가 없으면 기다리는 요청은 거절하거나 특정 숫자만큼 대기하도록 설정 할 수 있다.
- 장점
	- Thread가 미리 생성되어 있으므로 Thread 생성하고 종료되는 resource비용이 절약되고 응답 시간이 빠르다.
    - 생성 가능한 Thread의 최대치가 있으므로 너무 많은 요청이 들어와도 기존 요청은 안전하게 처리 할 수 있다.

### WAS의 멀티 Thread 지원
- 멀티 Thread에 대한 부분은 WAS가 처리
- 개발자가 멀티 Thread 관련 코드를 신경쓰지 않아도 된다.
- 개발자는 마치 싱글 Thread 프로그래밍을 하듯이 편리하게 소스코드를 개발하면 된다.
- 멀티 Thread 환경이므로 싱글톤 객체(서블릿, 스프링 빈)는 주의해서 사용한다.

## 자바 백엔드 웹 기술 역사









# 참고문헌
[스프링 MVC 1편 - 백엔드 웹 개발 핵심 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard)

# Github
[spring-study-mvc-1](https://github.com/hong-brother/spring-study-mvc-1)
[spring-study-mvc-2](https://github.com/hong-brother/spring-study-mvc-2)
