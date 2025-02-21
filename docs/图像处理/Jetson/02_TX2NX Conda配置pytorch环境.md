## 说明

为了不重复安装一些基础包，我创建一些基础的conda管理包，后面使用复制即可。

## TX2NX固件版本

jetpack4.6.4 miniforge

## 配置

创建虚拟环境

```
conda create -n pytorch python=3.6
```

pytorch 和 torchvision 需要官网下载whl安装。[PyTorch for Jetson - Jetson & Embedded Systems / Announcements - NVIDIA Developer Forums](https://forums.developer.nvidia.com/t/pytorch-for-jetson/72048)根据jetpack版本选择。

我下载的pytorchv1.10.0，然后找到torchvision [pytorch/vision: Datasets, Transforms and Models specific to Computer Vision (github.com)](https://github.com/pytorch/vision)在release中找到对应的版本记得和pytorch对应。

```
- PyTorch v1.0 - torchvision v0.2.2
- PyTorch v1.1 - torchvision v0.3.0
- PyTorch v1.2 - torchvision v0.4.0
- PyTorch v1.3 - torchvision v0.4.2
- PyTorch v1.4 - torchvision v0.5.0
- PyTorch v1.5 - torchvision v0.6.0
- PyTorch v1.6 - torchvision v0.7.0
- PyTorch v1.7 - torchvision v0.8.1
- PyTorch v1.8 - torchvision v0.9.0
- PyTorch v1.9 - torchvision v0.10.0
- PyTorch v1.10 - torchvision v0.11.1
- PyTorch v1.11 - torchvision v0.12.0
- PyTorch v1.12 - torchvision v0.13.0
- PyTorch v1.13 - torchvision v0.13.0
- PyTorch v1.14 - torchvision v0.14.1
- PyTorch v2.0 - torchvision v0.15.1
- PyTorch v2.1 - torchvision v0.16.1
```

然后安装pytorch

```C
sudo apt-get install python3-pip libopenblas-base libopenmpi-dev libomp-dev 
pip install 'Cython<3' 
pip install 'numpy<1.24' torch-1.10.0-cp36-cp36m-linux_aarch64.whl
```

然后安装torchvision

```BASH
sudo apt-get install libjpeg-dev zlib1g-dev libpython3-dev libopenblas-dev libavcodec-dev libavformat-dev libswscale-dev

# 从git下载相应版本，jetson无法下载的话，自己用windows下载传过来
git clone --branch v0.11.1 https://github.com/pytorch/vision torchvision   

cd torchvision
export BUILD_VERSION=0.11.1  
pip install 'pillow<7'
export OPENBLAS_CORETYPE=ARMV8
python setup.py install --user
cd ../  
```

## 检查是否安装成功

```python
import torch
print(torch.__version__)

print(torch.version.cuda)
print(torch.backends.cudnn.version())

print(torch.cuda.is_available()）
```

## 安装一些常用配置

### opencv

安装依赖

```bash
sudo apt-get install build-essential
sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
```

安装opencv

```
pip install opencv-python --verbose
pip install opencv-contrib-python --verbose 
```