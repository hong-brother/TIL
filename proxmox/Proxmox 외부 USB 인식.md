# USB Drive 자동 인식 
> Proxmox 에서 컨테이너 및 VM에 HDD나 SSD를 인식 시켜야 할 경우에 참고하는 글이다.

## Proxmox에 USB 인식 확인
![](https://velog.velcdn.com/images/hong-brother/post/108e4755-b3ed-4285-a92e-bf7923bad962/image.png)

- Proxmox의 disk 리스트를  위와 같이 조회 하면 SSD가 목록에 표시되었지만 마운트가 되지는 않았다.

## USB 자동마운트 활성화
- proxmox 노드에서 shell을 열고 다음과 같이 입력하여 **pve6-usb-automount** 를 설치합니다

```bash
apt-key adv --recv-keys --keyserver keyserver.ubuntu.com 2FAB19E7CCB7F415
echo "deb https://apt.iteas.at/iteas buster main" > /etc/apt/sources.list.d/iteas.list
apt update
apt install pve6-usb-automount
```

- 설치 후 서버에 마운트되어 있는 저장매체를 다시 물리적으로 해제하고 마운트 시킵니다.
![](https://velog.velcdn.com/images/hong-brother/post/f1fb1ffc-a3a9-48dc-b580-db316248cf7c/image.png)
- 위와 같이 자동으로 마운트 되어 있는 것을 확인 할 수 있습니다.

## LXC 내에 HDD/SSD 마운트
- 마운트 정보를 확인합니다.

```bash
lsblk
```

- proxmox 노드에 shell을 열고 다음과 같이 입력합니다. (mounbt point 가 겹치지 않도록 주의)
```bash
pct set 161 -mp4 /media/sde1,mp=/home/hsnam/Project/ftpData/hdd/hdd_2
```

![](https://velog.velcdn.com/images/hong-brother/post/ca20faed-e134-4a29-af8f-ac65cc95627e/image.png)

- 위와 같이 lxc에 마운트 되어 있는 것을 확인 할 수 있다.

