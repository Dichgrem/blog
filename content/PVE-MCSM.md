+++
title = "PVE安装与MC服务器搭建"
date = 2023-08-11

[taxonomies]
tags = ["PVE","Debain"]
+++

前言
假期将至，不少家里有闲置设备的小伙伴想尝试开设一个我的世界（Minecraft）服务器，却不知从何下手。本文以PVE-Debian-MCSM为主线介绍其部署流程。
<!-- more -->
什么是PVE？

PVE (全称 Proxmox Virtual Environment) 是一款开源免费的虚拟化环境平台，同时支持KVM 虚拟机和 LXC 容器。它基于 Debian 和 KVM 技术开发，可在一台 PC 或服务器上同时运行Linux、OpenWRT、Windows 等，实现计算、网络、存储一体化解决方案，即所谓的“all in one”。类似的平台还有ESXi、Unraid等。



什么是MCSM？

MCSManager 面板（简称：MCSM 面板）是一款全中文，轻量级，开箱即用，多实例和支持 Docker 的 Minecraft 服务端管理面板。

此软件在 Minecraft 和其他游戏社区内中已有一定的流行程度，它可以帮助你集中管理多个物理服务器，动态在任何主机上创建游戏服务端，并且提供安全可靠的多用户权限系统，可以很轻松的帮助你管理多个服务器。



具体步骤：安装PVE并优化，开设虚拟机并安装Debian，安装mscm界面并开设实例，配置网络服务。

准备工作
1.PVE镜像，推荐使用7.4版本 https://www.proxmox.com/en/downloads

2.Debian镜像：https://mirrors.tuna.tsinghua.edu.cn/debian/dists/ （不建议使用DVD版，会出现奇怪的问题。）

3.我们的老伙计Rufus：https://www.423down.com/10080.html

4.Purpur1.19 服务端：https://purpurmc.org/

5.MC启动器HMCL：https://hmcl.huangyuhui.net/

6.Zerotier客户端：https://www.zerotier.com/download/

一.安装PVE并优化
1.将下载好的镜像用Rufus写入U盘。



2.将U盘插到目标主机上面，进入BIOS-boot设置启动顺序。这里我使用二手浪潮服务器X99主板，矿龙电源以及一块128G的SSD固态。注意：大部分服务器主板有机箱入侵检测机制，需要在说明书中找到特定针脚并用导电帽盖上，否则无法开机。

3.进入安装界面，选择install；



随后跑码，进入如下界面，同意协议：



设置硬盘与文件类型，可以选择ext4或者btrfs;



选择国家与地区，这里需要手打出China；



随后设置密码与邮件，邮件可以随便填：



然后设置网络连接，这里插网线就有地址；设置主机名，并记下内网IP地址；



检查无误后开始安装：



安装完成后reboot。

4.浏览器打开PVE地址，进入系统后我们需要给PVE换源。

首先，移除(备份)一下 PVE 原始的官方源 (将 sources.list 改名为 sources.list.bak)

mv /etc/apt/sources.list /etc/apt/sources.list.bak
添加国内 Debian 软件源：

nano /etc/apt/sources.list
改为

deb https://mirrors.ustc.edu.cn/debian/ bookworm main contrib

deb-src https://mirrors.ustc.edu.cn/debian/ bookworm main contribe

deb https://mirrors.ustc.edu.cn/debian/ bookworm-updates main contrib

deb-src https://mirrors.ustc.edu.cn/debian/ bookworm-updates main contrib
#编辑文件 pve-no-subscription.list

nano /etc/apt/sources.list.d/pve-no-subscription.list
#内容如下：

deb https://mirrors.tuna.tsinghua.edu.cn/proxmox/debian bookworm pve-no-subscription
屏蔽 PVE 企业源：

nano /etc/apt/sources.list.d/pve-enterprise.list
将下面这一行注释掉 (前面加上井号)：

#deb https://mirrors.tuna.tsinghua.edu.cn/proxmox/debian bookworm pve-no-subscription
更新测试：

apt-get update
二.新建虚拟机并安装Debian
1.找到local-btrfs(pve),在其中的ISO中上传下载好的Debian镜像；



2.随后创建虚拟机，选择Debian镜像并设置CPU核数与硬盘、内存大小；







3.一路确认后开机进入命令行界面，即可开始Debian安装。我们选择graphical install：



4.选择国家和语言，随后自动配置网络；



5.设置主机名，跳过域名设置；设置root账户名和密码、普通用户账户名与密码；



6.对磁盘进行分区，由于是虚拟机我们选择使用整个磁盘；





7.安装基本系统，随后将进入包管理器和大组件安装；



我们选择清华源，速度较快。注意：Debian安装时默认开启安全源，这个源是国外的所以下载速度极慢，因此还需要修改配置文件，这里使用Ctrl+Alt+F2 从图形界面转到tty命令终端, 键入 Enter，使用命令nano /target/etc/apt/sources.list，将所有的源都改成 http://mirrors.ustc.edu.cn 或者清华源，然后Ctrl+X 退出保存， Ctrl+Alt+F5回到图形界面。



下载需要一些时间，此时可以饮口茶先，随后看到如下界面：



由于是服务器所以不需要桌面环境：



安装grub引导：



随后安装完成，reboot后进入mscm的安装。

三.安装MCSM并开设实例
1.开机进入tty1界面



2.安装JAVA环境，不同版本的游戏的Java版本也不同。这里我们使用1.19版，需要安装Java18。

安装wget 和 下载Java18

apt install wget && wget http://img.zeruns.tech/down/Java/OpenJDK18U-jre_x64_linux_hotspot_18.0.1_10.tar.gz
创建安装目录

mkdir /usr/local/java/
解压当前目录下的JDK压缩文件

tar -zxvf OpenJDK18U-jre_x64_linux_hotspot_18.0.1_10.tar.gz -C /usr/local/java/
软链接程序到环境变量中

ln -sf /usr/local/java/jdk-18.0.1+10-jre/bin/java /usr/bin/java
测试是否安装正常，显示 openjdk version "18.0.1" 2022-04-19 则为正常

java -version
3.端口开发，面板需要 23333和24444 端口，游戏服务器默认端口是 25565。

在PVE-防火墙中打开它们。如果还是不行，执行如下命令：

systemctl stop firewalld

systemctl disable firewalld

service iptables stop
从而关闭防火墙。

4.安装面板，这里使用一键安装命令（注意该脚本仅适用于 AMD64 架构）

wget -qO- https://gitee.com/mcsmanager/script/raw/master/setup.sh | bash
执行完成后，使用 systemctl start mcsm-{web,daemon} 即可启动面板服务。使用 systemctl enable mcsm-{daemon,web}.service 实现开机自启。

5.在浏览器中打开该地址加上23333端口后缀，即可看到面板，账户为root，密码为123456。

6.新建实例，上传Purpur1.19 服务端,设置名称随后开启实例





7.随后我们可以在配置文件中设置游戏的相关选项，如关闭正版验证等。



8.大功告成，此时打开HMCL启动器，即可加入游戏。



四.配置联机网络
1.此时不要忘记需要和小伙伴们一起玩耍。如果你家里有公网固定IPv4或者IPv6，直接输入联机即可；如果没有公网IP，此时就需要进行内网穿透或者DDNS。

这里介绍一种名为zerotier的工具。

2.首先在 https://www.zerotier.com/ 注册并创建一个私有网络；

3.首先在虚拟机中安装curl命令支持

apt-get install curl
4.安装gnupg非对称信息加密系统，通讯所需必备软件

apt-get install gnupg
5.安装ZeroTier

curl -s https://install.zerotier.com/ | bash
安装成功后提示如下：

Success! You are ZeroTier address [ xxxxxxxxx ].
方括号内地址为类似于MAC地址。

6.设定开机自启动（分别执行如下命令）

systemctl start zerotier-one.service

systemctl enable zerotier-one.service
7.加入自己的私有网络

zerotier-cli join xxxxxxxxx
8.大功告成!此时只要让小伙伴们下载zerotier客户端并加入相同的私有网络，即可一起快乐联机！
