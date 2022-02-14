## 빌더 패턴(Builder Pattern)
>
빌더 패턴이란 복합 객체의 생성 과정과 표현 방법을 분리하여 동일한 생성 절차에 서로 다른 표현 결과를 만들 수 있게 하는 패턴이다.

빌터 패턴에 대한 내용은 [여기](https://velog.io/@hong-brother/Design-Pattern-%EB%B9%8C%EB%8D%94Builder-%ED%8C%A8%ED%84%B4)를 참조

## 구현해보자
``` javascript
export class InspectionsMappingDto {
  public detectionId: number;
  public uploadIdx: number;
  public filename: string;
  public originFileName: string;
  public farmMapId: string;
  public pnu: string;

  constructor(builder) {
    this.detectionId = builder.getDetectionId();
    this.uploadIdx = builder.getUploadIdx();
    this.filename = builder.getFileName();
    this.originFileName = builder.getOriginFileName();
    this.farmMapId = builder.getFarmMapId();
    this.pnu = builder.getPnu();
  }

  static Builder = class {
    detectionId;
    uploadIdx;
    filename;
    originFileName;
    farmMapId;
    pnu;

    getDetectionId() {
      return this.detectionId;
    }

    setDetectionId(detectionId) {
      this.detectionId = detectionId;
      return this;
    }

    getUploadIdx() {
      return this.uploadIdx;
    }

    setUploadIdx(uploadIdx) {
      this.uploadIdx = uploadIdx;
      return this;
    }

    getFileName() {
      return this.filename;
    }

    setFileName(name) {
      this.filename = name;
      return this;
    }

    getOriginFileName() {
      return this.originFileName;
    }

    setOriginFileName(originFileName) {
      this.originFileName = originFileName;
      return this;
    }

    getFarmMapId() {
      return this.farmMapId;
    }

    setFarmMapId(id) {
      this.farmMapId = id;
      return this;
    }

    getPnu() {
      return this.pnu;
    }

    setPnu(pnu) {
      this.pnu = pnu;
      return this;
    }

    build() {
      return new InspectionsMappingDto(this);
    }
  };
}

```
- 매 클래스 마다 Setter, Getter, static Builder class를 매번 이렇게 빌더를 만들어 주면 코드의 가독성도 떨어지고 양도 늘어나 관리하기 쉽지 않다.
- 하지만 이미 npm에 빌더 패턴에 대해서 만들어 놓았기 때문에 아래와 같이 쉽게 구현 할 수 있다.
- [Builder-pattern](https://www.npmjs.com/package/builder-pattern)

## 빌더 패턴
- java의 lombok라이브러를 사용해 보았다면 금방 사용 이해 할 수 있을 것이다. 오히려 lombok에는 클래스 마다 @Buillder 어노테이션을 붙여줘야하는 번거러움이 있었지만 typescript에서는 Builder로 감싸서 인스턴스를 생성만 해주기만 하면 된다.

- typescript builder
```javascript
import { Builder } from 'builder-pattern';

 return Builder(StatisticsEmdDto)
   .emdKor(item.emdKor)
   .emdCd(item.emdCd)
   .count(item.count)
   .area(item.area)
   .build();
```
- java builder
```java
	User user = User.builder()
    .name("망나니 개발자") 
    .age(18) 
    .height(200) 
    .iq(1000)
    .build();
```

