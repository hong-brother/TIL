# Proxmox 란?
- Proxmox VE는 기업 가상화를위한 오픈 소스 서버 관리 플랫폼입니다. KVM 하이퍼 바이저와 LXC, 소프트웨어 정의 스토리지 및 네트워킹 기능을 단일 플랫폼에 긴밀하게 통합합니다. 통합 된 웹 기반 사용자 인터페이스를 사용하면 VM 및 컨테이너, 고 가용성 클러스터 또는 통합 재해 복구 도구를 쉽게 관리 할 수 있습니다.
- Hypervisor종류중 하나이며 HostOS에 따라 Type1, Type2로 나뉠수 있습니다.

![](https://images.velog.io/images/hong-brother/post/07c5cecd-8897-45b4-bfb0-8f438926c02d/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-10%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.21.47.png)
- Hypervisor란 가상머신을 생성하고 구동하는 소프트웨어 입니다. 가상머신 모니터라고도 불리는 하이퍼바이저 운영 체제와 가상 머신의 리소스를 분리해 vm의 생성과 관리를 지원합니다. 또한 하이퍼바이저는 위와 같이 Type1, Type2로 나뉠수 있습니다.

# Install Proxmox
- 설치는 어렵지 않다. [Proxmox](https://www.proxmox.com/en/)에서 ISO파일을 다운로드 받아 설치하면됩다.
- 설치후 http://호스트IP:8006/으로 접속해서 확인 할 수 있다.
![](https://images.velog.io/images/hong-brother/post/a5bd803b-42bf-4639-9ad7-fee0de1c9820/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-10%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.35.37.png)

# 설치 후 설정
## 구독 설정 해제
- 설치 후 ssh 접속하고 나서 apt-get update를 통해 패키지를 업데이트가 되어야 하는데 subscribe 설정이 되어 있어 업데이트가 되지 않는다.
![](https://images.velog.io/images/hong-brother/post/eb49bee0-ef5c-4909-bd99-06ac3a88bf4b/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-10%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.48.02.png)
- 무료 라이센스를 선택하여 패키지를 업데이트 하자
1. /etc/apt/sources.list 변경

```bash
deb http://ftp.debian.org/debian bullseye main contrib
deb http://ftp.debian.org/debian bullseye-updates main contrib

# PVE pve-no-subscription repository provided by proxmox.com,
# NOT recommended for production use
deb http://download.proxmox.com/debian/pve bullseye pve-no-subscription

# security updates
deb http://security.debian.org/debian-security bullseye-security main contrib
```
- nano /etc/apt/sources.list 하여 pve-no-subscription을 추가 하자

2. /etc/apt/sources.list.d 변경
- /etc/atp/sources/list.d의 pve-enterprise.list 파일 변경
```
pve-enterprise.list
# deb https://enterprise.proxmox.com/debian/pve bullseye pve-enterprise
```

- /etc/atp/sources/list.d의 pve-no-subscription.list 파일 생성
```
pve-no-subscription.list
deb http://download.proxmox.com/debian/pve bullseye pve-no-subscription
```

- 패키지 업데이트
```
apt-get update
apt-get dist-upgrade
```

## NO-SUBSCRIPTION 팝업 제거
- 작업 경로 이동
```
cd /usr/share/javascript/proxmox-widget-toolkit
```

- 파일 백업
```
cp proxmoxlib.js proxmoxlib.js.back
```

- 팝업수정
```
vi proxmoxlib.js
```
![](https://images.velog.io/images/hong-brother/post/4e83f375-7e9a-4b7f-b117-9f1e19da0c8b/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-10%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%206.10.59.png)
- 블록으로 처리한 부분이 구독권한이 없으면 팝업이 되도록 되어 있다. 해당 부분에 Ext.Msg.show를 void로 변경한다.
>
간만에 보는 ExtJS 2018년?에 한참 많이 썼는데 그때 기억으로는 개발자가 UI를 신경 쓸 필요 없이 SPA형태의 페이지를 만들 수 있어서 편하게 개발했는데 라이센스 비용이 엄청 비싼 기억이...

## Dark UI기능 반영
- Proxmox-Shell 접근
```
~# wget https://raw.githubusercontent.com/Weilbyte/PVEDiscordDark/master/PVEDiscordDark.sh
~# bash PVEDiscordDark.sh install
```
- 위와같이 git에서 다운로드 받아서 실행한다. 실행 후 새로고침
![](https://images.velog.io/images/hong-brother/post/794af22d-9d0c-45f9-a0e3-2375c754bdd2/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-03-10%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%206.29.32.png)


# 참고
- [vembu](https://www.vembu.com/blog/type-1-and-type-2-hypervisor/)
- [hypervisor](https://www.redhat.com/ko/topics/virtualization/what-is-a-hypervisor)
- [PVEDiscordDark](https://github.com/Weilbyte/PVEDiscordDark)
