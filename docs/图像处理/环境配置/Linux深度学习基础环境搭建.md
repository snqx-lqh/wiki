## Cuda 和 Cudnn
后验证，如果是不做tensorrt或者onnx的部署，不需要安装Cuda和Cudnn，在安装pytorch的时候，自动就会安装相关的工具。

先在`cmd`环境使用`nvidia-smi`指令查看cuda支持的最高版本。

```BASH
nvidia-smi
```

我这里的Cuda Version是12.4版本，所以需要下载不能超过12.4版本的Cuda以及对应版本的Cudnn。

然后去[Cuda](https://developer.nvidia.com/cuda-toolkit-archive)官网和[Cudnn](https://developer.nvidia.com/rdp/cudnn-archive)官网下载自己适合的版本，cudnn下载的是个压缩包，然后记得和Cuda下载相应版本。

这里选择Cuda版本10.2，使用`sh *.run`的方式安装cuda。

```BASH
wget https://developer.download.nvidia.com/compute/cuda/10.2/Prod/local_installers/cuda_10.2.89_440.33.01_linux.run
chmod 777 cuda_10.2.89_440.33.01_linux.run
sudo sh cuda_10.2.89_440.33.01_linux.run
```

```BASH
是否同意条款，必须同意才能继续安装）
accept/decline/quit: accept

下一步弹出来的选项不选Driver

Installing the CUDA Toolkit（开始安装）
```

然后将环境加入环境变量

```BASH
sudo vim ~/.bashrc

export PATH="/usr/local/cuda-10.2/bin:$PATH"
export LD_LIBRARY_PATH="/usr/local/cuda-10.2/lib64:$LD_LIBRARY_PATH"

source ~/.bashrc
```

安装完成后可以使用`nvcc -V`查看是否安装完成。

```BASH
nvcc -V
```

Cudnn安装路径[cuDNN Archive | NVIDIA Developer](https://developer.nvidia.com/rdp/cudnn-archive)，下载对应版本，这里下载 8.2.1 for Cuda 10.2，有时候下载不了，可以复制链接到迅雷查看，点开后选择 cuDNN Library for Linux

下载好以后，接着用以下命令安装配置。

```BASH
tar -zxvf cudnn-10.2-linux-x64-v8.2.1.32.tgz

sudo cp cuda/include/cudnn.h /usr/local/cuda/include/ 
sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64/ 
sudo chmod a+r /usr/local/cuda/include/cudnn.h 
sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
```

查看cuDNN版本方法：

```BASH
cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
```

注意，这句话可能执行了没效果，那是因为新版本换位置了，需要用:

```BASH
cat /usr/local/cuda/include/cudnn_version.h | grep CUDNN_MAJOR -A 2
```

这里再次注意路径问题。

至此CUDN + cuDNN安装完成，可以执行相关训练文件查看是否有gpu信息输出，或监控一下gpu状态

```bash
watch -n 1 nvidia-smi
```

## 卸载Cuda

```BASH
sudo apt-get --purge remove "*cuda*" "*cublas*" "*cufft*" "*cufile*" "*curand*"  "*cusolver*" "*cusparse*" "*gds-tools*" "*npp*" "*nvjpeg*" "nsight*" "*nvvm*"

sudo apt-get autoremove
```
## Miniconda安装

anaconda比较臃肿，直接安装[miniconda](https://docs.anaconda.com/free/miniconda/index.html)就行，官网下载太慢的话可以去[清华镜像源Miniconda镜像](https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/)选择指定版本的Conda镜像。

安装的时候，直接`bash`你下载下来的对应的版本即可。最好放在用户目录下执行。安装过程应该就是输入yes和选择安装路径，其他的直接回车。

```BASH
bash Miniconda3-latest-Linux-x86_64.sh
```

安装完成后，重新打开`终端`，可以使用以下命令创建新环境

```BAHS
conda create -n test python=3.8
```

可以通过以下指令查看安装的环境

```BASH
conda env list
```

创建完成后使用以下命令跳转到该环境

```bash
conda activate test
```

要退出的话

```BASH
conda deactivate 
```

删除目标环境

```bash
conda env remove -n test
```

添加清华源，国外源下载包可能比较慢，所以我们查看清华源的文档，进行换源。[官方帮助文档](https://mirrors.tuna.tsinghua.edu.cn/help/anaconda)

2024_12_05:现在文档的步骤是找到用户目录下`${HOME}/.condarc`的`.condarc`文件，然后将该文件打开，没有就自己建立一个，加入以下内容

```BASH
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

即可添加 Anaconda Python 免费仓库。

一般安装python库的话，要么使用pip安装，要么使用conda安装，最好不要同时使用，相互间的依赖文件可能会冲突。但是有的时候，缺少一些库文件的时候，可以用pip把对应的包删除掉，再使用conda安装，可能会安装好。

## pip换源

在家目录下创建隐藏文件夹`.pip`，在隐藏文件夹`.pip`中创建`pip.conf`文件， 编辑pip.conf文件

```BASH
mkdir .pip
cd .pip
vim pip.conf

[global] 
index-url = https://pypi.tuna.tsinghua.edu.cn/simple/
```

## Pytorch环境安装

到[官网](https://pytorch.org/get-started/previous-versions/)选择指定版本安装即可，但是一般不推荐使用这种方式，因为国内连接经常失效。

可以直接到[whl/torch_stable](https://download.pytorch.org/whl/torch_stable.html)网页中安装对应的whl文件，然后再pip下载对应的whl，如果翻q可能会快一点。

一般我们会安装3个工具，torch、torchvision、torchaudio。版本间对应关系可以参考[博客](https://blog.csdn.net/shiwanghualuo/article/details/122860521)，他博客中的对应关系分别来自官方文档[torchvision](https://github.com/pytorch/vision#installation)和[torchaudio](https://pytorch.org/audio/main/installation.html#compatibility-matrix)，中的表格。自己选择项目对应的版本进行下载。

安装完成后测试

```python
import torch
print(torch.__version__)

print(torch.version.cuda)
print(torch.backends.cudnn.version())

print(torch.cuda.is_available())
```

## Opencv环境搭建

安装opencv

```bash
pip install opencv-python --verbose
```

安装opencv-contrib-python

```bash
pip install opencv-contrib-python --verbose
```

## 创建软链接

由于很多地方需要数据集，但是部分项目如果使用全局变量可能需要修改源码的部分，所以使用软链接，打开终端，然后执行以下指令。

```BASH
ln -s [源文件] [目标文件]
```