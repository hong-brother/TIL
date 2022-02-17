## 빌더 패턴
- 빌더 패턴은 복잡한 객체를 **생성하는 방법**을 정의하는 클래스와 **표현하는 방법을 정의하는 클래스**를 별도로 분리하여, 서로 다른 표현이라도 이를 생성할 수 있는 **동일한 절차를 제공하는 패턴**이다.

<p align="center">
  <img src="https://images.velog.io/images/hong-brother/post/b0562c2f-2f8a-4e4b-8487-8f58c9a996d5/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-02-14%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%209.13.13.png"/>
</p>

## 장점
- 객체 생성에 필요한 파라미터의 의므를 코드단에서 명확히 알 수 있다.
- 생성에 필요한 파라미터를 추가 할때 마다 생성자를 만들지 않아도 된다.

## 단점
- 추가적인 빌더 클래스를 만들어야 하므로 코드에 양이 늘어난다.

## 구현
- 객체를 생성하는 방법과 표현하는 방법을 분리하여 구현.

----

Builder Interface
```java
package me.hsnam.builder;

import java.time.LocalDate;

public interface PhotoBuilder {

    PhotoBuilder name(String name);

    PhotoBuilder createDate(LocalDate createDate);

    PhotoBuilder updateDate(LocalDate updateDate);

    PhotoBuilder description(String description);

    PhotoBuilder photoInfo(String name, String description);

    PhotoBuilder etcInfo(String etc, int view);

    Photo build();
}
```

```java
package me.hsnam.builder;

import java.time.LocalDate;

public class DefaultPhotoBuilder implements PhotoBuilder{

    private String name;

    private LocalDate createdDate;

    private LocalDate updateDate;

    private String description;

    private String etc;

    private int view;

    @Override
    public PhotoBuilder name(String name) {
        this.name = name;
        return this;
    }

    @Override
    public PhotoBuilder createDate(LocalDate createdDate) {
        this.createdDate = createdDate;
        return this;
    }

    @Override
    public PhotoBuilder updateDate(LocalDate updateDate) {
        this.updateDate = updateDate;
        return this;
    }

    @Override
    public PhotoBuilder description(String description) {
        this.description = description;
        return this;
    }

    @Override
    public PhotoBuilder photoInfo(String name, String description) {
        this.name = name;
        this.description = description;
        return this;
    }

    @Override
    public PhotoBuilder etcInfo(String etc, int view) {
        this.etc = etc;
        this.view = view;
        return this;
    }

    @Override
    public Photo build() {
        return new Photo(name, createdDate, updateDate, description, etc, view);
    }
}
```

```java
package me.hsnam.builder;

import lombok.Getter;
import lombok.Setter;
import lombok.ToString;

import java.time.LocalDate;

@Getter
@Setter
@ToString
public class Photo {
    private String name;

    private LocalDate createdDate;

    private LocalDate updateDate;

    private String description;

    private String etc;

    private int view;

    public Photo(String name, LocalDate createdDate, LocalDate updateDate, String description, String etc, int view) {
        this.name = name;
        this.createdDate = createdDate;
        this.updateDate = updateDate;
        this.description = description;
        this.etc = etc;
        this.view = view;
    }
}
```
- Test
```java
package builder;

import me.hsnam.builder.DefaultPhotoBuilder;
import me.hsnam.builder.Photo;
import me.hsnam.builder.PhotoBuilder;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import java.time.LocalDate;

public class BuilderTest {

    @Test
    @DisplayName("Builder 패턴 테스트")
    public void builderTest() {
        PhotoBuilder photoBuilder = new DefaultPhotoBuilder();
        Photo photo = photoBuilder
                .name("test")
                .createDate(LocalDate.of(2022, 02, 14))
                .updateDate(LocalDate.of(2022, 02, 15))
                .etcInfo("info", 2)
                .build();
        System.out.println(photo);

    }
}
```
