
# CPU 테스트 방법
## stress 설치
- sudo apt install stress -y (ubnutu)
- sudo yum install stress -y (centos)

## 코어 갯수 확인
- grep -c processor /proc/cpuinfo

## 부하 테스트
- sudo stress --cpu 코어 갯수 --timeout 3600 (1시간)

## 부하 확인
```
htop
```

# GPU 테스트 방법

## gpu-burn 설치
```
git clone https://github.com/wilicc/gpu-burn
cd gpu-burn
make
```
참고로 make명령어로 컴파일을 하려면
GPU서버에 알맞은 그래픽 드라이버와 CUDA가 설치되어있어야합니다.
그렇지 않으면 스트레스 테스트를 실행할수가 없습니다.

## 부하테스트 
- 120초동안 태우기
```
./gpu_burn 120
```
- 한 시간동안 태우기
```
./gpu_burn 3600
```

## 부하 확인
```
watch -n -1 nvidia-smi
```
