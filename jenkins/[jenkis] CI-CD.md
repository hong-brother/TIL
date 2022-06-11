# 
# CI/CD - 지속적인 통합, 서비스 제공, 배포
![](https://velog.velcdn.com/images/hong-brother/post/644a2bcb-333c-4254-97f5-c1af8f4aea9c/image.png)

## CI/CD란?
### CI(Continuous Integration) - 지속적인 통합
- 애플리케이션의 버그 수정이나 새로운 코드 변경이 주기적으로 빌드 및 테스트되면서 공유되는 레파지토리에 통합(merge)되는 것을 의미 한다.
- CI 중요한 포인트로는 코드 변경사항을 주기적으로 빈번하게 통합(merge), 즉. 작은 단위로 나눠서 통합한다. 
통합을 위한 단계(빌드, 테스트, 통합)의 자동화한다.

### CD(Continuous Delivery, Continuous Deployment) 지속적인 제공, 지속적인 배포
- Continuous Delivery(지속적인 제공)
	- CI를 통해서 새로운 소스코드의 빌드와 테스트 병합까지 성공적으로 진행되었다면, 빌드와 테스트를 걸쳐 레파지토리에 병합(merge)되는 것을 의미한다.
- Continuous Deployment(지속저인 배포)
	- 레파지토리에 병합된 내역을 사용자가 사용할 수 있는 배포환경까지 배포 되는 것을 의미한다.
    
## CI/CD의 장점
### 장점
- 자동화를 통한 개발 생산성 향상 및 빠르게 문제점을 발견 할 수 있다. 발견 버그는 수정하기 용이하다.
- 코드의 퀄리티를 향상(단위테스트)
- 배포 자동화
### 단점
- 인프라 구축(Jenkins 설정 다소 복잡)


![](https://velog.velcdn.com/images/hong-brother/post/6c04385a-9e8c-408d-ab97-1bcf70b7e178/image.png)
>
즉 어플리케이션 개발 간계부터 배포 때까지 이 모든 단계들을 자동화를 통해서 조금 더 효율적이고 빠르게 사용자에게 빈번이 배포 할 수 있도록 하는 것.
