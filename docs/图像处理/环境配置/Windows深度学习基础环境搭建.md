## Cuda 和 Cudnn
后验证，如果是不做tensorrt或者onnx的部署，不需要安装Cuda和Cudnn，在安装pytorch的时候，自动就会安装相关的工具。

先在`cmd`环境使用`nvidia-smi`指令查看cuda支持的最高版本，我在下载40系显卡的cuda的时候还有最低版本的要求，网上说是11.8，但是自己未测试，所以不知道。

```BASH
nvidia-smi

C:\Users\LQH>nvidia-smi
Thu Dec  5 13:25:49 2024
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 551.76                 Driver Version: 551.76         CUDA Version: 12.4     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                     TCC/WDDM  | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  NVIDIA GeForce RTX 4060      WDDM  |   00000000:01:00.0  On |                  N/A |
|  0%   38C    P8             N/A /  115W |    1029MiB /   8188MiB |      0%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+
```

我这里的Cuda Version是12.4版本，所以需要下载不能超过12.4版本的Cuda以及对应版本的Cudnn。

然后去[Cuda](https://developer.nvidia.com/cuda-toolkit-archive)官网和[Cudnn](https://developer.nvidia.com/rdp/cudnn-archive)官网下载自己适合的版本，cudnn下载的是个压缩包，然后记得和Cuda下载相应版本。

**注意**：下载后安装Cuda，安装选择自定义，如果是第一次安装，全选即可，但是可能会报一个没有vs的错，如果你的电脑装了Visual Studio就不用管，没有装的话就把Cuda中的这个与Visual Studio有关的选项取消勾选安装。

安装完成后可以使用`nvcc -V`查看是否安装完成。

```BASH
nvcc -V

C:\Users\LQH>nvcc -V
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2023 NVIDIA Corporation
Built on Mon_Apr__3_17:36:15_Pacific_Daylight_Time_2023
Cuda compilation tools, release 12.1, V12.1.105
Build cuda_12.1.r12.1/compiler.32688072_0
```

解压Cudnn的压缩包，将里面的3个文件复制到我们安装的cuda文件夹目录下，默认是`C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.1`，然后添加以下文件路径到环境变量。不要重复添加，因为有的环境路径可能在你安装Cuda的时候就安装了。

```BASH
C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.1\bin
C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.1\include
C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.1\lib
C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v12.1\libnvvp
```

## Miniconda安装

anaconda比较臃肿，直接安装[miniconda](https://docs.anaconda.com/free/miniconda/index.html)就行，官网下载太慢的话可以去[清华镜像源Miniconda镜像](https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/)选择指定版本的Conda镜像。

下载过程中就选择一下安装目录，记得在选择使用对象的时候选用他推荐的，就是仅个人使用那个选项，不要选择全局安装，那样每次都要用管理员权限。这个部分往后的教程，都是写的选择个人使用的方案。注意安装路径最好不要在C盘。

安装完成后，打开`Miniconda自带的终端软件`，可以使用以下命令创建新环境

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

2024_12_05:现在文档的步骤是

```BASH
conda config --set show_channel_urls yes
```

就会在用户目录下如`C:\Users\<YourUserName>\.condarc`生成一个`.condarc`，然后将该文件打开，加入以下内容

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

在刚建立好的conda的环境下，使用以下指令进行pip的换源。刚刚换的源是Conda的源，但是我们很多时候可能使用的是pip来进行安装下载。所以需要将pip的源换成国内源。

```BASH
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
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

由于很多地方需要数据集，但是部分项目如果使用全局变量可能需要修改源码的部分，所以使用软链接，用管理员方式打开`cmd`命令提示符，然后执行以下指令。

```BASH
mklink /D <目标路径> <源路径>

# 例如
mklink /d 要建立的文件 源文件
mklink /d E:\pysot\testing_dataset\OTB100   E:\dataset\OTB2015\OTB100
mklink /d E:\pysot\testing_dataset\VOT2018  E:\dataset\VOT2018
mklink /d F:\Project\WebBlog\MkDocs\docs    F:\Project\WebBlog\Repository\docs
```