# 로그인 처리 
## 쿠키
- 쿠키는 클라이언트(브라우저) 로컬에 저장되는 Key-Value로 이루어져 있는 데이터 파일 이다.
- 쿠키는 사용자 인증이 유효한 시간을 명시 할 수 있으며, 유효 시간이 정해지면 **브라우저가 종료되어도 인증이 유지된다는 특징이 있다.**

### 쿠키의 구성요소
- 쿠키이름
- 쿠키 값
- 만료시간
- 전송할 도메인명
- 전송할 경로
- 보안연결여부
- HttpOnly 여부

### 쿠키의 사용 목적
- 세션관리(Session Management)
	- 로그인, 사용자 넥네임, 접속시간, 장바구니 등 정보를 관리한다.
- 개인화(Personalization)
	- 사용자 마다 테마의 환경 설정등 정보들을 관리한다.
- 트래킹(Tracking)
	- 사용자의 행동을 기록하고 분석하고 기록한다.

### 쿠키의 원리
- 클라이언트(브라우저)가 서버에 접속하면 서버는 요청해더에(set-cookie: key=value)를 추가하여 응답답한다.
- 클라이언트(브라우저)는 서버 요청 이후 요청헤더에 추가하여 요청하여 서버와 통신 한다.

### 쿠키의 장점
- 서버의 저장 공간을 절약 할 수 있다.

### 쿠키의 단점
- 쿠키에 대한 정보를 HTTP 요청할 때마다 헤더에 추가하여 보내야 하기 때문에 트래픽을 발생 시킬수도 있다.
- 개인정보에 민감한 정보들을 쿠키에 저장하여 사용하였을 경우 유출에 대한 보안문제가 발생 할 수 있다.
## 세션
### 세션이란?
- 세션은 클라이언트(브라우저)와 웹 서버 간 네트워크 연결이 지속 유지되고 있는 상태를 뜻하며 클라이언트(브라우저)요청에 따른 정보를 클라이언트(브라우저)에 저장하는 것이 아니라 서버에 저장하여 사용자에게 정보가 노출되지 않는다.
![](https://velog.velcdn.com/images/hong-brother/post/aeee8ead-a479-443c-9172-f6f774db0fc9/image.png)

### 세션 사용 목적
- 로그인 정보 관리

### 세션의 원리
- 클라이언트가 서버에 접속하면 서버는 세션ID를 발급한다.
- 서버는 클라이언트에 응답할때 발급한 세션ID를 쿠키(set-cookie: sessionId)에 저장하여 응답한다.
- 이후 클라이언트는 서버에 요청할때 마다 쿠키에 저장된 세션ID를 해더에 추가하여 요청하고
- 서버는 해더의 세션ID를 저정된 세션저장소에 찾아 유효한지 확인 후 요청을 처리 한다.

### 세션의 장점
- 각 클라이언트가 서버에 접속하면 세션ID(고유ID)를 부여 한다.
- 세션ID로 각 클라이언트 마다 권한에 따른 서비스를 제공 할 수 있다.
- 각 클라이언트 정보가 서버에 저장되기 때문에 쿠키보다 보안에는 안전 하다.

### 세션의 단점
- 각 클라이언트 정보가 서버에 저장되기 때문에 서버에 처리되는 부하와 저장 공간을 필요로 할 수 있다.

## 정리
|      |  Cookie |Session|
|------|---------|-------|
|저장위치| 클라이언트 | 서버 | 
|용량제한|1개의 쿠키당 4KB, 도메인당 20개| 제한사항 없음 |
|만료시점|쿠키 저장시 설정(설정 없을 시에는 브라우저 종료시 만료)|로그아웃 시점 및 브라우저 종료시|
| 보안  | 탈취와 변조가 가능 | 서버에 저장되어 있어 상대적으로 안전 | 


# 서블릿 필터
## filter 흐름
- HTTP 요청 -> WAS -> 필터 -> 서블릿 -> 컨트롤러

## filter 제한
- 로그인 사용자
	- HTTP 요청 -> WAS -> 필터 -> 서블릿 -> 컨트롤러
- 비로그인 사용자
	- HTTP 요청 -> WAS -> 필터(적절하지 않은 요청 판단, 서블릿 호출 X)

## filter 체인
- HTTP 요청 -> WAS -> 필터1 -> 필터2 -> 필터3 -> 서블릿 -> 컨트롤러

## filter 인터페이스
```java
  public interface Filter {
      public default void init(FilterConfig filterConfig) throws ServletException
  {}
      public void doFilter(ServletRequest request, ServletResponse response,
              FilterChain chain) throws IOException, ServletException;
      public default void destroy() {}
   }
```

### init()
- 필터 초기화 메서드, 서블릿 컨테이너가 생성될 때 호출된다.

### doFilter()
- 고객의 요청이 올 때 마다 해당 메서드가 호출된다. 필터의 로직을 구현하면 된다.
- chain.doFilter(request, response)부분이 가장 중요하다. **다음 filter가 있으면 filter를 호출하고, filter가 없으면 서블릿을 호출한다. 만약 이 로직을 호출하지 않으면 다음 단계로 진행되지 않은다.**

### destroy()
- 필터 종료 메서드, 서블릿 컨테이너가 종료될 때 호출된다.

## filter 등록
```java

@Configuration
public class WebConfig {
	
    @Bean
    public FilterRegistrationBean logFilter() {
    	FilterRegistrationBean<Filter> filterRegistrationBean = new FilterRegistrationBean<>();
        filterRegistrationBean.setFilter(new LogFilter());
        filterRegistrationBean.setOrder(1);
        filterRegistrationBean.addUrlPatterns("/*");
        return filterRegistrationBean;
	} 
}

```
- 스프링 부트를 사용한다면 FilterRegistrationBean을 사용해서 등록하면 된다.
- SetFilter(new LogFilter()) : 등록할 필터를 지정한다.
- setOrder(1) : 필터는 체인으로 동작한다. 따라서 순서가 필요하다. 낮을 수록 먼저 동작한다. 
- addUrlPatterns("/*") : 필터를 적용할 URL 패턴을 지정한다. 한번에 여러 패턴을 지정할 수 있다.

# 스프링 인터셉터

## 스프링 인터셉터 흐름
- HTTP 요청 -> WAS -> 필터 -> 서블릿 -> 스프링 인터셉터 -> 컨트롤러

## 스프링 인터셉터 제한
- 로그인 사용자
	- HTTP 요청 -> WAS -> 필터 -> 서블릿 -> 스프링 인터셉터 -> 컨트롤러
- 비 로그인 사용자    
	- HTTP 요청 -> WAS -> 필터 -> 서블릿 -> 스프링 인터셉터(적절하지 않은 요청이라 판단, 컨트롤러 호출 X)

## 스프링 인터셉터 체인
- HTTP 요청 -> WAS -> 필터 -> 서블릿 -> 인터셉터1 -> 인터셉터2 -> 컨트롤러

## 스프링 인터셉터 인터페이스
```java
public interface HandlerInterceptor {
    default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {}
    default void postHandle(HttpServletRequest request, HttpServletResponse response Object handler, @Nullable ModelAndView modelAndView) throws Exception {}
  	default void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable Exception ex) throws Exception {}
}
```

![](https://velog.velcdn.com/images/hong-brother/post/6ebfd561-56f0-44ce-8c9f-2986a00e9858/image.png)

### 정상 흐름
- **preHandle** : 컨트롤러 호출 전에 호출된다. (더 정확히는 핸들러 어댑터 호출 전에 호출된다.)
	- preHandle 의 응답값이 true 이면 다음으로 진행하고, false 이면 더는 진행하지 않는다. false
인 경우 나머지 인터셉터는 물론이고, 핸들러 어댑터도 호출되지 않는다. 그림에서 1번에서 끝이
나버린다.
- **postHandle** : 컨트롤러 호출 후에 호출된다. (더 정확히는 핸들러 어댑터 호출 후에 호출된다.)
- **afterCompletion** : 뷰가 렌더링 된 이후에 호출된다.


![](https://velog.velcdn.com/images/hong-brother/post/c260ed28-55aa-4b7c-9769-d90888f831b6/image.png)

### 예외 상황
- **preHandle** : 컨트롤러 호출 전에 호출된다.
- **postHandle** : 컨트롤러에서 예외가 발생하면 postHandle 은 호출되지 않는다.
- **afterCompletion** : afterCompletion 은 항상 호출된다. 이 경우 예외( ex )를 파라미터로 받아서 어떤 예외가 발생했는지 로그로 출력할 수 있다. 
- **afterCompletion은** 예외가 발생해도 호출된다. 예외가 발생하면 postHandle() 는 호출되지 않으므로 예외와 무관하게 공통 처리를 하려afterCompletion() 을 사용해야 한다.
예외가 발생하면 afterCompletion() 에 예외 정보( ex )를 포함해서 호출된다.

### 인터셉터 등록
```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
	@Override
	public void addInterceptors(InterceptorRegistry registry) {
    	registry.addInterceptor(new LogInterceptor())
        		.order(1)
                .addPathPatterns("/**")
				.excludePathPatterns("/css/**", "/*.ico", "/error");
     }
}
```
- WebMvcConfigurer 가 제공하는 addInterceptors() 를 사용해서 인터셉터를 등록할 수 있다.
- registry.addInterceptor : 인터셉터를 등록한다.
- order : 인터셉터의 호출 순서를 지정한다. 낮을 수록 먼저 호출된다.
- addPathPatterns("/**") : 인터셉터를 적용할 URL 패턴을 지정한다. 
- excludePathPatterns("/css/**", "/*.ico", "/error") : 인터셉터에서 제외할 패턴을 지정한다.


![](https://velog.velcdn.com/images/hong-brother/post/124d4261-ab74-4f7d-9768-ef8dfe2936a5/image.png)

# 예외 처리와 오류 페이지
## 서블릿 예외처리  
서블릿은 2가지 방식으로 예외처리를 지원한다
- Exception
- response.sendError(HTTP 상태 코드, 오류 메시지)

WAS(여기까지 전파) <- 필터 <- 인터셉터 <-컨트롤러(예외발생)

## 오류 페이지 작동 원리
- 예외 발생 흐름
	- WAS(여기까지 전파) <- 필터 <- 서블릿 <- 인터셉터 <- 컨트롤러(예외발생)
    
- sendError 흐름
	- WAS(sendError 호출 기록 확인) <- 필터 <- 서블릿 <- 인터셉터 <- 컨트롤러 (response.sendError())
    
- 요류 페이지 요청 흐름
	- WAS `/error-page/500` 다시 요청 -> 필터 -> 서블릿 -> 인터셉터 -> 컨트롤러(/error-page/ 500) -> View
    
- 예외 발생과 오류 페이지 요청 흐름
1. WAS(여기까지 전파) <- 필터 <- 서블릿 <- 인터셉터 <- 컨트롤러(예외발생)
2. WAS `/error-page/500` 다시 요청 -> 필터 -> 서블릿 -> 인터셉터 -> 컨트롤러(/error- page/500) -> View


