## 빌더 패턴(Builder Pattern)
>
빌더 패턴이란 복합 객체의 생성 과정과 표현 방법을 분리하여 동일한 생성 절차에 서로 다른 표현 결과를 만들 수 있게 하는 패턴이다.

## 빌더 패턴을 구현해보자
- 매 클래스 마다 이런 짓을 해야 하다니... 오히려 가독성에서 최악이다.
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

## 빌더 패턴
```javascript
import { Builder } from 'builder-pattern';

 return Builder(StatisticsEmdDto)
   .emdKor(item.emdKor)
   .emdCd(item.emdCd)
   .count(item.count)
   .area(item.area)
   .build();
```

```java
	User user = User.builder()
    .name("망나니 개발자") 
    .age(18) 
    .height(200) 
    .iq(1000)
    .build();
```

