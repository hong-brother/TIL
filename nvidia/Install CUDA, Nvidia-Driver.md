![cuda](https://images.velog.io/images/hong-brother/post/96406844-8165-41cd-bad2-f2cdd43e4454/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-06%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2012.02.09.png)
## 까탈스러운 쿠다 ㅠ
회사내에 프로젝트 진행할때 AI 개발팀에서 추론코드를 백엔드와 연결하기 위해서 또는 FastAPI로 변경해서 백엔드 프로젝트를 해야 하는 상황이 와버렸다 백엔드 개발하기도 바쁜데 python에 딥러닝이라니 ㅠㅠ
먼저 AI쪽 딥러닝프레임 워크 PyTorch, Tensorflow를 시작하기 위해서는 cuda라는 녀석을 알아야 한다. [CUDA](https://ko.wikipedia.org/wiki/CUDA)
하지만 **CUDA** 너란 녀석... 만만하게 볼 친구가 아니고 정말 예민한 친구이다.
PyTorch, Tensorflow를 사용하는 AI쪽 개발자들은 너무나도 잘 다루시겠지만...
나같이 서비스를 만드는 백엔드 개발자는 딥러닝쪽에 너무나도 무지하다.
**CUDA**를 설치하기 위해서는 당연히 Nvidia그래픽카드가 있어야한다.
또하나 그래픽 카드만 있어서 되는건 아니고 **해당 그래픽카드에 맞춰 설치해야하는 cuda-driver 버전이 달라진다.**
먼저 그래픽카드 정보를 확인하자! [NVDIA HOME](https://developer.nvidia.com/cuda-gpus)
// cuda version과 nvidia-driver 버전 확인 하는 방법 작성중...

## 설치 기준
- 하드웨어(GPU)
	- Tesla T4 * 4EA
- OS
```bash
  hsnam@hsnam-PowerEdge-R740:~$ cat /etc/*release
  DISTRIB_ID=Ubuntu
  DISTRIB_RELEASE=20.04
  DISTRIB_CODENAME=focal
  DISTRIB_DESCRIPTION="Ubuntu 20.04.3 LTS"
  NAME="Ubuntu"
  VERSION="20.04.3 LTS (Focal Fossa)"
  ID=ubuntu
  ID_LIKE=debian
  PRETTY_NAME="Ubuntu 20.04.3 LTS"
  VERSION_ID="20.04"
  HOME_URL="https://www.ubuntu.com/"
  SUPPORT_URL="https://help.ubuntu.com/"
  BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
  PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
  VERSION_CODENAME=focal
  UBUNTU_CODENAME=focal
```
- 커널 정보
```bash
- 커널 정보
hsnam@hsnam-PowerEdge-R740:~$ uname -a
Linux hsnam-PowerEdge-R740 5.11.0-27-generic #29~20.04.1-Ubuntu SMP Wed Aug 11 15:58:17 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
```
- 설치 드라이버
	- CUDA 11.2
	- Nvidia-driver-460

### install cuda-11.2

- Configuration(사전 필요 디펜던시 설치)

```bash
sudo apt-get update
sudo apt-get upgrade -y
sudo apt-get install -y build-essential cmake unzip pkg-config
sudo apt-get install -y libxmu-dev libxi-dev libglu1-mesa libglu1-mesa-dev
sudo apt-get install -y libjpeg-dev libpng-dev libtiff-dev
sudo apt-get install -y libavcodec-dev libavformat-dev libswscale-dev libv4l-dev
sudo apt-get install -y libxvidcore-dev libx264-dev
sudo apt-get install -y libgtk-3-dev
sudo apt-get install -y libopenblas-dev libatlas-base-dev liblapack-dev gfortran
sudo apt-get install -y libhdf5-serial-dev graphviz
sudo apt-get install -y python3-dev python3-tk python-imaging-tk
sudo apt-get install -y linux-image-generic linux-image-extra-virtual
sudo apt-get install -y linux-source linux-headers-generic
```

### Install NVIDIA-DRIVER

```bash
sudo apt-get purge nvidia*

sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update

ubuntu-drivers devices

sudo apt-get install -y nvidia-driver-460
```

### reboot server
```/bash
sudo reboot
```

### check nvidia-smi
```bash
nvidia-smi
```

### install cuda 11.2

```bash
wget https://developer.download.nvidia.com/compute/cuda/11.2.2/local_installers/cuda_11.2.2_460.32.03_linux.run
sudo sh cuda_11.2.2_460.32.03_linux.run
```

- **DO NOT check the option of installing the driver!!!**
- example) Driver - 460 체크 해제!!! 
해제를 하는 이유는 cuda 설치 시 nvidia driver 같이 설치 하게 되었 있는데설치 할때 위의 해당 커널버전에서는 nvidia-driver 버전이 설치가 안되는 이슈가 있어서 수동으로 설치 하기 위해서 체크를해제 했다.
이런 부분에 있어서 cuda는 설치 부터 너무 어렵다.

### Set environmental

```bash
sudo nano ~/.bashrc

export PATH=/usr/local/cuda-11.2/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-11.2/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
export CUDA_HOME=/usr/local/cuda
```
### check

```bash
nvidia-smi
nvcc -V
```
![nvcc](https://images.velog.io/images/hong-brother/post/fb3a2492-dfd2-40e2-8fc7-7d80d9709b9e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-06%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2012.05.45.png)

![nvdiai-smi](https://images.velog.io/images/hong-brother/post/54fca618-16d9-4b3b-81a0-30cd460bce5b/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-06%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2012.05.18.png)
