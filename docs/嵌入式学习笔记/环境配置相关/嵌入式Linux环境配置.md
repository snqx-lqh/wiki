## 说明

在ubuntu环境下运行，使用虚拟机实现

VMware16 + WIN11 + Ubuntu18.04

## 虚拟机Ubuntu安装

1）安装时按住WIN键可以直接拖动，解决显示不全的问题，注意在框内点，不要点状态栏

2）直接最小安装，其他的都不安装

3）清除整个磁盘，也可以自建分区

如果自建分区，可以按照以下分区，这是虚拟机，如果是实机应该要一个efi分区

| 分区名   | 文件格式 | 分区类型 | 大小      |
| ----- | ---- | ---- | ------- |
| /boot | ext4 | 逻辑分区 | 512MB   |
| /swap | swap | 逻辑分区 | 虚拟机内存x2 |
| /     | ext4 | 主分区  | 剩下的     |

4）填完电脑信息后开始安装

5）等待安装完成，重启，去掉镜像文件

6）安装vmtool工具，20.04好像自己安装了，但是我是安装的18.04，他不能直接使用，而且官方的也不能用，需要安装一个开源库。但是默认库好像没有，需要换源成清华源或者其他的，换完源后`sudo apt-get update`。然后安装开源工具。

```bash
sudo apt-get autoremove open-vm-tools 
```

```bash
sudo apt-get install open-vm-tools
```

```bash
sudo apt-get install open-vm-tools-desktop
```

```bash
reboot
```

至此，基本安装完成。

## 安装常用库并进行基础配置

安装网络工具，vim和git

```BASH
sudo apt-get install net-tools vim git build-essential gcc g++ make zlib* libffi-dev
```

安装ssh工具，并且打开ssh

```bash
sudo apt-get install openssh-server
sudo systemctl start ssh
```

ubuntu更改主目录文件名为英文

在终端中输入以下命令

```bash
export LANG=en_US
xdg-user-dirs-gtk-update
```

在询问是否将目录转化为英文的窗口中选择同意，使用命令将系统语言转化为中文

```bash
export LANG=zh_CN
```

重启系统，在登录的时候会提示是是否把英文目录转化为中文，选择不同意，并勾选不再提示

## 软件工具
### RaiDrive文件传输

1）进行ubuntu虚拟机和windows间的文件传输，安装RaiDrive软件默认安装即可，选择一下安装路径即可。

2）安装完成后，点击上面的settings，切换为中文语言，然后把启动时自动运行取消勾选，其他看自己情况。

3）添加链接信息，点击上面的添加，然后服务类型选择Nas，然后选择SFTP，把只读取消掉，启动RaiDrive后自动连接取消，sftp://后面填上你的ip地址，然后账户和密码就填写你的用户和密码即可。

4）查看本地映射的ubuntu文件路径

### vscode安装

ubuntu18的vscode只支持到[1.85](https://code.visualstudio.com/updates/v1_85)

安装下载即可

字体修改

```
Consolas, 'Courier New', monospace
```

## 配置环境

### 交叉编译环境

使用gcc-linaro[Linaro Releases](https://releases.linaro.org/components/toolchain/binaries/)

选择版本安装，比如gcc-linaro-7.5.0-2019.12-x86_64_arm-linux-gnueabihf.tar

```BASH
sudo mkdir /usr/local/arm
sudo cp gcc-linaro-7.5.0-2019.12-x86_64_arm-linux-gnueabihf.tar /usr/local/arm/ -f
cd /usr/local/arm
sudo tar -vxf gcc-linaro-7.5.0-2019.12-x86_64_arm-linux-gnueabihf.tar

#配置系统环境
sudo vi /etc/profile
#添加以下指令
export PATH=$PATH:/usr/local/arm/gcc-linaro-7.5.0-2019.12-x86_64_arm-linux-gnueabihf/bin
sudo apt-get install lsb-core lib32stdc++6

#重启
reboot
#检查版本号
arm-linux-gnueabihf-gcc -v
```

### 配置网口

**开发板和电脑直连模式**

电脑虚拟机配置两个网络，一个NAT用来上网，一个桥接模式连接开发板，桥接网卡需要重新配置IP地址，Windows、Linux、开发板的IP地址要配置在同一网段下才能相互ping通。

开发板配置网络IP信息

```bash
vi /etc/network/interfaces

auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 192.168.5.9
netmask 255.255.255.0
gateway 192.168.5.1


reboot

```


**开发板和电脑连接到路由器**

虚拟机还是可以配置两个网络，一个NAT用来上网，一个桥接模式连接路由器，IP会自动分配。

开发板配置网络IP信息

```bash
vi /etc/network/interfaces

auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp

```

windos网络ping不通的话注意关闭防火墙

### NFS挂载

虚拟机NFS安装

在 Ubuntu 终端执行以下指令安装 NFS

```BASH
sudo apt-get install nfs-kernel-server
```

新建 NFS 共享目录，并给予 NFS 目录可读可写可执行权限。

```BASH
sudo mkdir -p /home/lqh/armlinux/nfs
sudo chmod 777 /home/lqh/armlinux/nfs/
```

执行以下指令打开 etc/exports 文件，进入 etc/exports 文件，在最后添加内容

```BASH
sudo vi /etc/exports

/home/lqh/armlinux/nfs *(rw,sync,no_root_squash)
```

修改完以后保存退出。执行以下指令重启 NFS 服务器。

```BASH
sudo /etc/init.d/nfs-kernel-server restart
```

执行以下指令查看 NFS 共享目录。

```BASH
showmount -e
```

在使用正点原子开发板提供的uboot的时候，默认启动的是协议2的形式，Ubuntu 18.04 nfs 默认为协议3和协议4。

若想要求 nfs 支持协议2，就在`/etc/default/nfs-kernel-server`末尾加一句： `RPCNFSDOPTS="--nfs-version 2,3,4 --debug --syslog"`

```BASH
sudo vim /etc/default/nfs-kernel-server

RPCNFSDOPTS="--nfs-version 2,3,4 --debug --syslog"
```

然后重启nfs

```BASH
sudo /etc/init.d/nfs-kernel-server restart
```


挂载方式

```bash
mount -t nfs -o nolock,vers=[版本] [目标IP]:[目标路径] [挂载的本机路径]

mount -t nfs -o nolock,vers=3 192.168.5.11:/home/lqh/linux/nfs /mnt
```

### TFTP挂载

执行以下指令，安装 xinetd。

```BASH
sudo apt-get install xinetd
```

查询/etc/下是否存在 xinetd.conf 文件，没有的话则自己新建一个。已经有 xinetd.conf 文件可以跳过。

```BASH
ls /etc/xinetd.conf
```

如果有的话，就跳过下一步，没有的话，执行以下指令。

```BASH
sudo vi /etc/xinetd.conf
```

然后填入以下内容

```BASH
# Simple configuration file for xinetd
#
# Some defaults, and include /etc/xinetd.d/

defaults
{
# Please note that you need a log_type line to be able to use log_on_success
# and log_on_failure. The default is the following :
# log_type = SYSLOG daemon info
}

includedir /etc/xinetd.d
```

新建 TFTP 目录，这里建立在`/home/lqh/linux` 目录下，目录名为 tftpboot。将 tftpboot 目录赋予可读可写可执行权限。

```BASH
mkdir -p /home/lqh/armlinux/tftpboot
sudo chmod 777 /home/lqh/armlinux/tftpboot/
cd /home/lqh/armlinux/
ls
```

执行以下程序安装 tftp-hpa 和 tftpd-hpa 服务程序

```BASH
sudo apt-get install tftp-hpa tftpd-hpa
```

执行以下指令打开 tftpd-hpa 配置文件，修改 tftpboot 目录为 TFTP 服务器工作目录。

```BASH
sudo vi /etc/default/tftpd-hpa
```

```BASH
# /etc/default/tftpd-hpa

TFTP_USERNAME="tftp"
TFTP_DIRECTORY="/home/lqh/armlinux/tftpboot"
TFTP_ADDRESS=":69"
TFTP_OPTIONS="--secure"
```

执行以下指令创建/etc/xinetd.d/tftp 配置文件。（如果没有 xinetd.d 这个目录，可以先自己手动创建）

```BASH
sudo vi /etc/xinetd.d/tftp
```

```BASH
server tftp
{
	socket_type = dgram
	wait = yes
	disable = no
	user = root
	protocol = udp
	server = /usr/sbin/in.tftpd
	server_args = -s /home/lqh/armlinux/tftpboot -c
	#log_on_success += PID HOST DURATION
	#log_on_failure += HOST
	per_source = 11
	cps =100 2
	flags =IPv4
}
```

注意` server_args = -s `后面要添加自己的 tftp 工作路径。

修改/添加 tftp 文件后，执行以下指令重启 tftpd-hpa。重启 xinetd 服务。

```BASH
sudo service tftpd-hpa restart

sudo service xinetd restart
```

## 指定版本工具包安装
### Cmake

去[官网下载](https://cmake.org/files//)对应版本的Cmake

我这里下载的是cmake-3.16.7-Linux-x86_64.tar.gz

命令行解压

```bash
tar -zxvf cmake-3.16.7-Linux-x86_64.tar.gz
```

将下载的Cmake连接到系统中  
一般将Cmake文件放在/opt 或 /usr 路径下, 这里选择/opt：

```bash
sudo mv cmake-3.16.7-Linux-x86_64 /opt/cmake-3.16.7
```

创建软连接

```bash
sudo ln -sf /opt/cmake-3.16.7/bin/*  /usr/bin/
```

重启

```bash
reboot
```

检查一下

```bash
cmake --version
```

## 一些小问题

### 虚拟机无网络

```BASH
sudo dhclient -v
```

### 依赖问题

```BASH
sudo apt-get -f -y install
```