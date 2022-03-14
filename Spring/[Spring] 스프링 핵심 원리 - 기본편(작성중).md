# 싱글톤 컨테이너
## 싱글톤 - 싱글톤 컨테이너
### 싱글톤 패턴이란?
- 객체의 인스턴스 하나만 가지게 되는 패턴을 의미한다.
- 시스템 런타임, 환경 셋팅에 대한 정보 등 인스턴스가 여러 개 일 때 문제가 생 길수 있는 경우 사용하는 패턴이다.
### 싱글톤 단점
- 객체지향과 맞지 않다.
  - 아무 객체나 자유롭게 접근하고 수정하고 공유할 수 있는 전역 상태를 갖는 형태는 객체지향 프로그램에서는 지양되어야 할 모델이다.
- 개방-폐쇄 원칙 위해
  - 싱글톤 인스턴스가 너무 많은 일을 하거나 많은 데이터를 공유시킬 경우 다른 클래스의 인스턴스들 간에 결합도가 높아져 "개방-폐쇄 원칙"을 위반할 가능성이 높다.
- 테스트 하기 어렵다.
- 안티패턴으로 불리기도 한다.

>
자세한 싱글톤 패턴은 여기에서 다시 확인하자 [싱글톤](https://velog.io/@hong-brother/Design-Pattern-%EC%8B%B1%EA%B8%80%ED%86%A4Singleton)

## 싱글톤 컨테이너
### 싱글톤 컨테이너(스프링 컨테이너)란?
- 주입을 이용하여 객체를 관리하는 컨테이너이다. **스프링 컨테이너** 라고도 말하며 싱글톤 패턴을 적용하지 않아도 객체 인스턴스를 싱글톤으로 관리한다.
- Bean의 **생명 주기**을 관리한다.

### 싱글톤 컨테이너(스프링 컨테이너)의 종류
- **Bean Factory**
	- 스프링 컨테이너의 최상위 인터페이스 이며, 스프링 빈을 관리하고 조회하는 역할을 담당한다.
<p align="center">
  <img src="https://images.velog.io/images/hong-brother/post/bde02a5f-a717-4d13-9da1-19814d4caab9/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-14%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2011.39.14.png"/>
</p>

- **ApplicationContext**
	- Bean Factory를 포함한 여러 인터페이스들을 상속받은 인터페이스 이므로 DI뿐만 아니라 메시지 국제화, 리스너 이벤트 관련 기능, 리소스 관련 기능, 개발-운영 환경을 분리해서 처리하고 애플리케이션 구동 시 필요한 정보들을 관리하는 기능 등을 제공한다.     
	- 스프링 컨테이너라고 하면 일반적으로 ApplicationContext를 뜻한다.
    - **메시지소스를 활용한 국제화 기능**
	- **환경변수(로컬, 개발,운영)구분해서 처리**
	- **애플리케이션 이벤트(이벤트를 발행하고 구독하는 모델을 편리하게 지원)**
	- **편리한 리소스 조회(파일, 클래스패스, 외부 등에서 리소스를 편리하게 조회)**
<p align="center">
  <img src="https://images.velog.io/images/hong-brother/post/a4d1ab25-ee2b-4f8a-979e-c52e7fca65b4/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-14%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2011.42.33.png"/>
</p>

- **ApplicationContext** 의 다양한 설정 형식 지원
<p align="center">
  <img src="https://images.velog.io/images/hong-brother/post/a3e4213c-5de9-4c5a-9e00-a9934191f49a/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-14%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2011.50.35.png"/>
</p>
	- GenericXmlApplicationContext : XML 파일을 설정 정보로 사용하는 스프링 컨테이너 구현 클래스이다. 최근에는 스프링 부트를 많이 사용하면서 XML기반의 설정은 잘 사용하지 않는다. <br>
    - AnnotationConfigApplicationContext : 자바 코드를 설정 정보로 사용하는 스프링 컨테이너 이다. 최근에 많이 사용하는 방식이다. <br>
	- GenericGroovyApplicationContext : 그루비 언어로 작성된 설정 정보를 사용하는 스프링 컨테이너이다.

- 스프링 빈 설정 메타 정보(BeanDefinition) - 참고
![](https://images.velog.io/images/hong-brother/post/ff0238f5-5357-41b5-a06d-d6a767fe3c1d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-15%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2012.07.34.png)
> 
BeanDefinition을 빈 설정 메타라고 한다. 스프링 컨테이너는 이 메타정보를 기반으로 스프링 빈을 생성한다. <br>

**BeanDefinition 정보**
BeanClassName: 생성할 빈의 클래스 명(자바 설정 처럼 팩토리 역할의 빈을 사용하면 없음)
factoryBeanName: 팩토리 역할의 빈을 사용할 경우 이름
factoryMethodName: 빈을 생성할 팩토리 메서드 지정
Scope: 싱글톤(기본값)
lazyInit: 스프링 컨테이너를 생성할 때 빈을 생성하는 것이 아니라, 실제 빈을 사용할 때 까지 최대한 생성을 지연처리 하는지 여부
InitMethodName: 빈을 생성하고, 의존관계를 적용한 뒤에 호출되는 초기화 메서드 명 DestroyMethodName: 빈의 생명주기가 끝나서 제거하기 직전에 호출되는 메서드 명 
Constructor arguments, Properties: 의존관계 주입에서 사용한다. (자바 설정 처럼 팩토리 역할의 빈을 사용하면 없음)

- 싱글톤, 싱글톤 컨테이너(스프링 컨테이너) 주의점
	- 객체 인스턴스를 하나만 생성해서 공유하는 싱글톤 방식은 여러 클라이언트가 하나의 같은 객체 인스턴스를 공유하기 때문에 싱글톤 객체는 상태를 유지(staeful)하게 설계하면 안된다.
	- 싱글톤 객체는 무상태(stateless)로 설계해야 한다.
    - 특정 클라이언트에 의존적인 필드가 있으면 안된다.
    - 특정 클라이언트가 값을 변경할 수 있는 필드가 있으면 안된다.
    - 가급적 읽기만 가능해야 한다.
    - 필드 대신에 자바에서 공유되지 않는, 지역변수, 파라미터, ThreadLocal등을 사용해야 한다.


### 싱글톤 컨테이너(스프링 컨테이너) 생명주기
- Bean
	- 객체 생성 - 의존관계 주입
- 스프링 컨테이너
	- 스프 컨테이너 생성 - 스프링 빈 생성 - 의존관계 주입 - 초기화 콜백 - 사용 - 소멸전 콜백 - 스프링 종료
    

# 컴포넌트 스캔(@ComponentScan)
- @ComponentScan은 @Component가 붙은 모든 클래스를 스프링 빈으로 등록한다.
- 이때 스프링 빈의 기본 이름은 클래스명을 사용하되 맨 앞글자만 소문자를 사용한다.
	- 빈 이름 기본 전략: MemberServiceImpl -> memberServiceImpl
    - 빈 이름 직접 지정: @Component("memberService2") -> memberService2

## ComponentScan의 범위
```
@ComponentScan(
          basePackages = "hello.core",
}
```

- basePackages: 탐색할 패키지의 시작 위치를 지정한다. 해당 패키지를 포함한 하위 패키지를 모두 탐색한다.
- basePackageClasses: 지정한 클래스의 패키지를 탐색 시작 위치로 지정한다.

>
Spring boot를 사용하면 스프링 부트의 대표 시작 지점인 @SpringBootApplication를 이 프로젝트 시작 루트 위치에 두는 것이 관례이며 @SpringBootApplication안에 @ComponentScan이 들어 있다.

![](https://images.velog.io/images/hong-brother/post/635d4c4e-dd4e-429c-8bd8-3ea9bbaf07f0/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-15%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2012.43.35.png)

## ComponentScan의 대상
컴포넌트 스캔은 @Component 뿐만 아니라 다음과 내용도 추가로 대상에 포함한다. 
- @Component : 컴포넌트 스캔에서 사용
- @Controlller : 스프링 MVC 컨트롤러에서 사용(스프링 MVC 컨트롤러로 인식)
- @Service : 스프링 비즈니스 로직에서 사용(핵심 비즈니스 계층을 인식하기 위한)
- @Repository : 스프링 데이터 접근 계층에서 사용(스프링 데이터 접근 계층으로 인식, 데이터 계층의 예외를 스프링 예외로 변환)
- @Configuration : 스프링 설정 정보에서 사용(스프링 설정 정보로 인식, 스프링 빈이 싱글톤을 유지하도록 추가 처리)

## 중복 빈 등록과 충돌
### 자동 빈 등록 VS 자동 자동 빈 등록
- ComponentScan에 의해 자동으로 스프링 빈이 등록되는데 그 이름이 같은 경우 스프링은 오류를 발생시킨다.(ConflictingBeanDefinitionException)

### 수동 빈 등록 VS 자동 빈 등록
- 자동 빈 등록
```java
@Component
public class MemoryMemberRepository implements MemberRepository {}
```

- 수동 빈 등록
```java
ComponentScan(excludeFilters = @Filter(type = FilterType.ANNOTATION, classes =Configuration.class))
public class AutoAppConfig {
	@Bean(name = "memoryMemberRepository")
	public MemberRepository memberRepository() {
		return new MemoryMemberRepository();
	}
}
```

수동 빈 등록과 자동 빈 등록의 충돌에서는 수동 빈이 우선권을 가진다(수동 빈이 자동 빈을 오버라이딩 한다.)
```
   Overriding bean definition for bean 'memoryMemberRepository' with a different
  definition: replacing
```
개발자가 의도적으로 설정해서 이런 결과가 만들어지기 보다는 여러 설정들이 꼬여서 이런 결과가 만들어지는 경우가 더 많기 때문에 **이러한 버그는 해결하기 어려운 애매한 버그이다.**
그래서 최근 **스프링 부트** 에서는 수동 빈 등록과 자동 빈 등록이 충돌이 나면 오류가 발생하도록 기본 값을 변경하였다.

## 자동, 수동의 올바른 실무 운영 기준
### 자동 기능을 기본으로 사용
>
 스프링은 @Component 뿐만 아니라 @Controller , @Service , @Repository 처럼 계층에 맞추어 일반적인 애플리케이션 로직을 자동으로 스캔할 수 있도록 지원한다. 거기에 더해서 최근 스프링 부트는 컴포넌트 스캔을 기본으로 사용하고, 스프링 부트의 다양한 스프링 빈들도 조건이 맞으면 자동으로 등록하도록 설계했다.

### 수동 빈 등록의 기준
- 업무 로직 빈: 웹을 지원하는 컨트롤러, 핵심 비즈니스 로직이 있는 서비스, 데이터 계층의 로직을 처리하는 리포지토리등이 모두 업무 로직이다. 보통 비즈니스 요구사항을 개발할 때 추가되거나 변경된다.
- 기술 지원 빈: 기술적인 문제나 공통 관심사(AOP)를 처리할 때 주로 사용된다. 데이터베이스 연결이나, 공통 로그 처리 처럼 업무 로직을 지원하기 위한 하부 기술이나 공통 기술들이다.


# 의존관계 자동 주입
## Field Injection 
```java
@Controller
public class HomeController {
    @Autowired
    private HomeService homeService;
}
```
가장 간단한 방식의 선언 방법 이지만. **의존 관계가 눈에 잘 보이지 않아 추상적이고, 이로 인해 의존성 관계가 과도하게 복잡해질 수 있다.**(@Autowired 선언 아래 3개든 10개든 ... 많이 추가 할 수록 의존 관계가 눈에 눈에 잘 보이지 않음)
또한 단일 책입 원칙에 반하는 안티패턴이다.
외부에서 변경이 불가능해서 테스트하기 힘든 단점이 있다.
스프링 설정을 목적으로하는 @Configuration 같은 곳에서만 특별한 용도로만 사용하자.


## Setter Injection
```java
@Controller
public class HomeController {
    private HomeService homeService;
    
    @Autowired
    public void setHomeService(HomeService homeService) {
    	this.homeService = homeService;
    }
}
```
Setter Injection을 통해서 homeService를 주입하지 않아도 HomeController 객체가 생성이 가능하며 주입하지 않고 호출시 NullPointException이 발생한다.
**즉 주입이 필요한 객체가 주입을 받지 않고 객체 생성이 된다는 문제가 발생한다.**

## Constructor Injection
- 일반적인 생성자 주입
```java
@Controller
public class HomeController {
    private final HomeService homeService;
    
    @Autowired
    public HomeController(HomeService homeService) {
    	this.homeService = homeService;
    }
}
```
- lombok를 이용한 생성자 주입
```java
@Controller
@RequiredArgsConstructor
public class HomeController {
    private final HomeService homeService;
}
```
Spring Framework Reference에서 권장하는 방법이다.
생성자 주입 방식은 필수적으로 사용해야하는 의존성 없이는 Instance를 만들지 못하도록 강제 할 수 있기 때문이다.
**생성자가 딱 1개만 있으면 @Autowired를 생략해도 자동 주입 된다.**

> 침고
@Autowired의 기본 동작은 주입할 대상이 없으면 오류를 발생, 주입할 대상이 없어도 동작하게 하려면 @Autowired(required = false)로 지정

- 레거시 코드에서는 수정자 주입과 필드 주입을 많이 사용했지만 최근 스프링을 포함한 DI프레임워크 대부분(Nest.JS) 포함 생성자 주입을 아래와 같은 이유로 권장한다.
	- 대부분의 의존관계 주입은 한번 일어나면 애플리케이션 종료시점까지 의존관계를 변경할 일이 없다. 오히려 대부분의 의존관계는 애플리케이션 종료 전까지 변하면 안된다.(불변해야 한다.)
	- 수정자 주입을 사용하면, setXxx 메서드를 public으로 열어두어야 한다.
	- 누군가 실수로 변경할 수 도 있고, 변경하면 안되는 메서드를 열어두는 것은 좋은 설계 방법이 아니다. 
	**- 생성자 주입은 객체를 생성할 때 딱 1번만 호출되므로 이후에 호출되는 일이 없다. 따라서 불변하게 설계할 수 있다.**
    
## @Autowired 필드명, @Qualifier, @Primary
### @Autowired 필드 명 매칭
1. 타입 매칭
2. 타입 매칭의 결과가 2개 이상일 때 필드 명, 파라미터 명으로 빈 이름 매칭

### @Qualifier
- 추가 구분자를 붙여주는 방법이다. 주입시 추가적인 방법을 제공하는 것이지 빈 이름을 변경하는 것은 아니다.
```java

  @Bean
  @Qualifier("mainDiscountPolicy")
  public DiscountPolicy discountPolicy() {
    return new ...
  }
```
- @Qualifier 끼리 매칭
- 빈 이름 매칭
- NoSuchBeanDefinitionException 예외 발생

### @Primary
- 우선순위를 정하는 방법이다. @Autuwired 시에 여러 빈이 매칭되면 @Primary가 우선권을 가진다.

### @Qualifier, @Primary 활용
>
코드에서 자주 사용하는 메인 데이터베이스의 커넥션을 획득하는 스프링 빈이 있고, 코드에서 특별한 기능으로 가끔 사용하는 서브 데이터베이스의 커넥션을 획득하는 스프링 빈이 있다고 생각해보자. 메인 데이터베이스의 커넥션을 획득하는 스프링 빈은 @Primary 를 적용해서 조회하는 곳에서 @Qualifier 지정 없이 편리하게 조회하고, 서브 데이터베이스 커넥션 빈을 획득할 때는 @Qualifier 를 지정해서 명시적으로 획득 하는 방식으로 사용하면 코드를 깔끔하게 유지할 수 있다. 물론 이때 메인 데이터베이스의 스프링 빈을 등록할 때 @Qualifier 를 지정해주는 것은 상관없다.

### 우선순위
>
@Primary 는 기본값 처럼 동작하는 것이고, @Qualifier 는 매우 상세하게 동작한다. 이런 경우 어떤 것이 우선권을 가져갈까? 스프링은 자동보다는 수동이, 넒은 범위의 선택권 보다는 좁은 범위의 선택권이 우선 순위가 높다. 따라서 여기서도 @Qualifier 가 우선권이 높다.

# 빈 생명주기 콜백
- 스프링 빈
	- 객체 생성->의존 관계 주입
- 스프링 빈의 이벤트 라이프 사이클
	- 스프링 컨테이너 생성 -> 스프링 빈 생성 -> 의존관계 주입 -> 초기화 콜백 -> 사용 -> 소멸전 콜백 -> 스프링 종료

>
참고: 객체의 생성과 초기화를 분리하자.
생성자는 필수 정보(파라미터)를 받고, 메모리를 할당해서 객체를 생성하는 책임을 가진다. 반면에 초기화는
이렇게 생성된 값들을 활용해서 외부 커넥션을 연결하는등 무거운 동작을 수행한다.
> 따라서 생성자 안에서 무거운 초기화 작업을 함께 하는 것 보다는 객체를 생성하는 부분과 초기화 하는 부분을 명확하게 나누는 것이 유지보수 관점에서 좋다. 물론 초기화 작업이 내부 값들만 약간 변경하는
정도로 단순한 경우에는 생성자에서 한번에 다 처리하는게 더 나을 수 있다.

>
참고: 싱글톤 빈들은 스프링 컨테이너가 종료될 때 싱글톤 빈들도 함께 종료되기 때문에 스프링 컨테이너가 종료되기 직전에 소멸전 콜백이 일어난다. 뒤에서 설명하겠지만 싱글톤 처럼 컨테이너의 시작과 종료까지 생존하는 빈도 있지만, 생명주기가 짧은 빈들도 있는데 이 빈들은 컨테이너와 무관하게 해당 빈이 종료되기 직전에 소멸전 콜백이 일어난다. 자세한 내용은 스코프에서 알아보겠다.

## 빈 생명주기 콜백 
### 설정 정보에 초기화 메서드, 종료 메서드 지정
- 설정 정보에 @Bean(initMethod = "init", destroyMethod = "close") 처럼 초기화, 소멸 메서드를 지정할 수 있다.

```java
@Configuration
static class LifeCycleConfig {
	@Bean(initMethod = "init", destroyMethod = "close")
	public NetworkClient networkClient() {
		NetworkClient networkClient = new NetworkClient();
        networkClient.setUrl("http://hello-spring.dev");
        return networkClient;
	} 
}
```
- 설정 정보 사용 특징
	- 메서드 이름을 자유롭게 줄 수 있다.
	- 스프링 빈이 스프링 토드에 의존하지 않는다.
	- **코드가 아니라 설정 정보를 사용하기 때문에 코드를 고칠 수 없는 외부 라이브러리에도 초기화, 종료 메서드를 적용 할 수 있다.**
    
- 종료 메서드 추론
	- @Bean의 destroyMethod 속성에는 아주 특별한 기능이 있다.
	- 라이브러리는 대부분 close , shutdown 이라는 이름의 종료 메서드를 사용한다.
	- @Bean의 destroyMethod 는 기본값이 (inferred) (추론)으로 등록되어 있다.
	- 이 추론 기능은 close , shutdown 라는 이름의 메서드를 자동으로 호출해준다. 이름 그대로 종료 메서드를 추론해서 호출해준다.
	- 따라서 직접 스프링 빈으로 등록하면 종료 메서드는 따로 적어주지 않아도 잘 동작한다.
	- 추론 기능을 사용하기 싫으면 destroyMethod="" 처럼 빈 공백을 지정하면 된다.
    
### @PostConstruct, @PreDestroy 애노테이션 지원
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
- @PostConstruct, @PreDestroy 특징
	- 최신 스프링에서 가장 권장하는 방법이다.
	- 애노테이션 하나만 붙이면 되므로 매우 편리하다.
	-  패키지를 잘 보면 javax.annotation.PostConstruct 이다. 스프링에 종속적인 기술이 아니라 JSR-250 라는 자바 표준이다. 따라서 스프링이 아닌 다른 컨테이너에서도 동작한다.
	-  컴포넌트 스캔과 잘 어울린다.
	-  유일한 단점은 외부 라이브러리에는 적용하지 못한다는 것이다. 외부 라이브러리를 초기화, 종료 해야 하면 @Bean의 기능을 사용하자.
    
