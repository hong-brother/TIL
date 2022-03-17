# ISCSI(Internet Small Computer Systems Interface)
>
iSCSI(인터넷 소형 컴퓨터 시스템 인터페이스)는 데이터 저장 장비들을 연결하는 인터넷 프로토콜(IP) 기반의 저장 네트워킹 표준이다. 이 인터페이스는 확장성이 크고 구현 비용이 적다. 기존의 네트워크 인프라와 iSCSI를 통해서 기존 저장 공간을 확장하거나 백업 위치로 활용하는데 NAS를 사용할 수 있습니다.

iSCSI는 두 개의 종단, 즉 하나의 대상과 하나의 개시자로 구성되어 있습니다. 컴퓨터 상의 개시자는 iSCSI 클라이언트로 기능하고 iSCSI 호스트를 검색해서 대상을 설정하는 데 사용됩니다. 대상은 iSCSI 서버에 위치하는 저장 리소스입니다. iSCSI 대상은 종종 네트워크 연결 하드 디스크 저장 전용 장치(ASUSTOR NAS)입니다.

## ISCSI의 장점
- TCP/IP 네트워크를 통해서 SCSI I/O 명령을 고속으로 전송하기 위한 프로토콜 기반의 스토로지
- IP기반으로 로컬(LAN) 네트워크 및 광대역(WAN) 네트워크에서 저장소 운영 가능
- Window 및 Linux등의 운영 체제에서는 ISCSI 디스크는 실제 하드 디스크로 연결
- SAN기반 구성 보다는 대폭적 저비용 및 IP 네트워크 기반의 유연성이 매우 높음
- Network Teaming과 함께 구성시 매우 높은 고사용 시스템 구축

## 스토리지 활용 분야
- Microsoft Windows Cluster 공유 저장소
- Microsoft Exchange / SQL Server Database 저장소
- Hyper-V 및 VMWare 등의 가상화 인스턴스 이미지 저장소
- 로드 밸런싱 구성 다중 웹서버의 공유 저장소

# NAS에 ISCSI 생성
- SAN Mana

