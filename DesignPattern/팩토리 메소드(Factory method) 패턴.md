>
"객체를 생성하는 인터페이스를 정하되 어느 클래스를 인스턴스화 할지는 하위 클래스가 결정하게 하는 패턴 - GoF"
## 팩토리 메소드(Factory method) 패턴
- 객체 생성 처리를 서브 클래스로 분리해 처리하도록하는 패턴이다. 객체의 생성 코드를 별도의 클래스/메서드로 분리함으로써 객체 생성의 변화에 유용하다.

<p align="center">
  <img src="https://images.velog.io/images/hong-brother/post/b0db1f6c-75e6-4ecb-bc76-88a70f6fe6d1/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-02-11%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2012.26.53.png"/>
</p> 


## 팩토리 메서드 장점
- 팩토리 매서드를 통해서 객체를 생성하기 때문에 또다른 형태의 객체를 생성 하더라도 Factory를 상속받아 구현해서 생성하기 때문에 코드의 수정이 없다.(SOLID 개방 폐쇄 원칙 - **기존 코드를 변경하지 않고 새로운 기능을 확장 할 수 있다.**)

## 팩토리 메서드 단점
- 패턴의 특성에 따라서 코드가 복잡해지거나 클래스가 많아 질 수 있다.

## 구현 다이어그램
<p align="center">
  <img src="https://images.velog.io/images/hong-brother/post/a38d1e61-14e6-4843-94af-bdfcbb03d065/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-02-13%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%207.38.29.png"/>
</p> 

### HouseFactory

```java
package me.hsnam.factorymethod.base;

/**
 * Product
 */
public interface HouseFactory {

    default House orderHouse(String name, String email) {
        validate(name, email);
        House house = createHouse();
        prepareFor();
        completeMessage(name);
        return house;
    }

    private void validate(String name, String email) {
        if (name == null || name.isBlank()) {
            throw new IllegalArgumentException("must input name");
        }
        if (email == null || name.isBlank()) {
            throw new IllegalArgumentException("must input email");
        }
    }

    private void prepareFor() {
        System.out.println("building house...");
    }

    House createHouse();

    private void completeMessage(String name) {
        System.out.println(name + "complete house!");
    }
}

```
### BlackHouseFactory
```java
package me.hsnam.factorymethod.base;

/**
 * ConcreteProduct
 */
public class BlackHouseFactory implements HouseFactory{
    @Override
    public House createHouse() {
        return new BlackHouse();
    }
}

```
### GreenHouseFactory
```java
package me.hsnam.factorymethod.base;

/**
 * ConcreteProduct
 */
public class GreenHouseFactory implements HouseFactory {

    @Override
    public House createHouse() {
        return new GreenHouse();
    }
}

```
### House
```java
package me.hsnam.factorymethod.base;


import lombok.Getter;
import lombok.Setter;
import lombok.ToString;

@Getter
@Setter
@ToString
public class House {

    private String name;

    private String color;

    private String address;
}

```
### GreenHouse
```java
package me.hsnam.factorymethod.base;

/**
 * Creator
 */
public class GreenHouse extends House {
    public GreenHouse() {
        setAddress("갑천면");
        setColor("green");
        setName("그린하우스");
    }
}

```
### BlackHouse
```java
package me.hsnam.factorymethod.base;

/**
 * Creator
 */
public class BlackHouse extends House{
    public BlackHouse() {
        setAddress("굴포로");
        setColor("black");
        setName("블랙하우스");
    }
}
```
### Clicnt(TestCase)
```java
package factory;

import me.hsnam.factorymethod.base.BlackHouseFactory;
import me.hsnam.factorymethod.base.GreenHouseFactory;
import me.hsnam.factorymethod.base.House;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

public class FactoryMethod {

    @Test
    @DisplayName("simple Factory pattern")
    public void clientHouseTest() {
        System.out.println("Set Order");
        House green = new GreenHouseFactory().orderHouse("green", "hsnam@gmail.com");
        House black = new BlackHouseFactory().orderHouse("black", "hsnam@gmail.com");

        System.out.println(green);
        System.out.println(black);

    }
}

```

