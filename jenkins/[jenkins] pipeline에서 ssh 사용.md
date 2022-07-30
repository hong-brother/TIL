# GOAL
- Jenkins에서 PipeLine을 이용하여 배포 script 작성시 ssh 사용하기
>
전제조건
ssh에 필요한 ssh agent가 설치 되어 있다는 전제조건으로 시작합니다.
Jenkins는 docker에서 기반으로 실행되고 있습니다.

# SSH 인증서 생성하기
- **SSH 인증키는 jenkins가 설치된 서버에서 생성합니다. **
Native로 설치된 Jenkins라면 home 디렉토리 에서 .ssh로 이동합니다. 또는 docker container로 실행된 jenkins라면 jenkins_home/.ssh 디렉토리로 이동합니다.

- 인증서를 생성합니다.
```bash
ssh-keygen -t rsa -b 4086
enerating public/private rsa key pair.
Enter file in which to save the key (/home/hsnam/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/hsnam/.ssh/id_rsa.
Your public key has been saved in /home/hsnam/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:es3MyESZ54fFjTKE5m3VP6NONzVmaxVMG26G2BxcWRk hsnam@hsnam-ssh.localdomain
The key's randomart image is:
+---[RSA 4086]----+
|         .. .o+E*|
|        o+ .=+==o|
|       o+.+o++.*.|
|       ..oo=  o*+|
|        S.o . + B|
|       + * . o = |
|      . + = o o .|
|       .     .   |
|                 |
+----[SHA256]-----+
```
인증키 생성시 공개키(id_rsa.pub), 비밀키(id_rsa)가 생성된다.

- 공개키를 원격 호스트에 복사한다.(배포대상 서버)
```bash
ssh-copy-id -i /home/hsnam/.ssh/id_rsa.pub hsnam@192.168.0.2
```

- Host key checking 
터미널에서 ssh 접속시 yes/no로 접속할지 물어본다. jenkins에서 ssh접속시 접속여부에 물어보지 안도록 한번은 아래와 옵션으로 접속하여 사용하도록 한다.
```bash
ssh -o StrictHostKeyChecking=no hsnam@192.168.0.2
```
만약 docker container로 운영중인 jenkins라면 container로 접속하여 설정하도록 하자.!

# Jenkins에 SSH 비밀키 등록
>
Dashboard > Credentials > System > Global credentials > Add Credentials

1. 인증서 등록
![](https://velog.velcdn.com/images/hong-brother/post/a357c789-b0fd-42b2-b806-649292952b3e/image.png)
- ID
	- PipeLine에서 설정한 ID로 설정값을 가져오기 때문에 중복되지 않은 ID를 입력한다.
- Private Key 
	- Enter directly를 설정하여 위에서 생성한 private key를 입력한다.
    
    
# PIPELINE 에서 SSH 사용하기
```js
env.TARGET_HOST = "hsnam@192.168.0.2"
node {
    try {
        stage('ssh-test') {
            sshagent (credentials: ['192.168.0.2-ssh']) {
                sh 'ssh -o StrictHostKeyChecking=no "uptime"'
            }
        }
    } catch (env) {
        echo 'error = ' + env
        throw env
    }
}
```
- 위와 같이 pipline script를 작성하고 job을 실행 하면 해당 서버의 uptime 결과를 확인 할 수 있다.
