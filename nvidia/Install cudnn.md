### cudnn이란?
- 이건 생략한다. AI쪽 개발자가 아니므로 이쪽 도메인지식이 없다. 근데 왜하느냐?
AI쪽 추론 코드를 FAST API로 랩핑해야 하는데 일단은 돌려봐야 하니깐 설치가 필요하다.
### cuda 버전에 맞는 cudnn 다운로드
tensorflow 에 사용하는(?) 필요한 cudnn을 찾아서 다운로드 하자 [다운로드](http://developer.nvidia.com/rdp/cudnn-download)

### 압축해제    
```bash
tar xvf cudnn-11.3-linux-x64-v8.2.1.32.tgz
```
    
### 파일 복사

```bash
cd cuda
sudo cp include/cudnn* /usr/local/cuda-11.2/include
sudo cp lib64/libcudnn* /usr/local/cuda-11.2/lib64/
sudo chmod a+r /usr/local/cuda-11.2/lib64/libcudnn*
```

### cudnn 설치 확인

```bash
cat /usr/local/cuda-11.2/include/cudnn_version.h | grep CUDNN_MAJOR -A 2
OR 
ldconfig -N -v $(sed 's/:/ /' <<< $LD_LIBRARY_PATH) 2>/dev/null | grep libcudnn

#define CUDNN_MAJOR 8
#define CUDNN_MINOR 2
#define CUDNN_PATCHLEVEL 1
--
#define CUDNN_VERSION (CUDNN_MAJOR * 1000 + CUDNN_MINOR * 100 + CUDNN_PATCHLEVEL)

#endif /* CUDNN_VERSION_H */
```

### 의존성 패키지 설치

```bash
sudo apt-get install libcupti-dev
sudo apt --fix-broken install
```
