+++
title = "OpenWrt 软路由部署"
date = 2023-08-08

[taxonomies]
tags = ["openwrt"]
+++


前言 openwrt 是一个自由的、兼容性好的嵌入式 linux 发行版。作为软路由玩家必备的一款神器，可以实现诸如去广告，多拨和科学上网等多种功能。本文以 openwrt 在X86平台的安装为例，介绍其部署流程。
<!-- more -->
## 为什么是X86？

无论是作为主路由或是旁路由，传统路由器由于主频低，内存小，并不适合作为软路由；而 NAS-软路由一体式 又有 all in boom 的风险，因此推荐X86平台作为物理机。当然，也可以采用 armbian 平台或是开发板，例如网心云老母鸡、树莓派等设备。截至本文撰写时间，二手平台上的价格不太利好：一台J1900平台的售价往往在200左右，而专门的多网口工控机价格在200到1000不等，树莓派更是成为了理财产品，需要慎重选择。

## 准备工作

- 1.openwrt 的编译包，由 eSir 大佬编译的三个经典版本：

https://drive.google.com/drive/folders/1uRXg_krKHPrQneI3F2GNcSVRoCgkqESr

- 2.PE 启动盘，这里推荐微PE：https://www.wepe.com.cn/download.html

- 3.img 写盘工具：https://www.roadkil.net/program.php?ProgramID=12#google_vignette

- 4.一个U盘与一台双网口物理机

安装流程：

## 一、进入PE环境：

1.打开微PE，将其安装进U盘中，安装完成后将 img 工具和 openwrt 包一起放进去；
![image-xgsv.webp](https://pic.dich.ink/1/2024/03/06/65e8665416902.webp)

2.将U盘插入目标主机，进入 BIOS-boot 设置U盘优先启动，各主板进入 BIOS 的按键不同，不确定的话建议都试一遍。
![image-uxcl.webp](https://pic.dich.ink/1/2024/03/06/65e8665ae2917.webp)
## 二、格式化硬盘并写盘

1.进入PE环境中，可以看到存在名为“分区助手”的软件，打开它并将目标主机硬盘格式化；注意不要分区！不要分区！不要设置文件系统！否则后续可能无法编译！
![image-lxgv.webp](https://pic.dich.ink/1/2024/03/06/65e8666fc739c.webp)
点击左上角提交并执行
![image-efkt.webp](https://pic.dich.ink/1/2024/03/06/65e8667fc956e.webp)
2.打开img写盘工具，将openwrt包写入硬盘，注意不要写进U盘里。
![image-otsu.webp](https://pic.dich.ink/1/2024/03/06/65e86669be5c8.webp)
## 三、进入配置界面

1.重启系统并快速拔出U盘，避免重新进入PE；这时系统开始运行了。注意Esir固件是不跑码的，无需担心。
![image-ogrk.webp](https://pic.dich.ink/1/2024/03/06/65e8666b11d79.webp)
2.当看到 `please press Enter to activate this console`这个提示的时候系统就安装完毕了。可使用 passwd 命令设置密码。软路由将自动获取IP地址，随后我们在浏览器中打开该地址，即可看到 Lucl 界面。
![image-ariu.webp](https://pic.dich.ink/1/2024/03/06/65e86687ef0fc.webp)
## 后记

开源世界还存在着 DD-WERT、Tomato 等系统。正如互联网的发展并非一帆风顺，OpenWRT 也出现过核心开发者出走，另立山头推出 LEDE 等波折，在18年 LEDE 与 openwrt 合并后，通过众多开发者的不懈努力， OpenWRT 有了现在丰富完善的生态。

``官方Wiki：https://openwrt.org/zh/start``
