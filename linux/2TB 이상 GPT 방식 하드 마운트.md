## 2TB  이상 디스크 GPT 파일 시스템 방식으로 마운트

### 디스크 정보 확인

```bash
sudo lshw -C disk
sudo fdisk -l
```

### 파티션 생성

```bash
mapping@mapping-PowerEdge-R740:~$ sudo parted /dev/sdb
GNU Parted 3.3
Using /dev/sdb
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) mklabel gpt
(parted) unit TB
(parted) mkpart
Partition name?  []? data (옵션)
File system type?  [ext2]? ext4
Start? 0
End? 100%
(parted) print
Model: DELL PERC H730P Adp (scsi)
Disk /dev/sdb: 16.0TB
Sector size (logical/physical): 512B/4096B
Partition Table: gpt
Disk Flags:

Number  Start   End     Size    File system  Name  Flags
 1      0.00TB  16.0TB  16.0TB  ext4         data

(parted) q (종료)
```

### 포맷하기

```bash
sudo mkfs -t ext4 /dev/sdb1
```

### 마운트

```bash
sudo mkdir /mnt/hdd/data
sudo mount /dev/sdb1 /mnt/hdd/data
chmod 777 /mnt/hdd/data
```

### 자동 마운트 (uuid 확인)

```bash
sudo ls -al /dev/disk/by-uuid
lrwxrwxrwx 1 root root 10  9월 16 10:22 608aedd5-1139-4822-a935-c643af8b96dd -> ../../sdb1
lrwxrwxrwx 1 root root 10  8월 31 19:36 9cbf855f-a17e-4f32-a8c3-94cc49f85acc -> ../../sda2

sudo nano /ect/fstab
UUID=608aedd5-1139-4822-a935-c643af8b96dd       /mnt/hdd/data   ext4    defaults    0       0

sudo mount -a
df -h 
```

### 심볼링크 추가

```bash
ls -s /mnt/hdd/data/mapping /home/mapping/data
sudo chmod -r 777 /mnt/hdd/data
```
