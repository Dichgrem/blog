+++
title = "综合工程:Win/linux双系统部署"
date = 2023-08-18

[taxonomies]
tags = ["Tech","Dualsystem"]
+++


前言 对于同时有着游戏和Linux环境需求的玩家来说，双系统似乎是其必经之路；而主流设备中两块的硬盘位也为双系统的安装提供了支持。本文以Revios+Garuda的安装为例介绍双系统的安装。

<!-- more -->

## **什么是Revios?**
Revios是一款经过精简和优化的Windows发行版（如果可以这么说的话），其特点为在**保证稳定性的情况下，禁用和删除一些系统服务，提高了速度；禁用系统大量隐私收集功能，保护了隐私；禁用部分功能组件，减少磁盘空间占用**。对于需要的组件可以自由添加回来。同生态位下还存在着诸如**AtlasOS、LTSC**等，但Revios在精简和功能性中做的平衡较好。如果你有**游戏需求，同时需要使用Adobe等专业软件**，那么这将是一个好的选择。

## **什么是Garuda？**
Garuda是一款基于arch的Linux发行版，具有高性能的zen内核、绚丽的桌面环境和自动创建快照的功能，在一众发行版（Endeavouros，Manjaro）中受到更多的喜爱（确信）。由于作者使用过其他发行版，故以此新系统为例。

## 准备工作：
- 我们的新伙计ventoy：**https://www.ventoy.net/cn/download.html**
- revios镜像：**https://www.revi.cc/revios/download/**
- Garuda镜像：**https://garudalinux.org/downloads.html**
- 一台主机，这里以我的垃圾佬鞋盒主机为例：


![image](ipfs://bafybeihv6uo7qnvbv2slhb3qygxlddnehczk2bse37wr4syy7h35ve75de)


> 具体步骤：ventoy挂载镜像，安装Revios，安装Garuda，设置引导顺序。

## **一、挂载镜像并安装**
1.我们可以发现这次我们有两个镜像需要安装，如果使用Rufus那么需要两次独立的写入；因此这里推荐使用ventoy工具,只要将镜像拖入盘内即可，可以同时部署多个镜像。


![image](ipfs://bafkreiaxytflocpss32qkwpkydwvq4fells5x2obworuxmw2lx5tvgluym)

2.进入BIOS,设置boot启动顺序，将USB设为首位，保存退出；


![image](ipfs://bafybeidov4dng6xq3itissintbi4f7zfjvuka33u3vysr3ogv76kh22tf4)


![image](ipfs://bafybeifh74xbrrayjbupeg7fu637blc2i6gked2pg4ewbcbstgfgg3h4c4)

3.重启后可以看到如下界面，我们选择revios，正常模式启动：


![image](ipfs://bafybeifcdawjoiozjfr6ri2btullxq2srj4gfmdqodfawfdblzsxmonrry)


可以看到一个经典的Windows安装界面，我们点击同意协议：


![image](ipfs://bafybeifvnevd7gj5vmiv5y6xrs2ftsvmt3ff7ove4g26cvtpuk5tdj5tay)


选择目标磁盘安装：

![image](ipfs://bafybeibah4akykdcurox32zw5nlwhu4iidarzzro7fpwnyhnqtkvfxsmp4)

随后进入设置环节：


![image](ipfs://bafybeigh76azpp6bx3zsrguyyjfbymnvzjasbplwgcuaozuqd56dbhbx44)


设置安全问题：

![image](ipfs://bafybeig7wgyynrupahmipqwywtnnawfmb5w4p2pzsdoznj6traxalfuzle)

![image](ipfs://bafybeidaq4bapgzr464bxoipjlyqi2x25tov2tcxcvfmwt24dpbj6nheny)

4.重启后进入系统，可以看到非常流畅，甚至在这台2G内存的主机上都毫无问题：



![image](ipfs://bafybeibk7fg5zz57mycm3zypmt2zqxnmuxawxv5uaee6r4loeotnjnqrrq)


系统自带Brave浏览器，以及一个控制中心，可以调节一些参数。


![image](ipfs://bafybeieei3dxysb73ir7ltsitdom3x7kc66tfer7ezegrydugjfdypcqgu)

## **二、安装garuda 并启动**
1.同样的ventoy我们选择garuda-正常启动，注意先以开源的驱动启动，不然可能会出问题：


![image](ipfs://bafybeihrhernqfaii3ny2y7nyrtw6o3k5etov7p2wryi2hdli52jkttauq)


跑码后进入欢迎界面，选择install garuda linux:

![image](ipfs://bafybeiest7aa6xhojjqotso72uv27nlrn76ijbuhawlfurezbqjc2nctyu)

设置语言，地区，键盘

![image](ipfs://bafybeif4pwywebc4p3czhuxpabsmtjubnqy7zaslsy2f7h4mky5tpzjsqm)


注意这里我们抹除整个磁盘，不要点错成另外一个Windows所在的盘（或者只有一个盘，选择并存安装，有风险），加密系统设置后每次开机得先输入此密码，随后设置以GRUB方式启动：

![image](ipfs://bafybeiexcsvci6cj5kde4ex6dw2e3owg4wpldubm5kxlgspdyt2ks6366q)


检查无误后开始安装:

![image](ipfs://bafybeihq5xht3jsjn2v6o76rmtzuadznsq2dc3fw3aaepda4ipyov2vz7a)

2.安装完成后重启，注意需要设置garuda 所在硬盘优先启动，随后可以看到如下界面，说明引导成功；这里是以Grub来进入Windows的boot manager。

![image](ipfs://bafybeic465dljbcmzwyj52fpukyvvhmt2obgjihvoood5kdqqqwu3ixmza)


## **后记**

不建议单盘双系统，还因为固态硬盘对两个不同文件系统的运行存在问题，不建议固态多分区也是如此。



