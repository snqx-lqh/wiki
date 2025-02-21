Jetson TX2 NX
![](image/Pasted%20image%2020240105102854.png)

亚博智能买的板子，出厂自带系统，用户名：jetson 密码：yahboom

## 摄像头测试

写入命令：nvgstcapture-1.0 ，摄像头就会起来了

USB摄像头
```bash
sudo apt-get install camorama

camorama
```
## 刷机

新设备建议刷机，主要是我的使用过程中遇到了一些pip非法指令的问题，

刷机使用SDK Manager[SDK Manager | NVIDIA Developer](https://developer.nvidia.com/sdk-manager)我刷的jetpack4.6.4版本

刷完机后移动文件系统到固态硬盘

```BASH
# 下载rootOnNVME 软件
git clone https://github.com/jetsonhacks/rootOnNVMe.git
cd rootOnNVMe/
chmod 777 copy-rootfs-ssd.sh 
chmod 777 setup-service.sh
./copy-rootfs-ssd.sh
./setup-service.sh
sudo reboot
```

然后将一些包进行升级

```
sudo apt-get update
sudo apt-get full-upgrade
```

## Cuda等安装（如果没有刷机下载的话）

使用JetPack

```bash
sudo apt install nvidia-jetpack
```

检测cudnn时遇到报错

fatal error: FreeImage.h: No such file or directory

解决方法是安装这个文件

```bash
sudo apt-get install libfreeimage3 libfreeimage-dev
```

## Cuda等检查

### 检查Cuda

安装完成后可在`/usr/local/`路径下查看有没有名为Cuda的文件

运行 `nvcc -V`查看，如果有文件但是没有信息的话

```BASH
sudo vim ~/.bashrc
```

在最后添加下面三行：（英文输入法下按下 i，进入插入模式，上下键让光标移动到最下面一行，然后复制以下三行，在光标处按下鼠标右键就会自动复制进去，然后按住 esc，输入冒号放开 esc，在输入 wq！强制保存退出）

```BASH
export CUDA_HOME=/usr/local/cuda-10.2
export LD_LIBRARY_PATH=/usr/local/cuda-10.2/lib64:$LD_LIBRARY_PATH
export PATH=/usr/local/cuda-10.2/bin:$PATH
```

注意:如果添加上面三句还是显示 nvcc not found，首先，查看 cuda 的 bin 目录下是否有 nvcc：

```BASH
cd /usr/local/cuda/bin
```

如果存在，把上面两句减少为下面两句，vim ~/.bashrc 进入配置文件； 添加以下两行：

```BASH
export PATH=/usr/local/cuda/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
```

然后保存退出，然后需要 source 下生效下。

```BASH
source ~/.bashrc
```

source 后，此时再执行 nvcc -V 执行结果如下

### 检查 cuDNN

系统中已经安装好了 cuDNN，并有例子可供运行，我们运行一下例子，也正好验证上面的CUDA

```BASH
cd /usr/src/cudnn_samples_v8/mnistCUDNN/
#进入例子目录 
sudo make
#编译一下例子
./mnistCUDNN # 执行
如果以上无法运行可以添加权限如下方法：
sudo chmod a+x mnistCUDNN # 为可执行文件添加执行权限
```


## jtop检查设备信息

jtop可以显示很多的运行信息

```BASH
sudo apt-get install python3-pip
sudo -H pip3 install jetson-stats
sudo systemctl restart jtop.service
sudo reboot
jtop
```

## 增加和删除交换分区

### 增加空间

第一行的6G就是你想要增加的SWAP容量。

```BASH
sudo fallocate -l 6G /var/swapfile
sudo chmod 600 /var/swapfile
sudo mkswap /var/swapfile
sudo swapon /var/swapfile
sudo bash -c 'echo "/var/swapfile swap swap defaults 0 0" >> /etc/fstab'
```

### 删除空间

打开之前设置好的启动挂载的fstab文件 

```BASH
sudo vi /etc/fstab
```

注释掉 

```bash
#/var/swapfile swap swap defaults 0 0
```

重启 reboot

启动之后 `sudp rm -rf /var/swapfile `这样以后增加的swap就去掉了

## Conda安装

TX2不能使用最新版的miniconda，所以下载安装miniforge[conda-forge/miniforge: A conda-forge distribution. (github.com)](https://github.com/conda-forge/miniforge)
选择aarch64的版本，下载下来

```BASH
cd ~
bash Miniforge3-Linux-aarch64.sh
```

中间需要填写的全填yes。安装完成后关闭当前shell，打开一个新的shell，不然不能使用。

卸载的话，按照如下步骤

```BASH
rm -rf ~/miniforge3/
```

然后删除~/.bashrc中和miniforge相关的文件

## 添加Conda清华源

不知道为什么，复制下来有时候有问题，输入问题？如果遇到问题，官网复制指令再运行[Oh My TUNA](https://tuna.moe/oh-my-tuna/)

```BASH
wget https://tuna.moe/oh-my-tuna/oh-my-tuna.py

# 自己用
python oh-my-tuna.py

# 全部用户
sudo python oh-my-tuna.py --global

# Get some help
python oh-my-tuna.py -h
```

2024_12_05：上面这个好像有点问题，在用户目录下建立.condarc，把下面这些源复制进去，清华源会一直更新，如果这个也不可以的话就去官网[清华源Anaconda](https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/)查看最新的解决方案。

```
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

## PiP 换源

在 user home 目录下新建一个 ~/.pip/pip.config 文件，添加如下配置：

```bash
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

如果想要临时更换 pip 源，则可以在相应的 pip 命令调用时，在命令的最后加上：

```bash
-i https://pypi.tuna.tsinghua.edu.cn/simple
```

除了清华源，还有

```BASH
https://mirrors.aliyun.com/pypi/simple    #阿里源
https://pypi.mirrors.ustc.edu.cn/simple/  #中科大源
http://pypi.douban.com/simple             #豆瓣源
```
## 换软件源

系统有时候访问官网会访问不了，所以可以试着换成国内的源，但是有可能会有bug，在能使用国外源的情况下就使用国外源，实在不行再换源。换源的[链接](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu-ports/)

换源需要修改的文件是`/etc/apt/sources.list`，可以复制一个备份，也可以修改的时候把原来的全部注释。修改成以下，注意，可能会和我写文章的时候不同，可以看换源链接。

```BASH
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-backports main restricted universe multiverse

# 以下安全更新软件源包含了官方源与镜像站配置，如有需要可自行修改注释切换
deb http://ports.ubuntu.com/ubuntu-ports/ bionic-security main restricted universe multiverse
# deb-src http://ports.ubuntu.com/ubuntu-ports/ bionic-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-proposed main restricted universe multiverse
# # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-proposed main restricted universe multiverse
```

中科大源

```BASH
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb http://mirrors.ustc.edu.cn/ubuntu-ports bionic main restricted universe multiverse
# deb-src http://mirrors.ustc.edu.cn/ubuntu-ports bionic main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu-ports bionic-updates main restricted universe multiverse
# deb-src http://mirrors.ustc.edu.cn/ubuntu-ports bionic-updates main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu-ports bionic-backports main restricted universe multiverse
# deb-src http://mirrors.ustc.edu.cn/ubuntu-ports bionic-backports main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu-ports bionic-security main restricted universe multiverse
# deb-src http://mirrors.ustc.edu.cn/ubuntu-ports bionic-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb http://mirrors.ustc.edu.cn/ubuntu-ports bionic-proposed main restricted universe multiverse
# deb-src http://mirrors.ustc.edu.cn/ubuntu-ports bionic-proposed main restricted universe multiverse

```

## nomachine

arm版本的下载地址[NoMachine - NoMachine for Arm](https://downloads.nomachine.com/linux/?id=30&distro=Arm)下载armv8的deb文件。

```bash
sudo dpkg -i nomachine_8.11.3_3_arm64.deb
```

如果没有屏幕，分辨率就会有问题，在远程桌面终端输入以下指令

```BASH
xrandr --fb 1280x720
```

修改完成后要关闭重新打开，不然鼠标是乱的

一般我习惯显示方式通过右上角进入nomachine的设置界面后调节display为Enable viewport mode

这里遇见过BUG，有问题再看：(gedit:10142): Gtk-WARNING : 16:33:54.182: cannot open display:

```BASH
export DISPLAY=:0
```

## python 非法指令 核心已转储

使用conda的时候，经常遇到的是非法指令，核心已转储

将`export OPENBLAS_CORETYPE=ARMV8`加入到`~/.bashrc`中

有一个方式，想尝试一下。https://blog.csdn.net/weixin_43710676/article/details/129222042

原因是[miniconda](https://so.csdn.net/so/search?q=miniconda&spm=1001.2101.3001.7020)的libcrypto.so.1.1文件出错，需要手动替换。


## deepstream（未实现）

https://docs.nvidia.com/metropolis/deepstream/6.0/dev-guide/text/DS_Quickstart.html

https://blog.csdn.net/djj199301111/article/details/123592402


```bash
## 1.  Prerequisites
sudo apt-get update
sudo apt install -y git python-dev python3 python3-pip python3.6-dev python3.8-dev cmake g++ build-essential \
    libglib2.0-dev libglib2.0-dev-bin python-gi-dev libtool m4 autoconf automake

# 2. Gst-python
cd /opt/nvidia/deepstream/deepstream-6.0/sources/apps/
git clone https://github.com/NVIDIA-AI-IOT/deepstream_python_apps.git
cd deepstream_python_apps/
git submodule update --init
sudo apt-get install --reinstall ca-certificates
cd 3rdparty/gst-python/
git checkout 1a8f48a
./autogen.sh PYTHON=python3
./configure PYTHON=python3
make
sudo make install

# 3. install pyds
cd ../../bindings/
mkdir build
cd build
cmake ..  -DPYTHON_MAJOR_VERSION=3 -DPYTHON_MINOR_VERSION=6 -DPIP_PLATFORM=linux_aarch64 -DDS_PATH=/opt/nvidia/deepstream/deepstream
make
sudo pip3 install ./pyds-1.1.0-py3-none-linux_aarch64.whl -i https://pypi.tuna.tsinghua.edu.cn

# 4. run sample
cd ../../deepstream_python_apps
mv  apps/* ./
cd deepstream-test1/
python3 deepstream_test_1.py ../../../../samples/streams/sample_qHD.h264

```