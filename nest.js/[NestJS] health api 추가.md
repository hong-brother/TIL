# check health
- 필자는 백엔드 개발시 가장 처음에 개발하는 API가 상태 체크 API이다. 
- 크게 2개가의 상태 체크 API를 만드는데 첫번째로는 service 상태를 확인하는 API, 두번째로는 DB상태를 체크하는 API 이다.
- 별거 아니긴 하지만 실무에서 되게 유용하게 사용이 된다. 
제한이 많은 여러 상황에서 ssh를 접근 하지 않아도 브라우저에서 URL만 호출하면 API 상태를 체크 할 수 있고 DB같은 경우 특별한 GUI(?) 음... pgadmin, dbeaver, toad, 등등등 DB를 접속하는 툴이 따로 있지 않은 이상 DB상태 체크하기 어렵기 때문에 API를 만들어 놓으면 브라우저에서 URL만 호출하면 상태를 체크 할 수 있다.
- 보통은 그냥 만들지만 NestJS같은 경우는 공식 Document에 설명이 되어 있기 때문에 공식 문서를 기반으로 설명한다.


## Check API Health
### install
- 먼저 @nestjs/terminus 설치한다.
```bin
npm install --save @nestjs/termius
```

### 모듈 설정 및 디렉토리 구성
- 필자는 해당 API를 공통 모듈로 분리 하므로 common.modules.ts를 생성한다.
```
- nest g module common
```
- 그리고 상태 체크 controller를 위한 파일을 생성한다.
```
- nest g controller health
```
![](https://velog.velcdn.com/images/hong-brother/post/68886206-7d29-4347-a147-0dec0a15ac47/image.png)

- 위와 같이 modules안에 common 구성후 common.modules, health.controller를 생성하였다.

### Controller 구성
```javascript
import {Controller, Get} from '@nestjs/common';
import {HealthCheck, HealthCheckService, HttpHealthIndicator} from "@nestjs/terminus";

@Controller('health')
export class HealthController {
    constructor(
        private health: HealthCheckService,
        private http: HttpHealthIndicator,
    ) {}

    @Get()
    @HealthCheck()
    check() {
        return this.health.check([
            () => this.http.pingCheck('nestjs-docs', 'https://docs.nestjs.com'),
        ]);
    }
}
```
- http.check를 사용하기 위해서는 @nestjs/axios가 사전에 설치되어 있어야한다.
```
 npm i --save @nestjs/axios
```
- http를 사용하기 위해 common.module에 httpmodule를 import되어 있어야 한다.

### common.module 구성
```javascript
import {Module} from '@nestjs/common';
import {HealthController} from "./health.controller";
import {TerminusModule} from "@nestjs/terminus";
import {HttpModule} from "@nestjs/axios";

@Module({
    imports: [
        TerminusModule,
        HttpModule.register({
            timeout: 5000,
            maxRedirects: 5,
        }),
    ],
    controllers: [
        HealthController,

    ],
})
export class CommonModule {
}
```

### API 확인
 - 사전에 설정한 swagger문서에 /health가 표시가 되며 api  요청시 아래와 같이 상태를 체크해서 보여준다.
![](https://velog.velcdn.com/images/hong-brother/post/ef908ff9-b503-4276-82b1-90ca9533a1fa/image.png)

