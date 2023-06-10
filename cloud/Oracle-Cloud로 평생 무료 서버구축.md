# Oracle Cloud로 평생 무료 서버 구축

## Oracle Cloud
- 2019년 오라클 오픈월드 2019에서 오라클 클라우드 프리티어가 공개
상시 무료 서비스를 제공하며, 추가적으로 $300의 크레딧을 제공
![](https://velog.velcdn.com/images/hong-brother/post/df1d481b-54b2-4c4a-b141-238859a417a8/image.png)
![](https://velog.velcdn.com/images/hong-brother/post/30aac99b-4d49-4f7d-9a96-76940e702044/image.png)



# Oracle Cloud 가입

- [오라클 클라우드 무료 체험 3분가이드](https://www.youtube.com/watch?v=yUA4FziG1Ak)

## VM 인스턴스 생성

### Create a VM Instance
![](https://velog.velcdn.com/images/hong-brother/post/43c86c7e-9705-4beb-b631-f3282a2790ba/image.png)

### Create compute instance
![](https://velog.velcdn.com/images/hong-brother/post/843f1a1e-1a4c-46fe-9d75-dfcd7f476b7b/image.png)

- 이름
	- 인스턴스 이름 설정
- 이미지 및 구성
	- 이미지는 Oracle Linux8 말고도 ubuntu, oracle 등 다양한 프리티원 지원하는 이미지를 선택 할 수 있다.
    
![](https://velog.velcdn.com/images/hong-brother/post/dbbe6029-2dbc-467c-b42d-5d8b59d4ab6f/image.png)

- 네트워킹
	- 새 가상 클라우드 네트워크를 생성 선택
	- 새 공용 서브넷 생성 선택
    

## vm instance 접속

- ssh 접속 (ssh key는 인스턴스 생성시 다운로드 받은 파일)

```bash
ssh -i 'ssh key' 사용자이름@공인Ipv4주소
```

- 만약 Permission denied (publickey,gssapi-keyex,gssapi-with-mic) 발생시

```bash
chmod 400 key~
ssh -i 'ssh key' user@ip
```
