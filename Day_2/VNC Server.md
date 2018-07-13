# VNC remote control

第二天的学习内容为配置好VNC Server，通过Wi-Fi或网线用电脑远程控制树莓派并显示树莓派图形界面，这样的话可以大大减少树莓派的外设，包括鼠标键盘显示器等。
***
## Step 0:在电脑上安装VNC Server客户端
在[VNC Server官网](https://www.realvnc.com/en/connect/download/vnc/)   下载VNC Server电脑版客户端，根据自己电脑系统选择Windows版、MacOS版或者是Linux版客户端，完成安装。

## Step 1:在树莓派上启动VNC Server服务端
在树莓派上打开终端，输入以下命令启动VNC Server服务：
```bash
~ $ vncserver
```
运行完这个命令之后会显示出树莓派上VNC Server的一些版权和版本号等相关信息，重点是最后一行的括号内的内容，为一组IP地址，此地址就是这个树莓派的VNC Server连接地址。

## Step 2:在电脑VNC Viewer输入地址连接树莓派
打开电脑上的VNC Viewer客户端，在VNC服务地址栏中输入上面树莓派所获取的树莓派VNC Server连接地址的IP。输入之后会显示需要输入树莓派的用户名和密码，如果没有设置过用户名和密码的话，树莓派的默认用户名为'pi',默认密码为'raspberry'，输入完成后应显示连接成功，此时电脑的VNC Viewer客户端上显示树莓派的桌面界面。

在界面的最上方有一个细长的白色隐藏栏，点击其中栏内倒数第二个设置图标可以进行界面的设置，包括显示界面的画质等，如果网络质量不好可以选择中等画质。

## Step 3:设置树莓派VNC Server开机自启动

打开树莓派终端，输入:
```bash
~ $ pwd
```
命令确认现在地址处在'/home/pi'中，也就是打开终端的默认目录。

在此目录下，有一个隐藏的文件夹'.config',输入:
```bash
~ $ cd .config/
```
进入到'.config'文件夹中，然后输入：
```bash
~ $ mkdir autostart
```
创建一个名为autostart的文件夹，之后输入：
```bash
~ $ cd autostart
```
进入到刚刚创建好的autostart文件夹中，再输入：
```bash
~ $ touch realvnc.desktop
```
创建一个名为realvnc.desktop的文件，我们建在此文件中编写VNC Server自启动脚本。输入一下命令进入Vim编辑器进行文件编辑：
```bash
~ $ sudo vim realvnc.desktop
```
此时打开了这个空的脚本，单击键盘“i”键进入Vim的文本输入模式，之后在文本编辑器中输入以下代码内容：
```Bash
[Desktop Entry]
Type=Application
Name=RealVNC
Rxec=vncserver -geometry 1920x1080 :1
StartopNotify=false
```
输入完毕后点击键盘'Esc'键退出Vim的输入模式，之后输入以下命令保存并退出Vim文本编辑器：
```bash
:wq!
```
realvnc.dsktop文件中第四行的参数geometry后面的数字为设置连接树莓派的电脑屏幕分辨率，根据自己屏幕的分辨率选择不同的数字，其中的'x'为小写字母'x',而不是乘号。

完成以上所有步骤之后重启树莓派，此时电脑上的VNC Viewer断开连接树莓派，等待树莓派重启成功后重新在电脑上连接VNC Viewer，如若连接成果则说明则说明树莓派的VNC Server自启动脚本运行成功。如果没有连接成功则说明配置有问题，仔细检查脚本内的代码拼写和大小写是否有误。

至此，配置VNC Server进行远程控制树莓派的配置任务完成。

## 一些注意事项：
1. 停止树莓派VNC Server的命令为：
```bash
~ $ kill vncserver :1
```
冒号后面的数字为VNC Server的进程标号，树莓派可以同时开启多个VNC Server，在连接树莓派的时候输入正确的VNC Server进程标号即可。同样，在VNC Server开机自启动脚本中的第四行的数字也是进程标号。当遇到VNC Server启动不正确或者电脑客户端输入密码错误次数过多连接不上当前VNC Server进程时，可尝试更换进程标号进行连接。VNC Server进程标号改变时，连接VNC Server的IP码也跟着改变。

**多开VNC Server进程没有意义**

2. 树莓派3B的Wi-Fi支持百兆带宽网速，实际测试树莓派用网线连接网络比用Wi-Fi连接网络的网络速度要好很多，用Wi-Fi连接网络后打开的VNC Server服务可能会造成画面不流畅或卡顿的问题，建议用网线连接。
