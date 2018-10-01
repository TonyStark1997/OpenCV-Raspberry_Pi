# Configuring Raspberry Pi

第一天的学习内容为配置树莓派的一些基本设置，包括更改下载源、安装一些必要软件、调整显示器布局、设置地区时间等

更多内容请关注我的GitHub库：https://github.com/TonyStark1997，如果喜欢，star并follow我！

***

## Step 0:准备材料清单

***

Raspberry Pi 3B

Camera V2

5V电源线和插头

16G内存SD卡

鼠标、键盘、显示器和Wi-Fi或网线

## Step 1:SD卡烧写系统

***

到[raspberry pi官网](https://www.raspberrypi.org)下载最新版Raspbian系统镜像。

![image1](https://raw.githubusercontent.com/TonyStark1997/OpenCV-Raspberry_Pi/master/Day_1/Image/image1.png)

点击网页上方Dowloads进入系统文件下载页面。

![image2](https://raw.githubusercontent.com/TonyStark1997/OpenCV-Raspberry_Pi/master/Day_1/Image/image2.png)

选择右侧RASPBIAN系统，点击进入下载页面。

![image3](https://raw.githubusercontent.com/TonyStark1997/OpenCV-Raspberry_Pi/master/Day_1/Image/image3.png)

在下载页面中有两个版本的RASPBIAN系统，其中左边的带有图形化界面的版本，右边的是不带系统界面的版本，我们选择左侧RASPBIAN STRETCH WITH DESKTOP版本系统镜像，点解Download ZIP进行下载IOS系统镜像文件。

下载IOS系统镜像完成后，开始进行系统文件的烧写。

推荐使用etcher作为系统镜像烧写软件，etcher是一个方便易用的跨平台系统镜像少些软件，可直接到[etcher官网](https://etcher.io)下载软件。

![image4](https://raw.githubusercontent.com/TonyStark1997/OpenCV-Raspberry_Pi/master/Day_1/Image/image4.png)

把SD卡插入读卡器中，将读卡器插到电脑，打开etcher，选择SD卡为烧写目标，选择下载好的rspbian系统镜像，进行一键烧写。

![image5](https://raw.githubusercontent.com/TonyStark1997/OpenCV-Raspberry_Pi/master/Day_1/Image/image5.png)

烧写完毕后，把SD卡插入树莓派，通过HDMI链接好显示器，同时链接好鼠标和键盘，最后插上电源线，树莓派开机。

![image6](https://raw.githubusercontent.com/TonyStark1997/OpenCV-Raspberry_Pi/master/Day_1/Image/image6.png)

**注意：如果显示器右上角有显示黄色小闪电标志则说明供电插头输出电流不够，需更换大电流电源插头，电源电流不足会导致系统外设不能正常工作甚至树莓派关机不能启动**

## Step 2：安装Vim编辑器

***

开机后首先安装Vim编辑器，以方便修改文件代码，raspbian自带的nano和vi不太好用，新手使用起来可能无法顺利进行编辑文本。开机后按Control+Alt+T快捷键打开终端，然后输入下面代码：

```bash
～ $ sudo apt-get install vim
```

输入完按回车键执行命令,出现[y/n]后输入y确认安装，等待安装成功。

## Step 3:更改下载源

***

树莓派默认的下载源是外国的源地址，通常在apt-get命令进行下载时速度会比较慢，建议更换为国内的[清华源](https://mirrors.tuna.tsinghua.edu.cn)，资源丰富并且下载速度快。

在终端中输入以下命令：

```bash
~ $ cd /etc/apt/
```

进入到/etc/apt/目录下，之后使用vim修改sources.list文件，此文件为apt下载软件的地址源。

```bash
~ $ sudo vim sources.list
```

进入vim编辑器后按“i”键进入vim的输入模式(具体vim使用方法请自行了解)，之后删除文件内全部内容，并输入新地址源。

>deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ wheezy main non-free contrib

>deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ wheezy main non-free contrib

输入完成后按键盘“Esc”键，之后输入“:wq!”保存并退出vim编辑器。

之后在终端内输入以下代码更新源地址并升级树莓派内软件：

```bash
~ $ sudo apt-get update
~ $ sudo apt-get upgrade
```

**具体使用方法请参考[此网页](https://mirrors.tuna.tsinghua.edu.cn/help/raspbian/)**

## Step 4:进行一些自定义

***

完成上述步骤后树莓派既可以正常工作，你可以在进行一些自定义包括更改用户名和密码、更换壁纸和界面风格、安装中文输入法、更换时区、调整显示器分辨率等，具体过程请根据需求自行百度，这里不再赘述。

希望大家自行熟悉vim使用方法、Linux操作命令和Linux操作系统相关知识。

最后放一张我的配置后的树莓派界面。

![image7](https://raw.githubusercontent.com/TonyStark1997/OpenCV-Raspberry_Pi/master/Day_1/Image/image7.png)
