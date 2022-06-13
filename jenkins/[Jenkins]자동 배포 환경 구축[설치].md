# Docker로 이용한 jenkins 환경 구축
> docker를 이용한 jenkins를 설치해보자

## jenkins 최소 사양
jenkins를 설치하기 위한 최소한의 서버 사양이다.
- CPU : 1CORE
- Memory : 1GB
- DISK = 빌드프로젝트 코드 용량 및 빌드 된 결과물의 용량

## jenkins 설치 사양
서버내에 jenkins를 설치하기 위해서 넉넉한 리소스를 부여 하였다. 범용적으로 사용될 jenkins를 생각하여 용량은 충분히 500GB를 할당.
- CPU : 12cores
- Memory : 16GB
- DISK = 500GB
- OS : rocky8


## jenkins 설치
- docker-compose.yml 안에 해당 내용을 작성한다. 
docker-compose안에 volumes mapping을 통하여 host 디렉토리에 jenkins 환경 설정 파일이 저장되도록 설정한다.
```bin/bash
version: '3'
services:
  jenkins:
    container_name: 'inno-jenkins-project'
    image: 'jenkins/jenkins:latest'
    restart: always
    ports:
      - 8080:8080
      - 50000:50000
    volumes:
      - ./jenkins_home:/var/jenkins_home
    environment:
      TZ: "Asiz/Seoul"
```
- docker-compose를 실행한다.
```
docker-compose up -d
```
- 홈디렉토리의 jenkins_home/secrets/initialAdminPasswd 내용을 확인하여 초기 패스워드를 입력한다.

![](https://velog.velcdn.com/images/hong-brother/post/af81a40d-c29f-4675-9df9-12ba9c63a40a/image.png)

- 필요한 플러그인 설치(Install suggested plugins)

![](https://velog.velcdn.com/images/hong-brother/post/188db778-1b34-4a28-a4e1-04698e16c8f4/image.png)
![](https://velog.velcdn.com/images/hong-brother/post/a09184e9-c51b-4104-9f94-7db439515c30/image.png)


