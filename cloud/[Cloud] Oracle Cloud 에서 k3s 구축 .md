# k3s란?

# k3s 구축
## 필요 resource
- Timezone 설정
```bash
$sudo yum install tzdata
sudo ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime
sudo reboot
```

- 방화벽 오픈
```
firewall-cmd --permanent --add-port=80/tcp 
```


## k3s 설치
### 마스터 노드에 k3s 설치
- install k3s
```bash
sudo yum install update
sudo curl -sfL https://get.k3s.io | sh -s - server
```
- 환경 변수 설정
```bash
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
or
echo "KUBECONFIG=/etc/rancher/k3s/k3s.yaml" >> ~/.bashrc

```

### 설치 상태 확인
- 서비스 상태 확인
```bash
service k3s status 
or
sudo systemctl status k3s
```
![](https://velog.velcdn.com/images/hong-brother/post/d720cefc-25f2-4bed-acf1-93fe358a9650/image.png)

- kubectl 명령어 확인
```bash
k3s kubectl get pods
```
![](https://velog.velcdn.com/images/hong-brother/post/73202138-e7b2-4c6b-8999-22adfd689559/image.png)
만약 **permission denied** 발생한다면 아래 명령어로 해결
```bash
sudo chmod 644 /etc/rancher/k3s/k3s.yaml
```
### Master node의 토큰 확인
```
sudo cat /var/lib/rancher/k3s/server/node-token
```


### Worker node에서 마스터 노드 등록
```bash
sudo curl -sfL https://get.k3s.io | K3S_URL=https://myserver:6443 K3S_TOKEN=XXX sh 
```

