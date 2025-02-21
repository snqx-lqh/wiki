## 下载源码并配置依赖

下载Opencv和Contrib源码，记得版本下载一样的，我这里安装的是3.4.16版本。

Opencv安装路径[Tags · opencv/opencv (github.com)](https://github.com/opencv/opencv/tags?after=3.4.17)

Contrib安装路径[Tags · opencv/opencv_contrib (github.com)](https://github.com/opencv/opencv_contrib/tags?after=4.5.4)

安装依赖

```bash
sudo apt-get install build-essential
sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
```

将Opencv和Contrib安装并解压，将Contrib解压到Opencv文件夹中。

```bash
sudo unzip opencv-3.4.16.zip
sudo unzip opencv_contrib-3.4.16.zip
```

## 编译源码

在Opencv目录下新建build文件夹，然后进入该文件夹，执行cmake和make，以及install

```bash
mkdir build
cd build/

cmake -D CMAKE_BUILD_TYPE=Release \  
-D OPENCV_GENERATE_PKGCONFIG=ON \  
-D CMAKE_INSTALL_PREFIX=/usr/local .. \ #想要把opencv安装到的路径  
-D OPENCV_EXTRA_MODULES_PATH=/home/lqh/opencv/opencv-3.4.16/opencv_contrib-3.4.16/modules .. # opencv_contrib的路径

make -j4

sudo make install
```

## 配置环境变量

```bash
sudo gedit /etc/ld.so.conf.d/opencv.conf
```

在空白文件下添加以下内容，这个应该就是前面opencv安装的路径加上/lib

```bash
/usr/local/lib
```

配置库

```bash
sudo ldconfig
```

更改环境变量

```bash
sudo gedit /etc/bash.bashrc
```

在文件后面添加

```bash
PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig  
export PKG_CONFIG_PATH
```

保存退出，终端输入

```bash
source /etc/bash.bashrc
```

测试

进入opencv/samples/cpp/example_cmake目录下，终端打开，依次输入：

```bash
cmake .  
make  
./opencv_example
```
