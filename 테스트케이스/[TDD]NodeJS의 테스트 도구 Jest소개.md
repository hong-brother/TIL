# NodeJS 테스트 도구
## Jest
- 페이스북에서 만들었고 심플에 중점을 둔 테스트 프레임워크 입니다. 
- 다양하게 Babel, TypeScript, Node.js, React, Angular, Vue.js 및 Svelte에서 많이 사용됩니다.
- NestJS에서 Jest가 기본으로 내장되어 설정되어 있다. 앞으로 Jest에 대해서 알아 볼 것 입니다.
## Jest 설치
```bash
npm install -D jest
```
설치 후에는 package.json에 아래와 같은 내용을 추가해 줍니다.
```json
{
  "scripts": {
    "test": "jest"
  }
}
```

## Jest 사용법
### 파일 명명 규칙
- tests 폴더에 .js, .ts 접미사가 있는 파일을 대상으로 합니다.
- "테스트대상.test.js", "테스트대상.test.ts"로 파일 이름을 작성합니다.
- "테스트대상.spec.js", "테스트대상.spec.ts"로 파일 이름을 작성합니다.

>
우리가 nestjs를 cli로 Controller나 Service를 생성하면 자동적으로 xxx.test.js파일이 제너레이트 되어 나온다.

### Jest 문법
```javascript
import { Test, TestingModule } from '@nestjs/testing';
import { INestApplication } from '@nestjs/common';
import * as request from 'supertest';
import { AppModule } from './../src/app.module';

describe('AppController (e2e)', () => {
  let app: INestApplication;

  beforeEach(async () => {
    const moduleFixture: TestingModule = await Test.createTestingModule({
      imports: [AppModule],
    }).compile();

    app = moduleFixture.createNestApplication();
    await app.init();
  });
  
  afterEach(() => {
    // 
  });
  
  test('1 is 1', () => {
    expect(1).toBe(1);
  });

  it('/ (GET)', () => {
    return request(app.getHttpServer())
      .get('/')
      .expect(200)
      .expect('Hello World!');
  });
});

```

#### describe
- 블록을 사용하여 테스트들을 그룹화 하여 단위를 크게 분류 해준다.

#### test, it
- 해당 블록을 통하여 테스트가 실행이 된다. 
- **expect**를 동하여 테스트할 변수나 값을 설정한다.
- toBe, toEqual를 통하여 테스트에 대한 결과를 예측하여 비교한다.

#### berforeEach
- test단위가 실행되기전 실행되는 단위 이다. 해당 단위를 통하여 test에 이전이 필요한 객체를 설정하거나 값을 셋팅하는 역할을 한다.

#### afterEach
- test단위가 실행되고나서 실행되는 단위 이다. 해당 단위를 통하여 test실행 이후 값을 clear하거나 DB를 통하여 테스트를 진행 하였다면 트랜젝션을 통해 Rollback을 수행 할 수도 있다.

