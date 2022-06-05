# nGrinder란?
nGrinder는 네이버에서 성능 측정 목적으로 jython(JVM위에서 파이썬이 동작)으로 개발 된 오픈소스 프로젝트이며, 2011년에 공개 하였습니다. 바닥부터 개발을 한 것이 아니라 The Grinder라는 오픈소스 기반에서 개발 하였습니다. nGrinder는서버에 대한 부하를 테스트 하는 것이므로 서버의 성능 측정이라고도 할 수 있습니다. 성능 측정이란 것은 실제 서비스에 투입 되기 전, 실제와 같은 환경을 만들어 놓고 서버가 사용자를 얼마 만큼 수용할 수 있는지를 실험 할 때 사용합니다. 만약 이와 같은 테스트를 하지 않으면, 엔지니어가 동시 접속자를 1000명정도로 예상하고 이에 맞는 설정을 구성하는데 예상에 넘는 동시 접속자가 발생해 버리면 서버가 죽어버려 서비스를 할 수 없는 문제가 있습니다. 이를 방지하기 위해 본 서비스에 앞서 테스트를 해 서버의 성능을 테스트 하는 것입니다. 

## 특징
- Jython 스크립트를 사용하여 테스트 시나리오를 만들고 여러 에이전트를 사용하여 JVM에서 스트레스를 생성합니다.
- 사용자 정의 라이브러리(jar, py)로 테스트를 확장합니다. 사실상 무제한입니다.
- 프로젝트 관리, 모니터링, 결과 관리 및 보고서 관리를 위한 웹 기반 인터페이스를 제공합니다.
- 여러 테스트를 동시에 실행합니다. 사전 설치된 여러 에이전트를 할당하여 각 에이전트의 활용도를 최대화합니다.
- 여러 네트워크 지역에 에이전트를 배포합니다. 다양한 네트워크 위치에서 테스트 실행
- 스크립트를 관리하기 위해 Subversion을 포함합니다.
- 스트레스를 생성하는 에이전트와 스트레스를 받는 대상 머신의 상태를 모니터링할 수 있습니다.
- NHN에서 1억 명 이상의 사용자가 있는 거대한 시스템을 테스트하는 데 사용되는 검증된 솔루션입니다.

## 아키텍쳐
![](https://velog.velcdn.com/images/hong-brother/post/1292d89a-d9a4-4400-9c15-148303884822/image.png)

### controller
- 성능 테스트를 위한 웹 인터페이스를 제공
- 테스트 프로세스를 조정
- 테스트 통계를 수집하고 표시
- 사용자가 스크립트를 생성하고 수정할 수 있는 기능 지원
### Agent
- Agent Mode가 실행될 때 Target 서버에 프로세스 및 스레드를 실행 시켜 부하 발생
- Moniter 모드에서 실행 시 대상 시스템 성능 (예 : CPU / 메모리) 모니터링

# Docker를 이용한 nGrinder환경 설정 구축
## controller
- compose로 구축한다.
```
version: '3.7'

services:
  controller:
    container_name: ngrinder-controller
    image: ngrinder/controller:latest
    restart: always
    environment:
      - TZ=Asia/Seoul
    ports:
      - "8880:80"
      - "16001:16001"
      - "12000-12009:12000-12009"
    volumes:
      - ./ngrinder-controller:/opt/ngrinder-controller
```

## agent
- 상황에 맞게 agent를 늘리거나 줄일 수 있도록 docker-compose로 구축한다.
- **agent 구축시 docker-compose 안에 command안에 controller의 ip로 변경한다.**
```
version: '3.7'

services:
  agent-1:
    container_name: ngrinder-agent-1
    image: ngrinder/agent:latest
    command: ["192.168.20.104:8880"]
    environment:
      - TZ=Asia/Seoul
    volumes:
      - ./ngrinder-agent-1:/opt/ngrinder-agent
  agent-2:
    container_name: ngrinder-agent-2
    image: ngrinder/agent:latest
    command: ["192.168.20.104:8880"]
    environment:
      - TZ=Asia/Seoul
    volumes:
      - ./ngrinder-agent-2:/opt/ngrinder-agent
  agent-3:
    container_name: ngrinder-agent-3
    image: ngrinder/agent:latest
    command: ["192.168.20.104:8880"]
    environment:
      - TZ=Asia/Seoul
    volumes:
      - ./ngrinder-agent-3:/opt/ngrinder-agent
  agent-4:
    container_name: ngrinder-agent-4
    image: ngrinder/agent:latest
    command: ["192.168.20.104:8880"]
    environment:
      - TZ=Asia/Seoul
    volumes:
      - ./ngrinder-agent-4:/opt/ngrinder-agent
  agent-5:
    container_name: ngrinder-agent-5
    image: ngrinder/agent:latest
    command: ["192.168.20.104:8880"]
    environment:
      - TZ=Asia/Seoul
    volumes:
      - ./ngrinder-agent-5:/opt/ngrinder-agent 
```

![](https://velog.velcdn.com/images/hong-brother/post/59d29538-fdcd-42e1-b4a0-fdfefd46efc1/image.png)

- nGrinder ui 접속하면이다. (계정과 초기 비밀번호는 admin / admin 이다.)

![](https://velog.velcdn.com/images/hong-brother/post/2d2c6f52-0af0-43de-a53b-ae4860ecbca3/image.png)

 - 화면과 같이 구축한 agent 정보를 managerment에서 확인 할 수 있다.
