![](https://velog.velcdn.com/images/hong-brother/post/8ad59bdb-0605-4ca9-a24c-a22dee71783b/image.png)
해당 포스트에서는 NestJS에 대한 프로젝트 구조에 대해서 생각해보는 개인 고찰 포스트 입니다. **단순히 이렇게도 생각해 볼 수 있겠군아** 라고 넘겨 주셔도 좋을 것 같습니다.🙏

# 프로젝트 구조는 항상 어렵다.
- 처음 개발자로 처음 시작한 언어는 Spring 프레임워크 였습니다. Spring만 5년(~~하지만 아직도 어렵다는 ㅠㅠ😭~~)이라는 시간동안 계속 해왔기 때문에 Spring에 초첨이 맞쳐져 있는건 아마 당연한 것 일거 같습니다.
- 타 언어를 습득할때(FastAPI, NodeJS, NestJS 등등) 항상 Spring 프레임워크 기준으로 타언어를 바라보기 때문에 러닝 커브가 있었다고 생각할 수도 있을것 같습니다. 
예를 들어 NodeJS(express)를 처음 접했을때 프로젝트 구조를 설정하는 것이 개인적인 선호에 따라 틀리고 프로젝트 아키텍쳐 및 모듈 주입에 따라 다르다는 사실에 혼란 스러웠습니다. Spring 기준으로 생각하는 저로써는 항상 같은 프로젝트 구조(Controller - Service - VO or DTO - Repository) 일괄되게 생각하기에 처음 타언어 프로젝트 구조를 가져갈때 구조적인 문제로 항상 많이 고민했던거 같습니다.

# NestJS? 운명?
![](https://velog.velcdn.com/images/hong-brother/post/330b6f97-ca1e-4780-b8e5-2287e63b5175/image.png)

- 사실 작년만 해도 NodeJS를 하면서 **Spring 프레임워크에 대한 진한 향수**를 잊지 못하고 있었습니다.
- TypeScript를 실무에 도입하므로써 타입에 대한 에러를 어느정도 문제를 해결 할 수 있었고 NestJS를 실무에 도입하므로써 가장 그리웠던 어노테이션, 데코레이터를 사용하므로써 Spring을 하는 착각(?)을 주는것 같았습니다.
- NestJS에서 Spring을 따라가면서 DI, IOC등 개념을 적용 할 수 있어서 아직까지도 만족 하면서 사용하고 있습니다.


# NestJS 프로젝트 구조
- NestJS는 `Module` 기반으로 `@Module()` 데코레이터를 사용하여 각 기능별 모듈을 생성하고 Root Module에서 집합시켜 사용합니다.
![](https://velog.velcdn.com/images/hong-brother/post/b57e4513-8dc4-4c63-ac6b-acd596c3680b/image.png)

- Spring에서는 모듈이라는 개념은 없기 때문에 여기서 쪼금 신선했습니다~ 하지만 뭐 각 기능에 따른 모듈만 만들어주고 그 모듈을 root module에 연결만 시켜주면 문제는 없었습니다.

# Module에 중복코드
- NestJS에 각 서비스 기능별로 module를 만들면서 문제가 발생 되었습니다. 각 module에 필요한 기능이 같으면 각 module에 명시를 해줘야 됩니다.
![](https://velog.velcdn.com/images/hong-brother/post/a467bede-3b51-4e80-aba2-45e4f0f35264/image.png)
- a.module.ts(왼쪽), b.module.ts(오른쪽) 보시면 JwtModule이 이미 각 모듈에 중복해서 사용하고 있습니다. 사실 뭐 중복해도 사용해도 상관없지만(~~아니야~ 상관있어~!!!~~) JwtModule설졍이 변경되면 사용된 각 모듈을 찾아서 설정해줘야 합니다ㅠㅠ 
- 그래서 각 모듈에 이런 문제를 NestJS에서도 해결 하기 위해서 여러 모듈을 만들어 놓았습니다. 
(각 모듈에 대해서는 다음 포스트에서 자세희 다루겠습니다.)

## NestJS Modules
![](https://velog.velcdn.com/images/hong-brother/post/862a6a03-1306-4992-9b1a-c0aa37b97bd6/image.png)
### Shared Modules
- NestJS에서는 모듈이 싱글톤으로 관리 되기 때문에 여러 모듈 간에 쉽게 Providers의 동일한 인스턴스를 공유 할 수 있습니다.

### Module re-exporting
- import한 모듈을 다시 export하여 다른 모듈에서 사용 할 수 있게 합니다.

### Dependeny Injection
- Module class에 생성자를 사용하여 의존성을 주입한다.

### Gloval Modules
- 똑같은 모듈을 매번 import하는 수고를 덜기 위해 모듈을 전역 스코프에 설정 할 수 있습니다.
- 모듈 최상단에 `@Global()` 데코레이터를 붙이기만 하면 import 없이 해당 모듈이 Provider에 사용 할 수 있게 됩니다.

### Dynamic Modules
- 사용자가 지정 가능한 모듈을 쉽게 만들어 줍니다.


## 좀더 예시를 들어보자
- 만약 아래와 같이 Common, User라는 모듈이 있다고 하자 각 모듈에는 특정 사이트에 헬스 체크를 하는 API아 있다고 가정 해보자
>
각 서비스에 요청하는 헬스 체크는 공식문서를 보고 참고 하였습니다. [문서](https://docs.nestjs.com/recipes/terminus#healthchecks-terminus)

![](https://velog.velcdn.com/images/hong-brother/post/4cbe6f9a-bfa3-4bec-9dd3-7142e8d149d1/image.png)

- Common 모듈안에 Controller는  `GET - /common/check` 요청이 들어오면 Service안에 check함수를 요청하게 되고 `https://docs.nestjs.com`이라느 사이트에 대해서 상태 체크를 합니다.

- User 모듈안에 Controller는  `GET - /common/check` 요청이 들어오면 Service안에 check함수를 요청하게 되고 `https://docs.nestjs.com`이라느 사이트에 대해서 상태 체크를 합니다.

![](https://velog.velcdn.com/images/hong-brother/post/a1b4be0c-50aa-4f85-aaba-cded0749c51e/image.png)

- 각 모듈에서 특정 `URL`에 상태를 체크하기 위해 사용해야 하는 모듈은 `TerminusModule`, ` HttpModule` 입니다. 상태를 체크하는 기능을 사용하기 위해서는 해당 모듈을 또 각 모듈에 import해야 하므로 위에서 JwtModule 중복되는 것 처럼 각 모듈에는 Module을 중복해서 import해야 하므로 중복 코드가 발생 될 것입니다.
- NestJS에서 공유 모듈, 글로벌 모듈 등 각 여러개를 제시하고 있는것 같지만 제가 아직 모듈에 대한 지식이 부족하여 적용하지 못하는것 같습니다.

# Module를 스프링 구조로 해결해보자
- 위에서 설명한 각 모듈에 중복되는 코드들을 해결하고자 생각을 한게 그냥 스프링처럼 해보면 어떨까 인가 입니다.
- NestJS의 모듈방식이 아닌 Spring처럼 구조를 가져가면 자연스럽게 해결 되는 것처럼 느껴집니다.

![](https://velog.velcdn.com/images/hong-brother/post/f68a1b18-cb3e-4c08-9cb8-e25c8dcb6c3a/image.png)

- 스프링과 같이 `Controller`, `Service`, `Repository`, `Dto` 등등 이렇게 가져가면 굳이 모듈을 공유 하지 않아도 될까 라는 생각을 해보았습니다.
- `Controller` 에는 common, user에 대한 Controller만 모아 놓았으며 라우터만 존재합니다.
- `Service` 에서는 common, user에 대한 비지니스 코드들만 모아 작성하였습니다.

![](https://velog.velcdn.com/images/hong-brother/post/35bc2631-266c-4471-91d3-6494b5b452d0/image.png)

- 모듈을 스프링과 같이 `controller`, `service`로 분리하여 모듈을 구성하니 `TerminusModule`, ` HttpModule`대한 모듈은 `service`모둘에만 작성하면 되니 중복코드는 줄어 들었습니다.
이와 같이 `JwtModule`와 같은 예시도 이런 구조로 설정하면 중복코드는 사라질 것 같습니다.

# 정리
- NestJS를 사용하면서 각 기능에 따른 Module에 공통으로 사용되는 모듈에 대해서 중복 코드가 발생되어 Spring과 같은 MVC구조로 해결 할 수 있지 않을까 생각해보았습니다.
- 어쩌면 아직은 제가 NestJS에 대한 Module에 대한 구조를 자세히 이해하지 못한 것 같습니다.
- NestJS에서 제시해주는 Modules개념을 다시 익히면서 구조적으로 해결 할 수 있는 방법을 좀더 고안해봐야 할 것 같습니다.





