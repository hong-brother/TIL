# jenkins 설치 환경
- CPU : 12cores
- Memory : 16GB
- DISK = 500GB
- OS : rocky8

# jenkins 설치
```
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
