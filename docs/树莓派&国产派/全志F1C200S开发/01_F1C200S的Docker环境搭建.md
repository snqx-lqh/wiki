## 笔记参考

主要参考的文章

[peng-zhihui/Planck-Pi](https://gitee.com/peng_zhihui/Planck-Pi#1-f1cxxxs%E8%8A%AF%E7%89%87%E7%9A%84%E4%B8%8A%E7%94%B5%E5%90%AF%E5%8A%A8%E9%A1%BA%E5%BA%8F)

[小白自制Linux开发板](https://www.cnblogs.com/twzy/p/14865952.html)

https://wiki.luckfox.com/zh/Luckfox-Pico/Luckfox-Pico-SDK#21-docker%E7%AE%80%E4%BB%8B

## 硬件设计

硬件以开源

开源链接：https://oshwhub.com/from_zero/f1c200s-he-xin-ban

## 环境搭建

**注意**：处理文档都在宿主机下，只有编译才使用Docker环境

提前安装好docker，下载镜像的时候，去https://xuanyuan.me/blog/archives/1154?from=tencent，关注加速列表。

根据官网一步步走下来就可以了

[Install Docker Engine on Ubuntu | Docker Docs](https://docs.docker.com/engine/install/ubuntu/#set-up-the-repository)

### 将用户添加到 Docker 组

在Ubuntu系统上，需要在运行Docker命令时添加 sudo 是因为 Docker 守护进程默认以 root 用户权限运行。Docker 执行一些需要特权权限的操作，如访问系统资源、管理容器、构建镜像、挂载文件系统等，以确保安全性和系统稳定性。如果希望不每次都使用 sudo 运行 Docker 命令，可以将当前用户加入 docker 组。

 运行以下命令将当前用户添加到docker组中：

```BASH
sudo usermod -aG docker $USER
``` 

注销并重新登录，或者运行以下命令使更改生效（我这里运行命令没生效，重启才可以）：

```BASH
sudo service docker restart
newgrp docker
```

确保用户 lqh 已经成功添加到 docker 组。(自己输入自己的用户名)，可以使用以下命令检查：

```
id lqh
```

### 配置镜像环境

我这里选择的镜像源是docker.1ms.run，到时候不一定能用，注意关注上面的加速列表

下载ubuntu镜像

```BASH
sudo docker pull docker.1ms.run/ubuntu:20.04
```

启动一个交互式的容器，使用名为 "f1c200s-env"，并将本地主机上的 SDK 目录映射到容器内的 /home 目录，最后以 Bash shell 运行。

```BASH
#第一次运行docker环境
sudo docker run -it --name f1c200s-env \
-p 8022:22 \
-v /home/lqh/f1c200s_WorkSpace:/home \
docker.1ms.run/ubuntu:20.04 /bin/bash

#第二次运行docker环境
sudo docker start -ai f1c200s-env

#退出
exit
```

进入容器后需要换源，先安装一个nano。安装前需要软件库更新

```BASH
apt-get update
apt-get install nano
```

然后执行`nano /etc/apt/sources.list`，换成你当时想要的源，可以是[清华源](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)，或者其他源。下面是ubuntu20的清华源

```BASH
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse

deb http://security.ubuntu.com/ubuntu/ focal-security main restricted universe multiverse
# deb-src http://security.ubuntu.com/ubuntu/ focal-security main restricted universe multiverse

# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
# # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
```

换源完成后，更新

```BASH
apt-get update
```

这个时候应该会报一个错误`Certificate verification failed: The certificate is NOT trusted.`，证书的问题这时候执行以下操作。

```BASH
# 把之前换源的https换成http
nano /etc/apt/sources.list

# 更新一下
apt-get update

# 安装证书
apt-get install --reinstall ca-certificates

# 把之前换源的http换成https
nano /etc/apt/sources.list

# 更新一下
apt-get update
```

安装需要的软件

```BASH
apt-get install xz-utils nano wget unzip build-essential git bc swig libncurses5-dev libpython3-dev libssl-dev pkg-config zlib1g-dev libusb-dev libusb-1.0-0-dev python3-pip gawk bison flex 
```

安装编译工具链

```BASH
cd /home

wget http://releases.linaro.org/components/toolchain/binaries/7.2-2017.11/arm-linux-gnueabi/gcc-linaro-7.2.1-2017.11-x86_64_arm-linux-gnueabi.tar.xz
```

注意：GCC版本要大于 6；此处为获取交叉编译链为7.2.1版本，也可以自行下载其他版本。

将工具链压缩包解压：

```
 mkdir /usr/local/arm 
 tar -vxf gcc-linaro-7.2.1-2017.11-x86_64_arm-linux-gnueabi.tar.xz -C /usr/local/arm 
```

配置环境变量：

```
nano  ~/.bashrc
```

打开文件添加下面的变量：

```
export PATH=$PATH:/usr/local/arm/gcc-linaro-7.2.1-2017.11-x86_64_arm-linux-gnueabi/bin
```

使环境变量立即生效 ：

```
source ~/.bashrc 
```

查询版本，确认安装成功 ：

```
arm-linux-gnueabi-gcc -v
```

配置完成后可以commit容器到镜像方便未来部署：

```
sudo docker commit f1c200s-env f1c200s-env.image
sudo docker save -o f1c200s-env.image.tar f1c200s-env.image
```

## uboot编译

在docker环境下的/home，或者在之前主机的f1c200s_WorkSpace目录中，但是极大概率上不了github，建议用电脑git下来后再使用远程工具送上虚拟机，或者虚拟机挂梯子。

```BASH
git clone https://github.com/Lichee-Pi/u-boot.git -b nano-v2018.01
```

克隆完毕文件会保存在当前目录下，进入该目录，

```BASH
cd u-boot
```

在该文件夹下有很多分支，我们可以查看所有分支，使用如下命令：

```BASH
git branch -a
```

现在我们使用的是nano开发板，所以将当前分支切换到nano分支，命令如下：

```BASH
git checkout nano-v2018.01
```

默认的没有指定交叉工具链和架构，因此在编译之前需要指定交叉工具链和芯片架构，u-boot的交叉编译器在u-boot 的根目录下中的**Makefile**文件中定义了。打开文件找到**CROSS_COMPILE**变量，大概在246行，把原来的那几行注释掉，修改为如下：

```BASH
ARCH?=arm
CROSS_COMPILE?=arm-linux-gnueabi-
```

找到**licheepi_nano_defconfig** 和 **licheepi_nano_spiflash_defconfig**，前者表示为TF卡启动，后者表示从SPI 设备启动，因为我们做的小板只有从TF卡启动，所以我们需要使用 **licheepi_nano_defconfig** 。

```BASH
make  ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- distclean
make  ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- licheepi_nano_defconfig  
```

这样我们把**licheepi_nano_defconfig** 作为默认配置项。

接下来我们就可以用图形界面进行配置了，执行

```BASH
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- menuconfig 
```

主要设置两个参数。

1. bootcmd，主要用于描述控制Linux内核文件以及其他描述文件加载到内存中位置以及启动Linux内核系统等
2. bootargs，用于配制文件系统、串口信息等。

bootcmd

```BASH
load mmc 0:1 0x80008000 zImage;load mmc 0:1 0x80c08000 suniv-f1c100s-licheepi-nano.dtb;bootz 0x80008000 - 0x80c08000;
```

bootargs

```BASH
console=ttyS0,115200 panic=5 rootwait root=/dev/mmcblk0p2 earlyprintk rw
```

编译

```BASH
make  ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- -j4
```

这时候可能会出现的问题。

1、bash: ./tools/binman/binman: /usr/bin/python: 解释器错误: 没有那个文件或目录

解决方法参考：https://blog.csdn.net/Jun626/article/details/104332728

由于之前没有安装python2，所以报错。

```BASH
apt-get install python
```

2、scripts/dtc/pylibfdt/libfdt_wrap.c:149:11: fatal error: Python.h: 没有那个文件或目录

解决方法参考：https://blog.csdn.net/p1279030826/article/details/112888891

缺少当前版本的`python-dev`

```BASH
//查询当前版本 
python -V

//根据对应的python版本安装`python-dev`（我的版本是python2.7）
apt-get install python2.7-dev
```

3、./tools/binman/binman: 1: binman.py: not found

解决方法参考[Linux小白](https://whycan.com/t_7275.html)的更改后的uboot。他把./tools/binman/这个目录下的binman.py中的内容复制到了目录下的binman文件中去。

最后，就会编译成功，得到`u-boot-sunxi-with-spl.bin`，在目录下。

烧录到TF卡

只要将u-boot-sunxi-with-spl.bin烧录到tf卡的8k偏移处地址就可以了，烧录步骤如下：使用dd命令进行块搬移：

```BASH
sudo dd if=u-boot-sunxi-with-spl.bin of=/dev/sdb bs=1024 seek=8
```

该命令中：

 **if**    文件名：输入文件名，缺省为标准输入。即指定源文件。< if=input file >

**of**   文件名：输出文件名，缺省为标准输出。即指定目的文件。< of=output file >

**bs**  bytes：同时设置读入/输出的块大小为bytes个字节。

**seek**  blocks：从输出文件开头跳过blocks个块后再开始复制。

这里的输出文件(**of**)为主机电脑的/dev/sdb文件，也就是TF卡，这里也体现了Linux一切皆文件的思想。

## Linux内核

下载Linux5.7.1源码，下载后完成后，将代码复制到Ubuntu新建的用户中并解压。

https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git/refs/tags?h=v5.10.161

国内源

https://mirror.bjtu.edu.cn/kernel/linux/kernel/v5.x/

放到虚拟机里面，我们的f1c200s_WorkSpace目录下解压。

```BASH
tar -vxzf linux-5.7.1.tar.gz
cd linux-5.7.1
```

我们先下载荔枝派的配置[https://files.cnblogs.com/files/twzy/linux-licheepi_nano_defconfig.zip](https://files.cnblogs.com/files/twzy/linux-licheepi_nano_defconfig.zip)然后传到虚拟机，解压后放到arch/arm/configs/目录下

```BASH
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- linux-licheepi_nano_defconfig
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- -j4
```

还有些修改看参考的Linux小白中

## 文件系统

制作文件系统也有很多方式，如通过busyBox、Buildroot等工具制作。

本次使用Buildroot，制作过程相对简单，兼容性好，由于根文件系统制作比较简单。

进入buildroot官网[https://buildroot.org/downloads](https://buildroot.org/downloads)

这里选择buildroot2018.2.11版本，将下载好软件包传入Ubuntu系统中，然后解压并进入源码目录中，输入清理命令。主要用于初始化一些设置，命令如下：

中间的选择看[Linux小白](https://www.cnblogs.com/twzy/p/15355842.html)

```BASH
make FORCE_UNSAFE_CONFIGURE=1
```

此时在源码的**output/images**目录下有一个rootfs.tar，这个文件就是最终生成的根文件系统镜像

## HelloDrv

 写一个编译ko文件

```BASH
KERNELDIR := /home/linux-5.7.1/
CURRENT_PATH := $(shell pwd)
CROSS_COMPILE:= arm-linux-gnueabi-
ARCH = arm
obj-m := hello_drv.o

build: kernel_modules

kernel_modules:
    $(MAKE) ARCH=$(ARCH) CROSS_COMPILE=$(CROSS_COMPILE) -C $(KERNELDIR) M=$(CURRENT_PATH) modules
clean:
    $(MAKE) -C $(KERNELDIR) M=$(CURRENT_PATH) clean
```

## 常见问题

1、退出docker容器时出现there are stopped jobs如何解决？
原文链接：https://blog.csdn.net/sunmingyang1987/article/details/104349392

首先查看哪些进程没结束：
```
jobs -l
```

显示：

```
[1]+  1023 Stopped                 python3 test2_Linux3.py  (wd: /program/opencv)
```

然后用以下命令结束进程：

```
kill -9 1023
```

注：1023是进程ID
