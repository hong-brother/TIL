# 싱글톤
## 싱글톤 패턴 
클래스의 인스턴스가 딱 1개만 생성되는 것을 보장하는 디자인 패턴이다. 그래서 객체 인스턴스를 2개 이상 생성하지 못하도록 막아야 한다. 
메모리 낭비가 심하다. 싱글톤 패턴 
private 생성자를 사용해서 외부에서 임의로 new 키워드를 사용하지 못하도록 막아야 한다. 


## 싱글톤 패턴 문제점 
싱글톤 패턴을 구현하는 코드 자체가 많이 들어간다. 의존관계상 클라이언트가 구체 클래스에 의존한다. DIP를 위반한다. 클라이언트가 구체 클래스에 의존해서 OCP 원칙을 위반할 가능성이 높다. 테스트하기 어렵다. 내부 속성을 변경하거나 초기화 하기 어렵다. private 생성자로 자식 클래스를 만들기 어렵다. 결론적으로 유연성이 떨어진다. 안티패턴으로 불리기도 한다. 


## 싱글톤 컨테이너 (스프링 컨테이너)
스프링 컨테이너는 싱글턴 패턴을 적용하지 않아도, 객체 인스턴스를 싱글톤으로 관리한다. 이전에 설명한 컨테이너 생성 과정을 자세히 보자. 컨테이너는 객체를 하나만 생성해서 관리한다. 
스프링 컨테이너는 싱글톤 컨테이너 역할을 한다. 이렇게 싱글톤 객체를 생성하고 관리하는 기능을 싱글톤 레지스트리라 한다. 스프링 컨테이너의 이런 기능 덕분에 싱글턴 패턴의 모든 단점을 해결하면서 객체를 싱글톤으로 유지할 수 
있다. 싱글톤 패턴을 위한 지저분한 코드가 들어가지 않아도 된다. DIP, OCP, 테스트, private 생성자로 부터 자유롭게 싱글톤을 사용할 수 있다. 

- 싱글톤 등록방법
```
  ApplicationContext ac = new
  AnnotationConfigApplicationContext(AppConfig.class);
```


## 싱글톤 방식의 주의점
>
싱글톤 패턴이든, 스프링 같은 싱글톤 컨테이너를 사용하든, 객체 인스턴스를 하나만 생성해서 공유하는 싱글톤 방식은 여러 클라이언트가 하나의 같은 객체 인스턴스를 공유하기 때문에 싱글톤 객체는 상태를 유지(stateful)하게 설계하면 안된다.

- 무상태(stateless)로 설계해야 한다!
특정 클라이언트에 의존적인 필드가 있으면 안된다.
특정 클라이언트가 값을 변경할 수 있는 필드가 있으면 안된다!
가급적 읽기만 가능해야 한다.
필드 대신에 자바에서 공유되지 않는, 지역변수, 파라미터, ThreadLocal 등을 사용해야 한다.
스프링 빈의 필드에 공유 값을 설정하면 정말 큰 장애가 발생할 수 있다!!!

## @Configuration과 바이트코드 조작의 마법
>
스프링 컨테이너는 싱글톤 레지스트리다. 따라서 스프링 빈이 싱글톤이 되도록 보장해주어야 한다. 그런데 스프링이 자바 코드까지 어떻게 하기는 어렵다. 저 자바 코드를 보면 분명 3번 호출되어야 하는 것이 맞다. 그래서 스프링은 클래스의 바이트코드를 조작하는 라이브러리를 사용한다.
모든 비밀은 @Configuration 을 적용한 AppConfig 에 있다.

# 컴포넌트 스캔
- 지금까지 스프링 빈을 등록할 때는 자바 코드의 @Bean이나 XML의 <bean> 등을 통해서 설정 정보에 직접 등록할 스프링 빈을 나열했다.
- 예제에서는 몇개가 안되었지만, 이렇게 등록해야 할 스프링 빈이 수십, 수백개가 되면 일일이 등록하기도 귀찮고, 설정 정보도 커지고, 누락하는 문제도 발생한다. 역시 개발자는 반복을 싫어한다.(무엇보다 귀찮다 ᅲᅲ)
- 그래서 스프링은 설정 정보가 없어도 자동으로 스프링 빈을 등록하는 컴포넌트 스캔이라는 기능을 제공한다.
- 또 의존관계도 자동으로 주입하는 @Autowired 라는 기능도 제공한다.
  
>
  컴포넌트 스캔은 이름 그대로 @Component 애노테이션이 붙은 클래스를 스캔해서 스프링 빈으로 등록한다. @Component 를 붙여주자.
  
> 참고: @Configuration 이 컴포넌트 스캔의 대상이 된 이유도 @Configuration 소스코드를 열어보면 @Component 애노테이션이 붙어있기 때문이다.
  
## ComponentScan
- @ComponentScan 은 @Component 가 붙은 모든 클래스를 스프링 빈으로 등록한다. 
- 이때 스프링 빈의 기본 이름은 클래스명을 사용하되 맨 앞글자만 소문자를 사용한다. 
- 빈 이름 기본 전략: MemberServiceImpl 클래스 memberServiceImpl
- 빈 이름 직접 지정: 만약 스프링 빈의 이름을 직접 지정하고 싶으면 @Component("memberService2") 이런식으로 이름을 부여하면 된다.

### 생성자에 @Autowired 를 지정하면,
- 스프링 컨테이너가 자동으로 해당 스프링 빈을 찾아서 주입한다. 
- 이때 기본 조회 전략은 타입이 같은 빈을 찾아서 주입한다.
getBean(MemberRepository.class) 와 동일하다고 이해하면 된다. 더 자세한 내용은 뒤에서 설명한다.

>
  참고로 스프링 부트를 사용하면 스프링 부트의 대표 시작 정보인 @SpringBootApplication 를 이 프로젝트 시작 루트 위치에 두는 것이 관례이다. (그리고 이 설정안에 바로 @ComponentScan 이 들어있다!)

  
### 컴포넌트 스캔 기본 대상
**컴포넌트 스캔은 @Component 뿐만 아니라 다음과 내용도 추가로 대상에 포함한다. **
  - @Component : 컴포넌트 스캔에서 사용
  - @Controlller : 스프링 MVC 컨트롤러에서 사용(스프링 MVC 컨트롤러로 인식)
  - @Service : 스프링 비즈니스 로직에서 사용(@Service 는 특별한 처리를 하지 않는다. 대신 개발자들이 핵심 비즈니스 로직이 여기에
있겠구나 라고 비즈니스 계층을 인식하는데 도움이 된다.)
  - @Repository : 스프링 데이터 접근 계층에서 사용(스프링 데이터 접근 계층으로 인식하고, 데이터 계층의 예외를 스프링 예외로 변환해준다.)
  - @Configuration : 스프링 설정 정보에서 사용(앞서 보았듯이 스프링 설정 정보로 인식하고, 스프링 빈이 싱글톤을 유지하도록 추가
처리를 한다.)
  
> 해당 애노테이션 위에는 @Component가 붙어 있다.
  
### 수동 빈 등록 vs 자동 빈 등록
- 만약 수동 빈 등록과 자동 빈 등록에서 빈 이름이 충돌되면 어떻게 될까?
(수동 빈이 자동 빈을 오버라이딩 해버린다.)
  
### 다양한 의존관계 주입
- 생성자 주입
- 수정자 주입(setter 주입)
- 필드 주입
- 일반 메서드 주입
  
### Autowired옵션 처리
자동 주입 대상을 옵션으로 처리하는 방법은 다음과 같다.
- @Autowired(required=false) : 자동 주입할 대상이 없으면 수정자 메서드 자체가 호출 안됨 
- org.springframework.lang.@Nullable : 자동 주입할 대상이 없으면 null이 입력된다.
- Optional<> : 자동 주입할 대상이 없으면 Optional.empty 가 입력된다.
  
### @Autowired 필드 명, @Qualifier, @Primary
- 조회 대상 빈이 2개 이상일 때 해결 방법
> 
@Autowired 필드 명 매칭
@Qualifier @Qualifier끼리 매칭 빈 이름 매칭 @Primary 사용


#### @Qualifier 사용
- @Qualifier 는 추가 구분자를 붙여주는 방법이다. 주입시 추가적인 방법을 제공하는 것이지 빈 이름을 변경하는 것은 아니다.
  
```java  
  @Component
  @Qualifier("mainDiscountPolicy")
  public class RateDiscountPolicy implements DiscountPolicy {}

```
- @Qualifier 정리
1. @Qualifier끼리 매칭
2. 빈 이름 매칭
3. NoSuchBeanDefinitionException 예외 발생
  
  
#### @Primary 사용
- @Primary 는 우선순위를 정하는 방법이다. @Autowired 시에 여러 빈이 매칭되면 @Primary 가 우선권을 가진다.
  
```
  
  @Component
  @Primary
  public class RateDiscountPolicy implements DiscountPolicy {}
  @Component
  public class FixDiscountPolicy implements DiscountPolicy {}
```
  
#### @Primary, @Qualifier 활용
코드에서 자주 사용하는 메인 데이터베이스의 커넥션을 획득하는 스프링 빈이 있고, 코드에서 특별한 기능으로 가끔 사용하는 서브 데이터베이스의 커넥션을 획득하는 스프링 빈이 있다고 생각해보자. 메인 데이터베이스의 커넥션을 획득하는 스프링 빈은 @Primary 를 적용해서 조회하는 곳에서 @Qualifier 지정 없이 편리하게 조회하고, 서브 데이터베이스 커넥션 빈을 획득할 때는 @Qualifier 를 지정해서 명시적으로 획득 하는 방식으로 사용하면 코드를 깔끔하게 유지할 수 있다. 물론 이때 메인 데이터베이스의 스프링 빈을 등록할 때 @Qualifier 를 지정해주는 것은 상관없다.
우선순위
@Primary 는 기본값 처럼 동작하는 것이고, @Qualifier 는 매우 상세하게 동작한다. 이런 경우 어떤 것이 우선권을 가져갈까? 스프링은 자동보다는 수동이, 넒은 범위의 선택권 보다는 좁은 범위의 선택권이 우선 순위가 높다. 따라서 여기서도 @Qualifier 가 우선권이 높다.
  
  
#### 자동, 수동의 올바른 실무 운영 기준
>
애플리케이션은 크게 업무 로직과 기술 지원 로직으로 나눌 수 있다.
업무 
  
- 로직 빈: 웹을 지원하는 컨트롤러, 핵심 비즈니스 로직이 있는 서비스, 데이터 계층의 로직을 처리하는 리포지토리등이 모두 업무 로직이다. 보통 비즈니스 요구사항을 개발할 때 추가되거나 변경된다.
- 업무 로직은 숫자도 매우 많고, 한번 개발해야 하면 컨트롤러, 서비스, 리포지토리 처럼 어느정도 유사한 패턴이 있다. 이런 경우 자동 기능을 적극 사용하는 것이 좋다. 보통 문제가 발생해도 어떤 곳에서 문제가 발생했는지 명확하게 파악하기 쉽다.
- 기술 지원 빈: 기술적인 문제나 공통 관심사(AOP)를 처리할 때 주로 사용된다. 데이터베이스 연결이나, 공통 로그 처리 처럼 업무 로직을 지원하기 위한 하부 기술이나 공통 기술들이다.
- 기술 지원 로직은 업무 로직과 비교해서 그 수가 매우 적고, 보통 애플리케이션 전반에 걸쳐서 광범위하게 영향을 미친다. 그리고 업무 로직은 문제가 발생했을 때 어디가 문제인지 명확하게 잘 들어나지만, 기술 지원 로직은 적용이 잘 되고 있는지 아닌지 조차 파악하기 어려운 경우가 많다. 그래서 이런 기술 지원 로직들은 가급적 수동 빈 등록을 사용해서 명확하게 들어내는 것이 좋다.

### 빈 생명주기 콜백 시작
>
객체 생성 -> 의존관계 주입
스프링 빈은 객체를 생성하고, 의존관계 주입이 다 끝난 다음에야 필요한 데이터를 사용할 수 있는 준비가 완료된다. 따라서 초기화 작업은 의존관계 주입이 모두 완료되고 난 다음에 호출해야 한다. 그런데 개발자가 의존관계 주입이 모두 완료된 시점을 어떻게 알 수 있을까?
**스프링은 의존관계 주입이 완료되면 스프링 빈에게 콜백 메서드를 통해서 초기화 시점을 알려주는 다양한 기능을 제공한다**. 또한 **스프링은 스프링 컨테이너가 종료되기 직전에 소멸 콜백**을 준다. 따라서 안전하게 종료 작업을 진행할 수 있다.
  
#### 스프링 빈의 이벤트 라이프사이클
- 스프링컨테이너생성->스프링빈생성->의존관계주입->초기화콜백 사용->소멸전콜백->스프링 종료
- 초기화 콜백: 빈이 생성되고, 빈의 의존관계 주입이 완료된 후 호출 
- 소멸전 콜백: 빈이 소멸되기 직전에 호출

```java
@Configuration
  static class LifeCycleConfig {
      @Bean(initMethod = "init", destroyMethod = "close")
      public NetworkClient networkClient() {
          NetworkClient networkClient = new NetworkClient();
          networkClient.setUrl("http://hello-spring.dev");
          return networkClient;
} }
```
  
```java
@PostConstruct
public void init() {
	System.out.println("NetworkClient.init"); connect();
	call("초기화 연결 메시지");
}
  
@PreDestroy
public void close() {
    System.out.println("NetworkClient.close");
    disConnect();
}
```
  
  
  
### 빈 스코프란?
>
지금까지 우리는 스프링 빈이 스프링 컨테이너의 시작과 함께 생성되어서 스프링 컨테이너가 종료될 때 까지 유지된다고 학습했다. 이것은 스프링 빈이 기본적으로 싱글톤 스코프로 생성되기 때문이다. 스코프는 번역 그대로 빈이 존재할 수 있는 범위를 뜻한다.
스프링은 다음과 같은 다양한 스코프를 지원한다.
  
- 싱글톤: 기본 스코프, 스프링 컨테이너의 시작과 종료까지 유지되는 가장 넓은 범위의 스코프이다. 
- 프로토타입: 스프링 컨테이너는 프로토타입 빈의 생성과 의존관계 주입까지만 관여하고 더는 관리하지 않는 매우 짧은 범위의 스코프이다.

웹 관련 스코프
- request: 웹 요청이 들어오고 나갈때 까지 유지되는 스코프이다.
- session: 웹 세션이 생성되고 종료될 때 까지 유지되는 스코프이다.
- application: 웹의 서블릿 컨텍스트와 같은 범위로 유지되는 스코프이다.
  
```java
@Scope("prototype")
@Component
public class HelloBean {}
  
```
  
### 프로토타입 스코프 - 싱글톤 빈과 함께 사용시 Provider로 문제 해결
- ObjectFactory, ObjectProvider
  
### 스코프와 프록시
```java
@Component
@Scope(value = "request", proxyMode = ScopedProxyMode.TARGET_CLASS)
public class MyLogger {

}

```
  
```java
@Controller
@RequiredArgsConstructor
public class LogDemoController {
    private final LogDemoService logDemoService;
    private final MyLogger myLogger;
    @RequestMapping("log-demo")
    @ResponseBody
    public String logDemo(HttpServletRequest request) {
        String requestURL = request.getRequestURL().toString();
        myLogger.setRequestURL(requestURL);
        myLogger.log("controller test");
        logDemoService.logic("testId");
        return "OK";
} }  
```  
-> 사실 별로 인거 같음 MyLogger클래스를 보지 않는 인상 프록시와 스코프를 썻는지 알수 없음.. 무분별하게 copy & past 할거 같음.
  
  
가짜 프록시 객체는 요청이 오면 그때 내부에서 진짜 빈을 요청하는 위임 로직이 들어있다.
가짜 프록시 객체는 내부에 진짜 myLogger를 찾는 방법을 알고 있다.
클라이언트가 myLogger.logic() 을 호출하면 사실은 가짜 프록시 객체의 메서드를 호출한 것이다. 가짜 프록시 객체는 request 스코프의 진짜 myLogger.logic() 를 호출한다.
가짜 프록시 객체는 원본 클래스를 상속 받아서 만들어졌기 때문에 이 객체를 사용하는 클라이언트 입장에서는 사실 원본인지 아닌지도 모르게, 동일하게 사용할 수 있다(다형성)
동작 정리
CGLIB라는 라이브러리로 내 클래스를 상속 받은 가짜 프록시 객체를 만들어서 주입한다.
이 가짜 프록시 객체는 실제 요청이 오면 그때 내부에서 실제 빈을 요청하는 위임 로직이 들어있다. 가짜 프록시 객체는 실제 request scope와는 관계가 없다. 그냥 가짜이고, 내부에 단순한 위임 로직만 있고, 싱글톤 처럼 동작한다.
특징 정리
프록시 객체 덕분에 클라이언트는 마치 싱글톤 빈을 사용하듯이 편리하게 request scope를 사용할 수 있다.
사실 Provider를 사용하든, 프록시를 사용하든 핵심 아이디어는 진짜 객체 조회를 꼭 필요한 시점까지 지연처리 한다는 점이다.
단지 애노테이션 설정 변경만으로 원본 객체를 프록시 객체로 대체할 수 있다. 이것이 바로 다형성과 DI 컨테이너가 가진 큰 강점이다.
꼭 웹 스코프가 아니어도 프록시는 사용할 수 있다.
주의점
마치 싱글톤을 사용하는 것 같지만 다르게 동작하기 때문에 결국 주의해서 사용해야 한다.
이런 특별한 scope는 꼭 필요한 곳에만 최소화해서 사용하자, 무분별하게 사용하면 유지보수하기 어려워진다.  
