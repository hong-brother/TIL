# Goal
- Jenkins를 이용하여 NestJS를 배포한다.
- NestJS를 배포하면서 배포 속도를 개선한다.

# 전제조건
>
- Jenkins에 필요한 설정은 사전에 셋팅이 되어 있다.
- NestJS는 dockerize되어 있지 않고 PM2로 관리 되어 있다.

# 배포
Application을 만들기 위해서는 여러명의 개발자들과 협업을 하면서 한개의 프로젝트에 여려명이 투입되어 개발을 하게 된다. 
그럼 당연히 Git이나 SVN등등 사용 할 것이며 이 모든 변경 사항을 서버에 배포가 이루어져야 하는데 코드가 변경될 때마다 매번 메인 개발자 또는 배포를 담당하는 개발자가 수동으로 배포하는것은 요즘 시대에 너무나도 멍청한 짓이다.(배포만 하는것이 아니라 다른일도 해야하는데... )
그러기에 CI/CD는 요즘 세상에는 필수이다. 그렇다면 회사에서는 CI/CD를 통하여 한 프로젝트당 3개의 서버에 배포가 된다. develop에 배포되는 서버가 따로 있고 release에 배포되는 서버가 따로 있으며 운영에 배포되는 서버는 따로 있으며 운영에 배포되는 서버는 특수한 상황(?)에 따라 수동으로 배포된다.
코드가 변경되서 develop, release 브렌치가 되면 merge가 되면 WebHook을 통하여 Jenkins에서 배포 하는 과정을 정리해보겠다.


## Plan 1
![](https://velog.velcdn.com/images/hong-brother/post/bfbbf33a-3213-43a8-80b1-632255a64438/image.png)

### Jenkins flow
- 누군가에 의해서 develop 브렌치에서 merge가 되면 Github에서 WebHook을 통하여 변경 감지를 알려준다.
- Jenkins 서버는 즉시 소스코드를 Workspace에 clone을 받는다. (git clone)
- Jenkins 서버는 필요한 라이브러리를 받는다 (npm install)
- Jenkins 서버는 해당 코드를 javascript로 빌드한다(npm run build)
- Jenkins 서버는 빌드된 결과물을 scp를 통하여 해당서버에 파일을 보낸다.
- 배포서버는 pm2 reload하여 인스턴스를 재시작한다.

### 장점
- 해당 방법은 npm install, npm run build등 과정에서 문제가 생겨도 배포서버에 문제가 되지 않는다. 
- 딱히 다른 장점(?) 잘 모르겠다.

### 단점 
- Jenkis 서버 운영체제와 배포 운영체제가 틀릴 경우 npm install과정에서 운영체제에 따라 받아오는 라이브러리 틀려서 배포시 문제가 발생 할 수 있다.
- 배포서버에 불필요한 파일이 존재하며 node_modules을 scp로 옮기는 시간이 많이 걸린다.
(배포 서버에는 node_modules, ecosystem.config.js, dist파일만 필요)
- Jenkis에서는 node버전을 상황에 따라 여러개로 셋팅을 해줘야 한다.

## Plan 2
![](https://velog.velcdn.com/images/hong-brother/post/6817d2bf-4957-4ec3-9eae-a5c6a55be0fe/image.png)

### Jenkins flow
- 누군가에 의해서 develop 브렌치에서 merge가 되면 Github에서 WebHook을 통하여 변경 감지를 알려준다.
- Jenkins 서버는 즉시 소스코드를 Workspace에 clone을 받는다. (git clone)
- Jenkins 서버에서는 clone받은 파일을 scp를 통하여 배포서버에 파일을 보낸다.
- 배포 서버에서는 필요한 라이브러리를 받는다 (npm install)
- 배포 서버에서는 해당 코드를 javascript로 빌드한다(npm run build)
- 배포 서버에서는 pm2 reload하여 인스턴스를 재시작한다.

### 장점
- Jenkins에서는 node버전을 신경 쓰지 않아도 된다.
- Jenkins서버에서 배포 서버로 파일을 보낼때 코드만 보내므로 전송 속도가 빠르다.
- Jenkins서버와 배포서버 운영체제가 달라도 문제가 되지 않는다.

### 단점
- npm install, npm build등 서버에서 문제가 발생하면 바로 배포서버는 문제가 발생한다.

## 결과
![](https://velog.velcdn.com/images/hong-brother/post/8b9e8002-c15d-469e-a6a6-0f8757443f58/image.png)

- #45번은 Plan1 Job 시간이 2분 4초 이며 #54번은 Plan2 Job 시간이 1분 18초이다.
- 확실히 배포 속도가 약 50%감소 하였으며 좀더 빠르게 배포를 진행 할 수 있었다.
- Plan1과 Plan2를 비교 하였을때 안정성은 Plan1이라고 말 할 수도 있을 것 같다. 하지만 merge이전에 코드리뷰 및 테스트 케이스자동화로 인하여 문제될만한 것들은 사전에 차단하기 때문에 빠르게 배포하는것이 중요 하다고 생각한다
- 빠르게 배포하는이유는 팀원들과 같이 개발하는 중에 FE쪽에서 배포때문에 개발흐름이 끊어 질 수도 있고 개발 서버만큼은 빠르게 배포하여 피드백을 받는것이 중요하다고 생각하기 때문이다.

