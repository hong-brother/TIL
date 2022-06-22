# Pipeline 
## 프로젝트 생성
- jenkins의 dashboard에서 새로운 item을 추가합니다.
![](https://velog.velcdn.com/images/hong-brother/post/cae72aac-1cff-4095-8cf9-a13d2b74d1ec/image.png)

## General
- General tab에서는 해당 pipline에 설명을 작성할 수 있으며 해당 pipeline에 대한 빌드 보관 전략(?)에 대해서 정책을 수립할 수 있습니다. 
예를 들어 해당 파이프라인에 빌드를 최대 100개 까지만 유지하고 빌드 이력에 대해서 30일 까지만 보관할 수 있도록 정책을 수립할 수 있습니다.
![](https://velog.velcdn.com/images/hong-brother/post/0677cad4-cbe3-4968-800f-c2973f3c3ee5/image.png)

## Build Triggers
- 빌드 트리거에서는 빌드 유발에 대한 액션에 대해서 정할 수 있습니다. 해당 pipeline은 github의 hook trigger를 사용 할 것으로 **GitHub hook trigger for GITScm polling** 항목에 체크합니다.
![](https://velog.velcdn.com/images/hong-brother/post/14b8a332-7f2a-44bc-9737-7d3fdfe74990/image.png)

## Pipeline
> - PIPELINE 이란?
파이프라인은 사용자 정의된 CD파이프라인 모델이다. 파이프라인을 통해 코드를 build 프로세스를 정의하고 stage를 포함하여 그것을 테스트하고 배포합니다.

- Jenkins pipeline은 Declarative pipelines과 Scripted pipelines을 지원합니다.
Scripted pipelines는 Groovy DSL을 기반으로 한 Jenkins pipeline에서 사용하는 전통적인 방법입니다.
Declarative pipelines은 Jenkins에 비교적 최근에 추가되었으며 Pipeline 하위 시스템 위에서 더 간단한 문법을 제공합니다.
해당 파이프라인에서는 Scripted pipelines을 사용하겠습니다.
- 아래와 같이 node안에 stage-install-build-Sonarqube Analysis - Unit Test - deploy 단계로 나눠어서 파이프라인을 작성하였습니다.
```
node {
    String targetBranch = "develop" 
    String TARGET_HOST = "jenkins@192.168.0.1"

    try {
        stage('Clone') {
        
        stage('Install') {
        }
        
        stage('Build') {
        }
        
        stage('Sonarqube Analysis') {
       }
       
        stage('Unit Test') {
        }
        
        stage('deploy') {
        }
    } catch (env) {
        // notifyMSTeams('FAIL')
        echo 'error = ' + env
        throw env
    }
}

```
### stage('clone')
- 해당 스테이지에서는 GitHub hook trigger에 의해서 pipeline이 작동 되면 깃허브 저장소에서 코드를 clone 하는 역할을 합니다.

### stage('Install')
- 해당 스테이지에서는 해당 코드에서 필요한 라이브러리를 설치하는 stage입니다. 예를 들어 java인경우는 gradle, maven에서 라이브러리를 설치하는 케이스가 될 것이며 nodejs경우 package.json에 정의된 라이브러리를 설치하는 케이스가 될 것입니다.

### stage('Build')
- 해당 스테이즈에서는 소스코드를 빌드하는 역할을 합니다. 언어에 따라 java는 java파일을 class로 바꾸는 빌드 스테이지가 필요 하며 nestjs같은 경우는 typescript를 javascript로 빌드해야 하는 스테이지가 필요합니다.

### stage('Sonarqube Analysis')
- 해당 스테이지에서는 소스 코드에 대한 정적분석을 합니다. 정적분석에 대한 툴은 많이 있지만 대표적으로 sonarqube를 사용하며 해당 스테이지에서 sonarqube로 코드를 전송하여 정적분석에 대한 리포팅을 받습니다.

### stage('Unit Test')
- 해당 스테이지에서는 소스 코드에 대한 단위 테스트를 진행합니다. merge 된 브렌치에서 테스트 없이 배포한다는 것은 그만큼 위험 부담감이 큰 엄청난 일입니다. 단위테스트를 통해 통과된 코드가 배포 되어야만 휴먼에러를 최소한으로 줄일 수 있으며 버그유발 확률을 최소화 시킬 수 있습니다.

### stage('deploy')
- 해당 스테이지에서는 단위 테스트까지 끝난 코드를 서버에 배포합니다. 배포하는 방법은 여러가지가 있습니다. 대표적으로 sftp나 scp로 파일을 전송하여 배포할 수도 있으며 docker로 도커라이징하여 배포 할 수 있습니다. 

## 결과 
- 깃허브에 특정 브렌치에 merge가되어 push가 된다면 jenkins에서 빌드를 감지하여 clone-install-build-analysis-unit test-deploy 순으로 stage를 진행하여 배포합니다.
- 자동배포를 구축하므로서 코드가 수정되었을때 일일이 사람이 배포하는 시간을 줄여 그만큼 코드 퀄리티를 높일 수 있도록 집중하게 됩니다. 또한 단위테스트 정적분석을 통하여 코드 퀄리티를 향상 시킬 수 있습니다.
![](https://velog.velcdn.com/images/hong-brother/post/6fef4553-9f71-4f5d-90d8-3d48d51cf4b1/image.png)

