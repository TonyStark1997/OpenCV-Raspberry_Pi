# Configure the OpenCV environment

在本篇文章中，我们将在Raspberry Pi3上配置带有python3.5的OpenCV3.4.0环境。

第三天的学习内容难度较大，而且在OpenCV编译的过程很漫长，所以建议大家仔细耐心的看好每一步和每一个操作，避免在配置环境中出现错误导致需要重新配置环境

***

## Step 0:拓展文件系统

***

树莓派默认没有使用SD卡上的所有存储空间，所以如果不拓展文件系统，在安装大型软件是会提示空间不足安装错误。

首先在终端内输入以下命令进入系统设置：

```bash
~ $ sudo raspi-config
```

然后按如下顺序选择：

> Advanced Options > A1 Expand filesystem > "Enter"键确认

此时会提示一条消息“The root partition has been resized”，即根分区已调整大小。使用以下命令可以看到存储空间的变化：

```bash
~ $ df -h
```

之后重启树莓派：

```bash
~ $ sudo reboot
```

## Step 1:释放一些存储空间

***

由于OpenCV库非常大，几乎有5G之多，所以要保证存储空间充足，推荐使用16G或16G以上的SD卡。

现在我们删除一些不必要的应用以节省存储空间:

```bash
~ $ sudo apt-get purge wolfram-engine
~ $ sudo apt-get purge libreoffice*
~ $ sudo apt-get clean
~ $ sudo apt-get autoremove
```

## Step 2:安装cmake

***

编译OpenCV需要使用Cmake进行编译，但是apt-get中的cmake版本较低无法进行编译，所以我们先到[cmake官网](https://cmake.org)下载最新版的camke或者直接用下面命令进行下载：

```bash
~ $ wget http://www.cmake.org/files/v3.12/cmake-3.12.2.tar.gz
```

之后解压：

```bash
~ $ tar -zxvf cmake-3.12.2.tar.gz
```

进入camke3.12.2文件夹并安装：

```bash
~ $ cd camke3.12.2
~ $ ./configure
~ $ make
~ $ sudo make install
```

## Step 3:安装依赖包

***

首先更新并升级所有现有软件包

```bash
~ $ sudo apt-get update
~ $ sudo apt-get upgrade
```

之后重启树莓派：

```bash
~ $ sudo reboot
```

重启之后打开终端，按照如下步骤安装相关软件和依赖包:

```bash
~ $ sudo apt-get install build-essential pkg-config -y
~ $ sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev -y
~ $ sudo apt-get install libxvidcore-dev libx264-dev -y
~ $ sudo apt-get install libgtk2.0-dev libgtk-3-dev -y
~ $ sudo apt-get install libatlas-base-dev gfortran -y
```

## Step 4:安装Python3、numpy库和相关依赖

***

安装Python3和numpy

```bash
~ $ sudo apt-get install python3 python3-setuptools python3-dev -y
~ $ wget https://bootstrap.pypa.io/get-pip.py
~ $ sudo python3 get-pip.py
~ $ sudo pip3 install numpy
```

## Step 5:下载OpenCV3.4.0和OpenCV contrib额外模块

***

使用以下命令下载OpenCV3.4.0和OpenCV contrib额外模块：

```bash
~ $ cd ~
~ $ wget -O opencv.zip https://github.com/Itseez/opencv/archive/3.4.0.zip
~ $ unzip opencv.zip
~ $ wget -O opencv_contrib.zip https://github.com/Itseez/opencv_contrib/archive/3.4.0.zip
~ $ unzip opencv_contrib.zip
```

## Step 6:编译并安装适用于Python3的OpenCV3.4.0

***

使用cmake进行编译

```bash
~ $ cd opencv-3.4.0
~/opencv-3.4.0 $ mkdir build
~/opencv-3.4.0 $ cd build
~/opencv-3.4.0/build $ cmake -D CMAKE_BUILD_TYPE=RELEASE \
-D CMAKE_INSTALL_PREFIX=/usr/local \
-D BUILD_opencv_java=OFF \
-D BUILD_opencv_python2=OFF \
-D BUILD_opencv_python3=ON \
-D PYTHON_DEFAULT_EXECUTABLE=$(which python3) \
-D INSTALL_C_EXAMPLES=OFF \
-D INSTALL_PYTHON_EXAMPLES=ON \
-D BUILD_EXAMPLES=ON\
-D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib-3.4.0/modules \
-D WITH_CUDA=OFF \
-D BUILD_TESTS=OFF \
-D BUILD_PERF_TESTS= OFF ..
```

## Step 7:交换空间大小获得更多虚拟内存

***

交换空间大小后可以使用树莓派四个核心一起编译而不会出现内存问题。

打开 /etc/dphys-swapfile，并修改CONF_SWAPSIZE参数：

```bash
~ $ sudo vim /etc/dphys-swapfile
```

之后按“i”键进入输入模式，修改CONF_SWAPSIZE参数为1024:

```bash
# set size to absolute value, leaving empty (default) then uses computed value
# you most likely don't want this, unless you have an special disk situation
# CONF_SWAPSIZE=100
CONF_SWAPSIZE=1024
```

修改完后按“Esc”键推出输入模式，之后输入“:wq!”保存并退出。

然后通过以下命令使其生效：

```bash
~ $ sudo /etc/init.d/dphys-swapfile stop
~ $ sudo /etc/init.d/dphys-swapfile start
```

## Step 8:进行编译

***

使用四个核心编译：

```bash
~ $ make -j4
```

## Step 9:使用单核进行编译

***

如果由于内存问题而在编译时遇到任何错误，则可以使用以下命令仅使用一个内核再次编译：

```bash
~ $ sudo make clean
~ $ sudo make
```

## Step 10:安装构建

***

编译构建后，使用以下命令安装构建：

```bash
~ $ sudo sudo make install
~ $ sudo ldconfig
```

## Step 11:验证OpenCV构建

***

运行make install后，OpenCV + Python绑定应安装在/usr/local/lib/python3.5/dist-packages或usr/local/lib/python3.5/site-packages中。

当你你需要使用OpenCV库的时候你需要知道它的安装位置，在我的树莓派中，它是在dist-packages里。

可以使用ls命令来确认OpenCV库所安装的位置：

```bash
~ $ ls -l /usr/local/lib/python3.5/dist-packages/
```

你要寻找像cv2.so这样的名字，如果它不存在，那么就找一个像cv2.cpython-35m-arm-linux-gnueabihf.so这样的名字（名字以cv2开头，以.so结尾），这是因为可能由于Python3的Python绑定库中的一些错误而发生。

我们需要使用以下命令将**cv2.cpython-35m-arm-linux-gnueabihf.so**重命名为**cv2.so**：

```
~ $ cd /usr/local/lib/python3.5/dist-packages/
/usr/local/lib/python3.5/dist-packages/$ sudo mv /usr/local/lib/python3.5/dist-packages/cv2.cpython-35m-arm-linux-gnueabihf.so cv2.so
```

## Step 12:测试OpenCV安装是否成功

***

```bash
pi@raspberrypi:~ $ python3
Python 3.5.3 (default, Jan 19 2017, 14:11:04) 
[GCC 6.3.0 20170124] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import cv2
>>> cv2.__version__
'3.4.0'
```

如果此处没有问题则说明安装成功。

## Step 13:删除OpenCV压缩包

***

```bash
~ $ cd ~
~ $ rm opencv.zip opencv_contrib.zip
```

## Step 14:交换回空间

***

打开你的/etc/dphys-swapfile文件并修改CONF_SWAPSIZE参数：

```bash
~ $ sudo vim /etc/dphys-swapfile
```

将文件内的CONF_SWAPSIZE参数修改回100:

```bash
# set size to absolute value, leaving empty (default) then uses computed value
# you most likely don't want this, unless you have an special disk situation
CONF_SWAPSIZE=100
```

修改完后按“Esc”键推出输入模式，之后输入“:wq!”保存并退出。

然后通过以下命令使其生效：

```bash
~ $ sudo /etc/init.d/dphys-swapfile stop
~ $ sudo /etc/init.d/dphys-swapfile start
```

## Step 15:结束

***

至此，OpenCV的环境配置就算完成了。使用OpenCV做计算机图像处理还需要学习其他很多东西，包括numpy库、Picamera库和计算机视觉原理等内容，在之后的文章中我会主要讲解树莓派做计算机视觉的应用和各种处理方法，以上内容请自行学习，相关的官网内都会有教程，同时我也在翻译OpenCV的学习手册，希望大家多多关注！
