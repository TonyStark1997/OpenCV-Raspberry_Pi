# Configuring Raspberry Pi

第一天的学习内容为配置树莓派的一些基本设置，包括更改下载源、安装一些必要软件、调整显示器布局、设置地区时间等
***
## Step 0:准备材料清单
Raspberry Pi 3B
Camera V2
5V电源线和插头
16G内存SD卡
鼠标、键盘、显示器和Wi-Fi或网线

## Step 1:SD卡烧写系统
到raspberry pi官网下载最新版Raspbian系统镜像

![image1](https://raw.githubusercontent.com/TonyStark1997/OpenCV-Raspberry_Pi/master/Day_1/Image/屏幕快照%202018-09-08%20下午1.15.56.png)

点击网页上方Dowloads进入系统文件下载页面

![image2](https://raw.githubusercontent.com/TonyStark1997/OpenCV-Raspberry_Pi/master/Day_1/Image/屏幕快照%202018-09-08%20下午1.16.25.png)

选择右侧RASPBIAN系统，点击进入下载页面

![image3](https://raw.githubusercontent.com/TonyStark1997/OpenCV-Raspberry_Pi/master/Day_1/Image/屏幕快照%202018-09-08%20下午1.16.43.png)

在下载页面中有两个版本的RASPBIAN系统，其中左边的带有图形化界面的版本，右边的是不带系统界面的版本，我们选择左侧RASPBIAN STRETCH WITH DESKTOP版本系统镜像，点解Download ZIP进行下载IOS系统镜像文件

下载IOS系统镜像完成后，开始进行系统文件的烧写

推荐使用etcher作为系统镜像烧写软件，etcher是一个方便易用的跨平台系统镜像少些软件，可直接到[etcher官网](https://etcher.io)下载软件。

![image4](https://raw.githubusercontent.com/TonyStark1997/OpenCV-Raspberry_Pi/master/Day_1/Image/屏幕快照%202018-09-08%20下午1.40.50.png)

把SD卡插入读卡器中，将读卡器插到电脑，打开etcher，选择SD卡为烧写目标，选择下载好的rspbian系统镜像，进行一键烧写

![image5](https://raw.githubusercontent.com/TonyStark1997/OpenCV-Raspberry_Pi/master/Day_1/Image/屏幕快照%202018-09-08%20下午1.44.33.png)

烧写完毕后，把SD卡插入树莓派，通过HDMI链接好显示器，同时链接好鼠标和键盘，最后插上电源线，树莓派开机

**注意：如果显示器右上角有显示黄色小闪电标志则说明供电插头输出电流不够，需更换大电流电源插头，电源电流不足会导致系统外设不能正常工作甚至树莓派关机不能启动**

## Step 2：安装Vim编辑器
开机后首先安装Vim编辑器，以方便修改文件代码，raspbian自带的nano和vi不太好用，新手使用起来可能无法顺利进行编辑文本。开机后按Control+Alt+T快捷键打开终端，然后输入下面代码：
>sudo apt-get install vim

输入完按回车键执行命令,出现[y/n]后输入y确认安装，等待安装成功。

