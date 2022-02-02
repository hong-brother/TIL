### install nvidia toolkit

```bash
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit
sudo systemctl restart docker
```

### check docker container 실행

```bash
docker run -it --gpus all --name prj_gorio pytorch/pytorch:1.6.0-cuda10.1-cudnn7-devel /bin/bash
```

### check

```bash
pip freeze
python -V
python
import torch

# GPU check
torch.cuda.is_available()

# GPU 정보 확인
torch.cuda.get_device_name(0)

# 사용 가능한 GPU 개수
torch.cuda.device_count()

# 현재 GPU 번호
torch.cuda.current_device()

# device 정보 반환
torch.cuda.device(0)
```

### default로 GPU 사용하기
daemon.json 내용 수정
```
sudo vim /etc/docker/daemon.json
```

```bash
{
    "default-runtime": "nvidia",
    "runtimes": {
        "nvidia": {
            "path": "nvidia-container-runtime",
            "runtimeArgs": []
        }
    }
}
```

- 만약 daemon.json가 없으면 생성한다.

```bash
dockerd --config-file /etc/docker/daemon.json
```
