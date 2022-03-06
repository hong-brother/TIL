# NestJS + RabbitMQ를 활용한 효율적인 비동기 구현
>
현재 회사 내부에서 운영중인 서비스 중에 순조롭게 잘 되고 있지만 내부적으로 특정 기능을 수행하면 서버 응답 속도가 매우 느려진다는 문제가 꾸준히 발생 되었다. 이 문제를 해결하기 위해 시간 투자를 많이 했기 때문에 다음에는 잊어버리지 않기 위해서 기록하기로 했다.

## 기능 설명 
- 서론에서 말한 특정 기능은 NestJS로 만들어진 백엔드 서버에서 특정 디렉토리를 지정하면 **특정 디렉토리에 있는 파일을 읽어서 메타데이터를 검수 후 문제가 없으면 Message Queue에 Publish하고 Subscribe에서는 메세지를 받아 해당 파일을 다른 폴더로 이동**하는 기능을 말한다. 
- **Message당 처리속도는 1초 이하, 처리해야될 Message는 갯수는 280,000건** 정도 된다.
- 건당 처리 속도는 오래걸리지 않지만 처리해야될 Message갯수가 많으므로 여러 process에 분배하기 위해 RabbitMQ를 사용하였다.
- 이 기능을 **Message Queue를 이용한 파일이동 기능**이라고 설명하겠다 ㅎㅎㅎ

## 현재 문제 상황
- 내부에서 운영중인 서비스는 RabbitMQ라는 미들웨어 메세지 broker를 쓰고 있고 NestJS를 cluster모드로 12~15개 프로세서를 올려서 서비스를 사용하고 있다. 
- 하지만 **Message Queue를 이용한 파일이동 기능**을 수행하면 cluster환경에서 모든 프로세서가 consumber역할로 메세지를 subscribe하고 있어 다른 HTTP Reqeust 요청이 들어오면 **교착 상태**가 발생하여 전체 시스템 성능이 저하가 된다.
![](https://images.velog.io/images/hong-brother/post/2d28b60a-e856-4a56-bb3f-6a0796eab525/full_process.png)
- **Message Queue를 이용한 파일이동 기능**이 수행 할때 pm2의 monit를 실행하여 살펴보면 모든 process가 활발히 일하고 있는 모습을 볼 수 있다.

## 해결 방안
### 1.Redis를 이용하여 분배하기
- 내부적으로도 WebSocket 사용하고 있기때문에 redis를 사용하고 있었다.
- Redis를 pub/sub로 사용하여 cluster모드 일때 특정 Process에 Sub를 활성화하여 분산시킬까 생각을 해보았는데 Redis를 사용하게 되면 메세지를 받았을때 Redis에서 지우도록 처리해줘야 되고 에러가 발생했을때 메세지를 어떻게 처리할 것이며... 많은 것들을 처리해야 하기때문에 어렵다 판단하였다.(~~개발자에게는 항상 시간이 충분치 않았다...~~)

### 2.특정 Process에만 Subscribe하도록 설정하기
#### 어노테이션 기반의 Subscribe
- 맨 처음부터 이 생각을 안한건 아니였다. NestJS에서 RabbitMQ를 사용할때 [nestjs-rabbitmq](https://www.npmjs.com/package/@golevelup/nestjs-rabbitmq) 모듈을 사용하고 있는데 해당 모듈에서는 subscribe를 구현할때 어노테이션 기반으로 구현하도록 가이드가 나와 있고 어노테이션기반으로 구현했기때문에 특정 Process만 비활성화 하는거는 어렵다 생각했었다...ㅠㅠㅠ
```javascript
import { RabbitSubscribe } from '@golevelup/nestjs-rabbitmq';
import { Injectable } from '@nestjs/common';

@Injectable()
export class MessagingService {
  @RabbitSubscribe({
    exchange: 'exchange1',
    routingKey: 'subscribe-route',
    queue: 'subscribe-queue',
  })
  public async pubSubHandler(msg: {}) {
    console.log(`Received message: ${JSON.stringify(msg)}`);
  }
}
```

#### Process가 initialize될때 동적 Subscribe
- **그렇다면 해당 서비스가 초기화될때 환경변수에 따라 활성화 여부를 결정 하고 활성화 하면 되지 않을까? **
nestjs-rabbitmq 모듈라이브러리 깃허브 이슈에서 찾아보니 이미 나같은 사람은 있었다.!!!!
뭐 대략적으로 요약하자면 **구독을 수동으로 만들려면 amqpConnection을 만들고 호출**하기만 하면된다.
![](https://images.velog.io/images/hong-brother/post/c38b94c8-03c1-4b9b-b2e7-a70a0882449c/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-06%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%2011.40.10.png)
- 생성자에서 amqpConnection을 주입받고 subscribe를 함수에 연결해보자!
```javascript
@Injectable()
export class InspectionsService {
  
  constructor(
    private readonly amqpConnection: AmqpConnection,
    ) {
    if (process.env?.activeInspectionSubscribe === 'true') {
      this.amqpConnection
        .createSubscriber(
          async (msg) => {
            try {
              await this.subInspectionsFileHandler(msg);
              return new Nack();
            } catch (e) {
              this.logger.error(e.message);
              return new Nack();
            }
          },
          {
              exchange: 'exchange1',
              routingKey: 'subscribe-route',
              queue: 'subscribe-queue',
          },
        )
        .then(() => {
          this.logger.log('Success Subscriber~');
        });
    }
  }
```

```javascript
module.exports = {
  apps: [
    {
      name: 'TEST',
      script: './dist/main.js',
      instances: 12, 
      exec_mode: 'cluster',
      merge_logs: true, 
      autorestart: true,
      watch: false,
      env_local: {
        NODE_ENV: 'local',
        activeInspectionSubscribe: false,
      },
      env_development: {
        NODE_ENV: 'development',
        activeInspectionSubscribe: false,
      },
      env_production: {
        NODE_ENV: 'production',
        activeInspectionSubscribe: false,
      },
    },
    {
      name: 'TEST',
      script: './dist/main.js',
      instances: 3,
      exec_mode: 'cluster',
      merge_logs: true,
      autorestart: true,
      watch: false, 
      env_local: {
        NODE_ENV: 'local',
        activeInspectionSubscribe: true,
      },
      env_development: {
        NODE_ENV: 'development',
        activeInspectionSubscribe: true,
      },
      env_production: {
        NODE_ENV: 'production',
        activeInspectionSubscribe: true,
      },
    },
  ],
};
```

- AmqpConnection를 주입 받아 **생성자에서 초기화 될때 환경변수에 따라서 subscribe를 생성한다.** 생성할때 exchange, routingKey, Queue를 설정해주고 해당 메세지 처리 부분에는 메세지 처리 함수를 호출하여 기능을 수행한다.
- PM2의 APP을 2개로 구성하여 하는 Subscriber활성화 하는 APP, Subscriber를 비활성화 하는 APP으로 나누어 구성한다.

## 결과
![](https://images.velog.io/images/hong-brother/post/42b8bf35-175c-4247-adcc-c469e238efad/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-03%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%202.36.30.png)

- pm2로 Cluster모드로 실행 시킨후 기능을 수행해보면 3개의 Process만 일하는 것을 확인 할 수 있다.
- 해당 기능이 수행되고 있을때 HTTP Reqeust를 수행해보면 이전에 비해 시스템이 성능 저하가 발생 되는 것은 해결이 되었다.
- 대략 만장정도의 데이터를 가지고 테스트 해보았지만 시스템 성능 저하 없이 3개의 Process로 15분 이내 모든 작업이 완료되는 것을 확인 할 수 있었다.

## 향후 계획
- 실제 280,000건의 데이터를 가지고 기능을 테스트 했을때 어느정도 걸릴지 시간 측정이 필요하고 그 시간에 따라서 특정 Subscriber process를 늘려야할지 확인을 해 볼 계획이다.
