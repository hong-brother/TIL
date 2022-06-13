# Github webhook자동배포 구축
> 
github의 특정 저장소에서 push가 되었을때 감지하여 jenkins에서 자동으로 빌드 및 배포 되도록 설정한다.

## plugin 설치
### Github Integration 플러그인 설치
- Dashboard > Plugin Manager 의 설치 가능 tab 선택 후 **"Github Integration"** 설치.
![](https://velog.velcdn.com/images/hong-brother/post/4641496e-159b-4d87-9fd0-ec616c87a78e/image.png)

## AccessToken 발급
- Github > Settings > Developer settings > Personal access token 접속

![](https://velog.velcdn.com/images/hong-brother/post/4d955ff1-b16c-436e-b623-4e055eef17f0/image.png)
- personal access tokens의 generate new token을 클릭한다.

![](https://velog.velcdn.com/images/hong-brother/post/6cdfb7f9-3090-457c-9cae-6ad13c60b4c5/image.png)
- token의 note명을 명시하고 repo, admin:repo_hook에 관련된 권한을 클릭한다. 또한 토큰에 대한 만료는 관리가 될 수 있도록 설정한다.
- **생성된 token은 따로 저장해둔다.**

## Jenkins에 personal access token을 위한 jenkins credential 등록
- jenkins > Dashboard > Credentials > System > Global credentials 접속 add Credentials 을 선택한다.
![](https://velog.velcdn.com/images/hong-brother/post/bb8f5555-7d6d-4303-bce1-6eaa8bb9cf15/image.png)

- 아래 그림과 같이 설정한다.
	- Kind = Secret text 
    - Scole = Global(Jenkins, nodes, items, all child items, etc)
    - Secret = **github에서 발급 받은 token**
    - ID = Jenkins 내에서 구분할 수 있는 ID(ex. Pipeline script 작성시 해당 인증을 가져 올 수 있는 ID)
    - Description = 해당 인증의 내용
![](https://velog.velcdn.com/images/hong-brother/post/e6f479e5-1fb2-4138-9e7b-15089b3ee232/image.png)

## Github webhook 설정
- 해당 타켓 저장소로 이동후 Settings > Webhooks을 선택한다.
![](https://velog.velcdn.com/images/hong-brother/post/d1e67d84-4d89-4e02-997a-7d35ddf0dc14/image.png)
- Payload URL은 Jenkins의 URL의 /github-webhook/을 추가한다.
- Content type은 application/json으로 설정한다.


## 참고
- https://www.jenkins.io/doc/
- https://lindarex.github.io/jenkins/jenkins-github-webhook-setting/
- https://nirsa.tistory.com/301







