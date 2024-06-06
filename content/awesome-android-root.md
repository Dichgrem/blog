+++
title = "综合工程:安卓刷机与root教程"
date = 2023-08-13

[taxonomies]
tags = ["综合工程"]
+++



前言 自安卓系统诞生以来，root 一直是玩机的必备过程。时至今日，在安卓定制系统日益完善的情况下，能 root 的机型越来越少，本文以小米手机为例，介绍 root 的具体方法。
<!-- more -->
## **一.什么是root**

这涉及安卓的权限系统。Andoird 系统是基于 Linux 内核的，其中的权限大致可以分为四级，即一般软件权限，用户权限，ADB 权限和超级管理员权限（su）。而所谓 Root 也就是使手机获得超级管理员的权限，但是出于种种原因，厂商默认不提供超级管理员的权限，因此，root的本质就是一个提权的过程。

## **二.为什么要root**

以权限系统为例，一般软件权限需要经过用户同意，即每次安装前出现的各种请求弹窗；而 ADB 权限常常用于开发者模式，可以调试一些比较深层的设置；至于 root 权限则为系统的最高权限，与 Windows 的 system 权限相当（比 administer 还高）。因此，当我们具备了 root 权限后，就可以实现许多功能，例如屏蔽广告，虚拟定位，安装 Google 框架和软件，满血运行CPU等等。

## **三.如何root**

首先我们要了解安卓系统的分区和启动。安卓的分区包括:

- recovery 分区，类似PC端的PE环境，手机上的恢复出厂设置即为从 recovery 恢复；

- cache 分区，保存系统最常访问的数据和应用程序。 擦除这个分区，不会影响个人数据，只是删除了这个分区中已经保存的缓存内容；

- boot 分区，类似PC端的MBR分区，用来引导系统启动，擦除后手机会卡在开机 logo 的界面；

- system 分区，包括操作系统与软件，vendor 定制文件与库文件等等，擦除后会卡在开机的动画界面；

- data 分区，存放用户数据和系统设置，擦除后不影响系统的运行。

手机启动阶段存在名为 bootloader 的程序，与 PC 端的 BIOS 类似，被称为 fastboot 模式，厂商一般会将其锁定。

早些年间，存在大量一键 root，kingroot 之类的软件，可以直接刷写 root 包，获得 root 权限，但成功率不高；

因此，现在主流的刷机步骤为

- 解开 bootloader，俗称解BL锁. 

> [BL锁原理参考](https://telegra.ph/BL%E9%94%81%E7%9A%84%E5%8E%9F%E7%90%86%E6%98%AF%E4%BB%80%E4%B9%88-05-29)

- 打开机器上的允许USB调试;

- 备份手机数据,即备份Data分区(可使用Neobackup或系统自带),字库/基带/官方固件,桌面样式截图;

- 下载该机器的官方原厂包以及要刷的第三方系统包;

- 提取以上两个包中的 boot.img 和 recovery.img 文件备份;

- 将机器与PC等设备连接,并进入 bootloader;

- 刷入第三方 recovery，比如大名鼎鼎的 TWRP,或者对应新系统的recovery.img；

- 进入 recovery 模式，清空原系统数据;然后刷入原系统作为底包，避免出现固件问题;

- 刷入新系统Zip包,随后重启,再次进入 recovery .

- 刷入 Magisk (面具)工具，随后重启进入桌面,安装 Magisk(apk),通过修补 boot.img 文件获得 root 权限；

- 安装 Magisk 模块和 Lsposed 框架（可在其中下载许多模块，推荐一键救砖，系统优化和 root 隐藏）

- 安装 Momo 软件检测系统环境是否正常。

> 如果旧系统上没有root权限无法直接备份Data分区,可以先用系统自带的备份,并下载好原版系统镜像以防止刷机失败.

## **四.哪些机型可以root**

看到这里很多小伙伴肯定跃跃欲试，不过在 2023 年的今天，能 root 的机型还是比较少。首先是最容易的一加和小米，可以申请官方解 BL 锁，需要等待7天；其次是联想，索尼等海外品牌，也比较容易；而 oppo 和 vivo 及其子品牌 iqoo 和 realme 有些是不行的；苹果的越狱在10代前是可以的，而华为全部机型都是不可以的，除非上万能的淘宝收费解锁，直接烧录芯片；至于三星，BL 锁一旦解开就会触发芯片物理熔断机制，无法使用 pay 以及升级系统，体验极差。因此，刷机有风险，root 需谨慎！刷机前要了解相应的厂商，考虑保修和变砖的问题！

> 截至2024年5月小米手机解BL锁已经收紧,条件非常苛刻.

## **五.具体操作流程**

1.笔者以 Redmi k30pro 5G 这款手机为例，首先我们进入手机设置界面，进入“我的设备”，在“全部参数”中找到“ MIUI 版本”，连续点击后开启开发者模式，随后在“更多设置”中开启 USB 调试， USB安装 功能。

2.随后下载大名鼎鼎的搞机工具箱http://jamcz.com/ （由B站up主晨钟酱出品），里面具有许多功能，包括进入各个模式，无极调速等等：
然后我们进入小米官网https://www.miui.com/unlock/download.html ，下载官方解锁工具，需要登陆小米账号并等待7天，随后即可解锁。
3.解锁完成后在https://mifirm.net/downloadtwrp/148 中下载对应的 TWRP 版本，注意 redmi 的海外名为 Poco 。
4.随后用数据线连接手机，用其中的一键刷写刷入 TWRP；随后下载 rom 包，可以选择原版，官改版，海外版和类原生版。相关链接：

``https://miuiver.com/``

``https://mi.fiime.cn/Android``

这里选择 ``https://c.mi.com/global/miuidownload/index``

下载时注意一并下载 boot.img 文件，作为 Magisk 的修补用。然后下载 Magisk 包，与 rom 一起存入TF卡或者U盘中。

Magisk : ``https://magisk.me/zip/``

**注意，由于本机型为新型AB分区**（``https://www.jianshu.com/p/b2726b304801``） 因此如果刷机失败，需要下载原厂包用以恢复AB分区，否则无法启动和安装rom。

5.通过搞机工具箱进入 recovery 模式，首先我们在wipe中清除Data、Cache两个分区，俗称“双清”，随后在高级清除选项中清除 Data、Cache、Dalvik Cache 和 System 分区，俗称“四清”。
6.清除完成后即可开始刷机。将 TF 卡或者U盘插入手机，在“安装”中选择 rom 包，右滑确认刷机；随后如法炮制，刷入 magisk.zip 包，不然会卡在开机 logo 界面，俗称“卡米”。
7.刷完之后重启，则会进入安装界面。注意：如果刷的是海外版的包，千万不能联网安装，否则会失败且变为国内版。

随后可以看到桌面环境

8.此刻我们将下载好的 boot.img 文件复制到手机上，打开 Magisk 软件，在其中选择修补一个文件，选中 boot.img，修复完成后可以看到超级用户一栏可以使用了，说明root完成。

> 截至2024年5月1日,该机已经刷入crdroid 10.4 类原生系统,非常丝滑,步骤同上,但需要刷入[新固件](https://xiaomifirmwareupdater.com/firmware/lmi/stable/V14.0.1.0.SJKMIXM/).

## **六.Root后的模块安装**

隐藏root的 shamiko 和 lsposed 框架,以及yc调度，李跳跳等等。

## **后记**

关于 root 的其他用途还有很多，这里就不一一列举，分享一些常用模块：

``https://sspai.com/post/68531``

## **参考**
 
 - [lineageos镜像](https://download.lineageos.org/devices/lmi/builds)
 - [lineageos教程](https://wiki.lineageos.org/devices/lmi/install/variant1/)
 - [机型介绍](https://wiki.lineageos.org/devices/lmi/variant2/)
 - [XDA-没有声音的问题解决](https://xdaforums.com/t/no-sound-issue-can-u-help.4479225/)
 - [XDA-更换内核](https://xdaforums.com/t/kernel-overclocked-no-gravity-2023-08-28-protonclang.4531497/)
