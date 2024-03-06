+++
title = "Android TV 折腾小记"
date = 2023-08-08

[taxonomies]
tags = ["android-TV"]
+++

## 前言 
由于AppleTV的高昂的售价和普通电视盒子广告的泛滥，一台开源、多功能的原生安卓电视盒子逐渐成为智能家居的必备神器。出于对IPTV、YouTube和家庭影院等需求，以及对一面赏心悦目电视墙的期待，这里分享Android TV （以下简称ATV）安装的一些要点。
<!-- more -->
## 准备工作：
1.一个ATV镜像，这里使用Tosathony制作的Android TV x86 9.0， 支持Android tv Remote，且可以下载Google Play Store 。https://pan.baidu.com/s/17eDDrf4WzWVmrc9hLw-c_w?pwd=a728

2.我们的老朋友Rufus写盘工具：https://www.423down.com/10080.html

3.Android tv Remote手机遥控器软件：https://android-tv-remote-control.en.softonic.com/android

4.Tiny ADB软件: https://androidmtk.com/tiny-adb-and-fastboot-tool#installer

5.一些可安装的软件：

当贝市场：https://www.dangbei.com/apps/

哔哩哔哩TV版：https://www.fenxm.com/104.html

kodi: http://www.kodiplayer.cn/

ATV Launcher: https://www.fenxm.com/592.html

安装流程：
## 一、写盘，BIOS启动
1.使用Rufus将下载好的ATV镜像写入U盘。



2.将U盘插到目标主机上，并设置BIOS-boot优先启动，不同设备进入BIOS的按键不同，大部分是F2或者DEL；

## 二、开始安装
1.boot成功后可以看到如下界面：



2.我们选择自动安装：



3.经过跑码后进入若干个选项，一路yes过去，文件系统选ext4;





随后运行ATV，并拔出U盘；



## 三、进入Google界面
由于国内网络环境问题，导致一些界面无法进入，可使用如下方法或全局科学。

1.如果卡在Google的logo界面或者动画比较缓慢，或者重启后无法进入ATV界面，需要在BIOS-Advanced-OS selection中将其设置为Windows 8.X或者Android。



2.然后可以看到PayPal界面，这里使用可以Ctrl+Alt+F1 进入命令行界面，随后输入

pm disable com.tosanthony.tv.networkprovider #注意空格
回车执行，随后按Ctrl+Alt+F7或F8回到图形界面。



3.下一步，我们可以看到自动更新界面，这里我们需要禁用它：

同样Ctrl+Alt+F1 进入命令行界面，随后输入

pm disable com.google.android.tungsten.setupwraith #注意空格
回车执行，随后按Ctrl+Alt+F7或F8回到图形界面。



4.此时会进入一个WiFi界面，如果你是使用网线直连就没有问题，或者用键盘连接家里的WiFi，作者因为工控机没有WiFi模块在这里卡了半天。

5.现在我们可以看到进入了ATV的桌面。



## 四、安装软件并设置桌面启动
1.首先我们在设置中找到“设置”>“设备首选项”>“关于”，然后在“构建”上点击几次以解锁“开发人员”选项，随后开启USB调试开关。





2.随后在设置 > 设备首选项 > 关于 > 状态中找到并记下IP 地址，然后用Tiny ADB连接上去，这里使用命令adb connect <IP 地址> 。 ，随后在ATV端授权连接；



3.接着使用命令adb install <path to android app.apk>将要安装的软件包上传，也可以将文件拖到命令提示符窗口上以复制其路径，回车确认。

附一些ADB常用命令：

adb reboot #将重启 Android 设备。

adb reboot recovery #将设备重新启动到恢复模式。

adb push <local> <remote> #将文件从您的 PC 复制到您的 Android 设备。

adb shell wm density <dpi> #改变显示器的像素密度。

adb kill server #切断 PC 和 Android TV 之间的连接。
4.如果存在一些软件无法安装，可开启ARM兼容层，具体方法为在 dl.android-x86.org/houdini/9_y/houdini.sfs 中下载得到houdini.sfs，把文件名改成houdini9_y.sfs，随后拷贝进U盘，进入命令行界面，输入 ls 找到storage目录，输入 cd storage 进入你的U盘，输入ls，查看你拷贝的 houdini9_y.sfs 文件，并复制到该目录下。

cp houdini9_y.sfs /system/etc

enable_nativebridge

reboot
5.安装一些软件包后我们发现需要代替掉ATV自带的桌面，从而形成海报墙的效果，这和linux的桌面环境切换有异曲同工之处。注意：替换前需要已经安装完成其他桌面！！！！（比如ATV Launcher）我们使用

pm disable-user --user 0 com.google.android.tvlauncher 
命令，禁用google默认的桌面。随后重启，即可看到如下海报墙：



## 后记
Android TV google官方TV库

https://github.com/googlesamples/leanback-showcase

智能电视，电视盒子开发SDK

https://github.com/boxmate/tvframe

选中框切换动画，适用于电视

https://github.com/EZJasonBoy/FocusChangeAnimation

仿泰捷视频最新TV版 Metro UI效果. 仿腾讯视频TV版(云视听•极光) 列表页

https://github.com/hejunlin2013/TVSample

tv常用效果控件，包括焦点、边框处理等

https://github.com/evilbinary/Tvwidget

Android tv，盒子，投影仪 控件

https://github.com/FrozenFreeFall/Android-tv-widget

TV 项目常用工具(焦点问题，适配问题等.)

https://github.com/genius158/TVProjectUtils
