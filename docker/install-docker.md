* 해당 문서는 공식 문서를 기반으로 작성하였으며 설치환경은 CentOS8 에서 설치 하였습니다. [docker](https://docs.docker.com/engine/install/)

# Install Docker Engine

1. Set Up the Repository 

```bash
$ sudo yum install -y yum-utils
$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

2. Install Docker Engine

```bash
$ sudo yum install docker-ce docker-ce-cli containerd.io -y
```

3. Start docker

```bash
$ sudo systemctl start docker
$ sudo systemctl enable docker
```

4. docker 유저 권한 추가
- sudo 없이 docker 명령어를 사용할 수 있게 한다. 
단. sudo 없이 docker 명령어를 입력하므로 신중하게 입력한다.!

```bash
$ sudo usermod -aG docker $USER
```
이후 로그아웃 또는 재부팅한다.

# Enable Docker Remote API

1. stop docker

```bash
$ sudo systemctl stop docker
```

2. modify docker service

```bash
$ sudo vi /lib/systemd/system/docker.service
```

3. add Config

```bash
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:4243 --containerd=/run/containerd/containerd.sock
```

4. start docker

```bash
$ sudo systemctl daemon-reload
$ sudo systemctl start docker
```

5. check Remoe API.

```bash
curl http://127.0.0.1:4243/version
```

# Install Docker-compose

1. download docker-compose

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

2. Set Permissions

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

3. check docker-compose

```bash
docker-compose version
```
