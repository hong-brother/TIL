>
"클래스가 인스턴스를 하나만 갖게 하고 전역 범위에서 이 인스턴스에 접근하는 단일 지점을 제공하기 위해 사용한다"  - Gof

## 싱글톤 패턴이란?
- **객체의 인스턴스 하나만 가지게 되는 패턴을 의미 한다.**
- 시스템 런타임, 환경 세팅에 대한 정보 등, 인스턴스가 여러개 일 때 문제가 생길 수 있는 경우 사용하는 패턴이다.
<p align="center">
  <img src="https://images.velog.io/images/hong-brother/post/0838efc4-bfc1-4a1b-ad96-def748832865/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-02-10%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%209.26.41.png" width="300"/>
</p> 
## 싱글톤의 쓰임새?
- 애플리케이션 도메인 전역에서 설정 데이터등 공유 데이터에 접근한다. <br>
- 값비싼 리소스를 한 번만 읽고 캐시하여 전역 범위에서 접근 가능한 지점을 공유하고 성능을 높인다. <br>
- 유일한 애플리케이션 로거 인스턴스를 생성한다.(하나만 필요) <br>
- 유일한 퍼사드 객체를 생성한다. <br>

## 싱글톤 단점
- 객체지향과 맞지 않다.
	- 아무 객체나 자유롭게 접근하고 수정하고 공유할 수 있는 전역 상태를 갖는 형태는 객체지향 프로그램에서는 지양되어야 할 모델이다.
- 개방-폐쇄 원칙 위해
	- 싱글톤 인스턴스가 너무 많은 일을 하거나 많은 데이터를 공유시킬 경우 다른 클래스의 인스턴스들 간에 결합도가 높아져 "개방-폐쇄 원칙"을 위해하게 된다.


## 싱글톤 구현
### 1. 기본 싱글톤
- 싱글톤은 인스턴스가 하나만 있어야 하므로 객체를 생성하는 코드를 외부에 드러나지 않게 구현 하면 됩니다.
인스턴스 생성 후 외부에서 호출할때 생성된 인스턴스를 반환하는 메서드를 클래스명을 통해 접근할 수 있도록 정적 메서드로 표시한다.(AppConfig.getInstance())
하지만 이 방법은 멀티스레드 환경에서는 안전하지 않은 방법 이다.
```java
package me.hsnam.designpatterns.singleton;

public class AppConfig {

    private static AppConfig instance; // 

    private AppConfig() {} // 생성자를 private로 만들어 외부에서 인스턴스를 만들 수 없도록 한다.

    public static AppConfig getInstance() {
        if (instance == null) {
            instance = new AppConfig();
        }
        return instance;
    }

}
```
### 2. 멀티스레드 환경에서의 싱글톤
- 2개의 스레드가 있다고 2개의 스레드가 if(instance == null)문에 머물러 있다고 가정한다면 아직 인스턴스가 생성되지 않았기 때문에 new AppConfig(); 가 2번 실행 될 수도 있다.
**- synchronized 함수로 동시에 한 스레드만 들어 올 수 있도록 동기화 작업을 한다. 하지만 동기화 작업때문에 성능 이슈가 발생 할 수도 있다.**
```java
package me.hsnam.designpatterns.singleton;

public class AppConfig {

    private static AppConfig instance;

    private AppConfig() {}

    public static synchronized AppConfig getInstance() { 
        // 동시에 한 스레드만 들어 올수 있도록 동기화 한다.
        // 동기화 작업때문에 성능에 문제가 될 수 도 있다.
        if (instance == null) {
            instance = new AppConfig();
        }
        return instance;
    }
}

```

### 3. eager initialization 싱글톤 
- AppConfig 클래스가 로딩될때 미리 만들어 놓아서 멀티 스레드 환경에서도 안전하다.
```java
package me.hsnam.designpatterns.singleton;

public class AppConfig {

    private static final AppConfig INSTANCE = new AppConfig();

    private AppConfig() {}

    public static AppConfig getInstance() {
        return INSTANCE;
    }
}
```

### 4. double checked locking 싱글톤
- 인스턴스를 더블체크하여 조금더 안전하게 멀티 스레드 환경에서 인스턴스를 생성 할 수 있다.
```java
package me.hsnam.designpatterns.singleton;

public class AppConfig {

    private static AppConfig instance;

    private AppConfig() {}

    public static AppConfig getInstance() {
        if (instance == null) {
            synchronized (AppConfig.class) {
                if (instance == null) {
                    instance = new AppConfig();
                }
            }
        }
        return instance;
    }
}
```

### 5.static inner class 싱글톤
- 멀티 스레드 환경에서도 안전하고 getInstance호출할때 생성되기 때문에 lazy 하게 생성하는 방법이다.
```java
package me.hsnam.designpatterns.singleton;

public class AppConfig {

    private AppConfig() {}
    
    private static class AppConfigHolder {
        private static final AppConfig INSTANCE = new AppConfig();
    }

    public static AppConfig getInstance() {
        return AppConfigHolder.INSTANCE;
    }
}
```

### 6.Enum 싱글톤
- 이펙티브 자바에서 이방법을 사용 할 것을 권장한다. enum 태생이 싱글톤이라 잡다한 생성과정은 JVM이 알아서 처리 하므로 객체 생성 및 동기화, 그리고 초기화 고민하지 않아도 된다. 또한 리플렉션에서도 안전하다.
```java
package me.hsnam.designpatterns.singleton;

public enum AppConfig {
    INSTANCE;

    public static AppConfig getInstance() {
        return INSTANCE;
    }
}

```

## [부록] 내가 만든 싱글톤 깨버리기
- 자바의 리플렉션을 이용하여 싱글톤을 새로 생성 할 수 있다.
```java
    public static void main(String[] args) throws NoSuchMethodException, InvocationTargetException, InstantiationException, IllegalAccessException {

        AppConfig appConfig1 = AppConfig.getInstance();

        Constructor<AppConfig> configConstructor = AppConfig.class.getDeclaredConstructor();
        configConstructor.setAccessible(true);
        AppConfig appConfig2 = configConstructor.newInstance();

        System.out.println(appConfig1 == appConfig2);
    }
```
