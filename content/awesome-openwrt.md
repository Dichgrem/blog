+++
title = "综合工程:OpenWrt 软路由部署与软件编译"
date = 2023-08-12

[taxonomies]
tags = ["综合工程"]
+++


前言 openwrt 是一个自由的、兼容性好的嵌入式 linux 发行版。作为软路由玩家必备的一款神器，可以实现诸如去广告，多拨和科学上网等多种功能。本文以 openwrt 在X86平台的安装为例，介绍其部署流程。
<!-- more -->
## 为什么是X86？

无论是作为主路由或是旁路由，传统路由器由于主频低，内存小，并不适合作为软路由；而 NAS-软路由一体式 又有 all in boom 的风险，因此推荐X86平台作为物理机。当然，也可以采用 armbian 平台或是开发板，例如网心云老母鸡、树莓派等设备。截至本文撰写时间，二手平台上的价格不太利好：一台J1900平台的售价往往在200左右，而专门的多网口工控机价格在200到1000不等，树莓派更是成为了理财产品，需要慎重选择。

## 在X86小主机上面安装OpenWrt

### 准备以下工具：

- 1.openwrt 的编译包，由 eSir 大佬编译的三个经典版本：
https://drive.google.com/drive/folders/1uRXg_krKHPrQneI3F2GNcSVRoCgkqESr
- 2.PE 启动盘，这里推荐微PE：https://www.wepe.com.cn/download.html
- 3.img 写盘工具：https://www.roadkil.net/program.php?ProgramID=12#google_vignette
- 4.一个U盘与一台双网口物理机

### 安装流程：

1.进入PE环境：

- 打开微PE，将其安装进U盘中，安装完成后将 img 工具和 openwrt 包一起放进去；
- 将U盘插入目标主机，进入 BIOS-boot 设置U盘优先启动，各主板进入 BIOS 的按键不同，不确定的话建议都试一遍。

2.格式化硬盘并写盘

- 进入PE环境中，可以看到存在名为“分区助手”的软件，打开它并将目标主机硬盘格式化；注意不要分区！不要分区！不要设置文件系统！否则后续可能无法编译！点击左上角提交并执行
- 打开img写盘工具，将openwrt包写入硬盘，注意不要写进U盘里。

3.进入配置界面

- 重启系统并快速拔出U盘，避免重新进入PE；这时系统开始运行了。注意Esir固件是不跑码的，无需担心。
- 当看到 `please press Enter to activate this console`这个提示的时候系统就安装完毕了。可使用 passwd 命令设置密码。软路由将自动获取IP地址，随后我们在浏览器中打开该地址，即可看到 Lucl 界面。

## 在ubuntu上编译openwrt的ipk包

首先需要ubuntu环境，可以是虚拟机或者WSL等。``注意以下操作不能使用Root用户！``
随后安装编译依赖的各个包：
```
sudo apt install python3-distutils-extra git gawk libncurses-dev build-essential binutils bzip2 diff find flex gawk gcc-6+ getopt grep install libc-dev libz-dev make4.1+ perl python3.7+ rsync subversion unzip which

sudo apt install -y build-essential python3-dev python3-setuptools swig \
    libmesa-dev libwayland-dev libgraphene-dev \
    gawk wget git-core diffstat unzip texinfo gcc-multilib \
    libncurses5-dev libncursesw5-dev zlib1g-dev \
    libssl-dev flex bison gperf libxml-parser-perl \
    python-is-python3 python3-pip gettext

sudo apt install -y swig
sudo apt install -y mesa-common-dev libwayland-dev libgraphene-1.0-dev
```
下载和安装仓库信息
```
./scripts/feeds update -a
./scripts/feeds install -a
```
下载并选中我们需要编译的包,这里以inyn为例：
```
git clone https://github.com/diredocks/openwrt-inyn.git ./package/inyn
make menuconfig
```

在 `menuconfig` 的命令行界面中，选中 `Network -> inyn` 将其首部调整为 `<M>` 表示按需编译，最后选中 `Save -> OK -> Exit` 保存配置信息，然后 `Exit` 退出配置。
编译 inyn 软件包
```
make package/inyn/compile V=s
```
## 常用命令:
```
# 更新软件列表
opkg update

# 更新所有 LUCI 插件
opkg list-upgradable | grep luci- | cut -f 1 -d ' ' | xargs opkg upgrade

# 如果要更新所有软件，包括 OpenWRT 内核、固件等
opkg list-upgradable | cut -f 1 -d ' ' | xargs opkg upgrade

```


## 参考

- [Openwrt wiki](https://openwrt.org/zh/docs/start)
- [OpenWrt在线定制编译](https://openwrt.ai/?target=ipq807x%2Fgeneric&id=xiaomi_ax3600)
- [openwrt luci 页面无法访问 问题排查](https://www.cnblogs.com/tfel-ypoc/p/17226064.html)
- [超详细，多图，简单，OpenWRT 设置，IPV6配置](https://post.smzdm.com/p/axz6369w/)
- [保姆级-光猫改桥接-路由拨号-openwrt端口转发](https://blog.csdn.net/weixin_44548582/article/details/121064734)
- [360T7刷机](http://www.ttcoder.cn/index.php/2023/07/11/p0/)