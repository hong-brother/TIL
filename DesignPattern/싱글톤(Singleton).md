>
"클래스가 인스턴스를 하나만 갖게 하고 전역 범위에서 이 인스턴스에 접근하는 단일 지점을 제공하기 위해 사용한다"  - Gof

## 싱글톤 패턴이란?
- 객체의 인스턴스 하나만 가지게 되는 패턴을 의미 한다.(객체 생성패턴)

## 싱글톤의 쓰임새?
- 애플리케이션 도메인 전역에서 설정 데이터등 공유 데이터에 접근한다.
- 값비싼 리소스를 한 번만 읽고 캐시하여 전역 범위에서 접근 가능한 지점을 공유하고 성능을 높인다.
- 유일한 애플리케이션 로거 인스턴스를 생성한다.(하나만 필요)
- 유일한 퍼사드 객체를 생성한다.

## 싱글톤 단점
- 객체지향과 맞지 않다.
	- 아무 객체나 자유롭게 접근하고 수정하고 공유할 수 있는 전역 상태를 갖는 형태는 객체지향 프로그램에서는 지양되어야 할 모델이다.
- 개방-폐쇄 원칙 위해
	- 싱글톤 인스턴스가 너무 많은 일을 하거나 많은 데이터를 공유시킬 경우 다른 클래스의 인스턴스들 간에 결합도가 높아져 "개방-폐쇄 원칙"을 위해하게 된다.


## 구현
- 싱글톤은 인스턴스가 하나만 있어야 하므로 객체를 생성하는 코드를 외부에 드러나지 않게 구현 하면 됩니다.
인스턴스 생성 후 외부에서 호출할때 생성된 인스턴스를 반환하는 메서드를 클래스명을 통해 접근할 수 있도록 정적 메서드로 표시합니다(AppConfig.getInstance())
또한 멀티 스레드 환경에서 싱글톤 인스턴스가 2개가 만들어질수 있기 때문에 synchronized 키워드로 잠금장치를 걸어 둡니다.
```java
package singleton;

public class AppConfig {

    private AppConfig() {}

    private static AppConfig appConfig = null;

    public static synchronized AppConfig getInstance() {
        if (appConfig == null) {
            appConfig = new AppConfig();
        }
        return appConfig;
    }

    private boolean isOpen = false;
    private int size = 1;

    public boolean isOpen() {
        return isOpen;
    }

    public void setOpen(boolean open) {
        isOpen = open;
    }

    public int getSize() {
        return size;
    }

    public void setSize(int size) {
        this.size = size;
    }
}
```
- DeskApp 클래스에서 AppConfig 인스턴스를 가져와서 값을 변경한다.
```java
package singleton;

public class DeskApp {
    private AppConfig appConfig = AppConfig.getInstance();

    public void setDeskApp() {
        appConfig.setOpen(false);
        appConfig.setSize(30);

        System.out.println("isOpen = " + appConfig.isOpen() + " size = " + appConfig.getSize());
    }
}

```

- MobileApp 클래스에서 AppConfig 인스턴스를 가져와서 값을 변경한다.
```java
package singleton;

public class MobileApp {
    private AppConfig appConfig = AppConfig.getInstance();

    public void setMobileApp() {
        appConfig.setOpen(true);
        appConfig.setSize(10);

        System.out.println("isOpen = " + appConfig.isOpen() + " size = " + appConfig.getSize());
    }
}

```

- Main클래스를 실행하여 확인 한다.
```java
package singleton;

public class Main {

    public static void main(String[] args) {
        new MobileApp().setMobileApp();
        new DeskApp().setDeskApp();
    }
}

```


- 자바 싱글톤은 자바5 버전부터 도입된 enum타입을 써서 생성하는게 최선입니다. 이펙티브 자바에서 이방법을 사용할 것을 권장합니다. enum 태생이 싱글톤이라 잡다한 생성과정은 JVM이 알아서 처리하므로 객체 생성 및 동기화, 그리고 초기화에 대한 고민은 하지않아도 됩니다.
```java
package singleton;

public enum AppConfig {
    INSTANCE;

    public static  AppConfig getInstance() {
        return INSTANCE;
    }

    private boolean isOpen = false;
    private int size = 1;

    public boolean isOpen() {
        return isOpen;
    }

    public void setOpen(boolean open) {
        isOpen = open;
    }

    public int getSize() {
        return size;
    }

    public void setSize(int size) {
        this.size = size;
    }
}
```
