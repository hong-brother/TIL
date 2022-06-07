# nestjs 성능테스트(스트레스 테스트)
- spring, spring boot를 협업에서 사용했을때는 기본으로 스레드 풀을 200으로 설정되어 있어서 개발을 할때 어느정도 성능이 보장되어 있기 때문에 고민하지 않고 개발을 했었다.(물론 보장은 서버 성능이나 로드벨런싱 등등 다른 문제도 있지만!)
- nodejs, nestjs로 넘어오면서 sping처럼 똑같겠지? 생각하고 개발을 하는순간
![](https://velog.velcdn.com/images/hong-brother/post/2189d180-1120-43fd-a910-58f1352b3b41/image.png)

그 프로젝트는 바로 폭망이다!!!

- 처음 프로젝트에서 폭망을 격은후 nodejs나 nestjs 개발을 할때는 request/response의 lifecycle주기를 굉장히 짧게 빨리 순환 되도록 꽤 신경을 많이 쓰고 개발을 한다.
- 그럼 과연 nodejs/nestjs는 얼마나 버틸까? 과연 tomcat처럼 express를 믿고 가야 할까?

# TPS 테스트
## TPS란?
- Transaction Per Second(TPS)는 초당 트랜잭션의 개수이다.
- 즉, 1초에 시스템이 처리할 수 있는 트랜잭션의 수를 의미한다. 예를 들어 TPS가 10이 나왔다면 1초에 30개의 트랜잭션을 처리 할 수 있다는 의미이다.

>
- TPS(Transaction Per Second) : 1초에 처리 가능한 트랜잭션의 수
- TPM(Transaction Per Minute) : 1분에 처리 가능한 트랜잭션의 수
- TPH(Transaction Per Hour) : 1시간에 처리 가능한 트랜잭션의 수

## 테스트 환경
### Target 환경
- 가상환경(container)에서 테스트를 진행한다. nestjs로 프로젝트를 만들었으며 기본으로 생성되어 있는 app.controller.ts의 get을 요청하면 "Hello World!"로 response 되도록 하는코드로 테스트 하였다. 
- os : rocky 8
- cores : 16 core
- memory : 8Gib
### Test Tool
- naver에서 제공하는 오픈소스의 nGrinder를 사용할 것이다.
![](https://velog.velcdn.com/images/hong-brother/post/7774afc8-88b8-4957-96df-463b5138292f/image.png)
- 1개의 Agent를 사용하여 가상 사용자는 10으로 설정하였다.
- 1개의 Agent에서 생성할 프로세스는 2, 스레드는 5 이다.
- 테스트 시간은 1분이다.


## Case1. 1 instance
아래 그림과 같이 pm2를 cluster모드로 올렸지만 pm2에서 instacne를 1개로 설정해서 서버에 배포하였다.
![](https://velog.velcdn.com/images/hong-brother/post/6fd9b59a-fe73-4efc-b59b-efa978d8f811/image.png)

### 테스트 결과
![](https://velog.velcdn.com/images/hong-brother/post/8da068f6-ceb0-498c-a1f3-7c75c051005a/image.png)
- TPS = 2,015.6
- Peak TPS = 2,399
- Excuted Tests = 112,916
- Successful Tests = 112,916
- errors = 0

## Case2. 5 instance
다음은 pm2로 cluster모드로 instance 5개로 테스트 하였다.
![](https://velog.velcdn.com/images/hong-brother/post/4ddd9278-4d11-49a1-a588-76792bde3de1/image.png)

### 테스트 결과
![](https://velog.velcdn.com/images/hong-brother/post/6ae9231c-e896-42fa-993b-db415051ffad/image.png)
- TPS = 8,152.6
- Peak TPS = 8,702
- Excuted Tests = 456,675
- Successful Tests = 456,675
- errors = 0

## Case3. 10 instance
다음은 pm2로 cluster모드로 instance 10개로 테스트 하였다.
![](https://velog.velcdn.com/images/hong-brother/post/0ef8bab7-be05-4c97-8d61-7fcbea44488a/image.png)

### 테스트 결과
![](https://velog.velcdn.com/images/hong-brother/post/d05f462b-8386-4dca-b38d-3326cc2efeb4/image.png)
- TPS = 7,764.8
- Peak TPS = 8,718
- Excuted Tests = 434,927
- Successful Tests = 434,927
- errors = 0

## Case4. 15 instance
다음은 이서버에서 최대로 가용할 수 있는 15개로 테스트 하였다.
(참고로 core 개수만큼 instance를 기동 할 수 있다고 알고 있는데 이 테스트 서버는 16core인데 왜 15개만 instance를 사용하는지는 잘 모르겠다..ㅠ)
![](https://velog.velcdn.com/images/hong-brother/post/b5341998-4bc7-499f-bcf5-0c7ed2bbb105/image.png)

#### 테스트 결과
![](https://velog.velcdn.com/images/hong-brother/post/ed7c252b-aa0b-4c10-838d-3aa14ef04ce6/image.png)

- TPS = 7,781.6
- Peak TPS = 8,842
- Excuted Tests = 435,918
- Successful Tests = 435,918
- errors = 0


## 결과 
- TPS 수치상으로 판단한다면 instance의 개수가 많을 수록 TPS 수치가 많이 올라 간다는 것을 알 수 있다.
- 하지만 꼭 한서버에 instance가 많다고 하더라고 TPS 수차가 올라 가지는 않는다.
(이 부분에 대해서는 아마 해당 메모리, CPU 점유률로 판단 하였을때 서버에서 감당 할 수 있는 스트레스를 넘어 선걸로 판단이 된다.)
- 중요한건! nodejs나 nestjs를 사용할때 꼭 pm2로 1개의 instance가 아닌 여러개의 instance로 배포하는 것은 필 수 인것 같다.
- 또한 이 테스트는 아주 아주 ~~ 간단한 GET으로 요청하여 응답하는 부분에 테스트를 진행 하는 것이여서 극단적으로 TPS 수치가 높게 나온것 같다.
- **실무에서 기능에따라, DB 쿼리에 따라 이 TPS 수치는 천차만별일 것이다. 하지만 기능을 구현하고 테스트 케이를 수행하고 내가 만든 기능이 얼마나 수행 하고 얼마나 버틸 수 있는지 스트레스 수치를 측정하는 것도 예상치 못한 버그를 잡아 낼 수 있고 경험적인 부분에서 좋은것 같다.**

## 그래서 이게 끝?
- 이번 편은 정말 내가 만든 nodejs/nestjs가 얼마나 버티는지 단순히 궁금해서 측정해본것이다.
- 궁극적으로 가장 궁금한것은 spring과 비교했을때 어떻게 차이가 나는것이다.!
- 다음은 spring과 테스트하여 단순히 TPS수치만 비교해보겠다.





