+++
title = "综合工程:OpenWrt路由部署与软件编译"
date = 2023-08-12

[taxonomies]
tags = ["综合工程"]
+++


前言 openwrt 是一个自由的、兼容性好的嵌入式 linux 发行版。作为软路由玩家必备的一款神器，可以实现诸如去广告，多拨和科学上网等多种功能。本文以 openwrt 在X86平台的安装为例，介绍其部署流程。
<!-- more -->

## 选择合适的设备

无论是传统的无线路由器还是小主机都有成为openwrt路由的潜力。截止到今天，已经有20多个品牌（小米，华硕，锐捷，华三等）30多种架构（x86,ipq,bcm,mtd等）支持刷入openwrt；你可以在这个[网站](https://mao.fan/select)找到符合你预算和其他要求的，能刷机的路由器。
选择合适的系统
## 选择合适的系统
除了openwrt主线外，还可以选择：

- [iStoreOS](https://github.com/istoreos)iStoreOS是openwrt的一个分支，iStoreOS 提供了软件中心：iStore，以及较美观的界面和docker支持，对新手体验较好。

- [ImmortalWrt](https://firmware-selector.immortalwrt.org/) 是一个原版openwrt的分支，中文优化好，更新也勤快，内置镜像源可以直连下载&更新。

## 在X86小主机上面安装OpenWrt

无论是作为主路由或是旁路由，传统路由器由于主频低，内存小，并不适合作为软路由；而 NAS-软路由一体式 又有 all in boom 的风险，因此推荐X86平台作为物理机。当然，也可以采用 armbian 平台或是开发板，例如网心云老母鸡、树莓派等设备。截至本文撰写时间，二手平台上的价格不太利好：一台J1900平台的售价往往在200左右，而专门的多网口工控机价格在200到1000不等，树莓派更是成为了理财产品，需要慎重选择。

### 准备以下工具：

- 一个U盘与一台双网口物理机

- openwrt 的编译包，官方网站：[OpenWrt Firmware Selector](https://firmware-selector.openwrt.org/?version=24.10.0-rc2&target=ipq40xx%2Fgeneric&id=glinet_gl-a1300)

- 或者用由 eSir 大佬编译的[懒人包](https://drive.google.com/drive/folders/1uRXg_krKHPrQneI3F2GNcSVRoCgkqESr)


- PE 启动盘，这里推荐[HotPE](https://github.com/VirtualHotBar/HotPEToolBox)

- [img 写盘工具](https://www.roadkil.net/program.php?ProgramID=12#google_vignette)



### 安装流程：

1.进入PE环境：

- 打开微PE，将其安装进U盘中，安装完成后将 img 工具和 openwrt 包一起放进去；
- 将U盘插入目标主机，进入 BIOS-boot 设置U盘优先启动，各主板进入 BIOS 的按键不同，不确定的话建议都试一遍。

2.格式化硬盘并写盘

- 进入PE环境中，可以看到存在名为“分区助手”的软件，打开它并将目标主机硬盘格式化；注意不要分区！不要分区！不要设置文件系统！否则后续可能无法编译！点击左上角提交并执行
- 打开img写盘工具，将openwrt包写入硬盘，注意不要写进U盘里。

3.进入配置界面

- 重启系统并快速拔出U盘，避免重新进入PE；这时系统开始运行了。注意Esir固件是不跑码的，无需担心。- 一个U盘与一台双网口物理机
- 当看到 `please press Enter to activate this console`这个提示的时候系统就安装完毕了。可使用 passwd 命令设置密码。软路由将自动获取IP地址，随后我们在浏览器中打开该地址，即可看到 Lucl 界面。
> 硬盘空间有一部分没有被格式化，可以手动格式化为ext4并挂载。

> 注意初始IP往往是192.168.1.1，如果和光猫冲突需要在网络-接口中更改。

> 基本系统主题比较简陋，可以使用luci-theme-argon。

> 刷错主题无法打开luci：通过 SSH 登录路由器，切换到另一个已知正常的主题（例如 Bootstrap）： 
``uci set luci.main.mediaurlbase='/luci-static/bootstrap'
uci commit luci
/etc/init.d/uhttpd restart``
然后重新访问 Web 界面，查看是否恢复正常。

## 在arm架构的硬路由上面安装OpenWrt

相比X86平台，arm架构的设备兼容性不高，不能随便找一个包就能安装。以下是一般步骤：

- 首先得知道你的设备的CPU，比如ipq40XX系列，然后在对应的[仓库](https://archive.openwrt.org/releases/23.05.4/targets/)查看并下载包体。

- 当然也可以在[这里](https://firmware-selector.openwrt.org/)直接下载相关型号对应的固件，其中 Sysupgrade 映像是用来更新现有运行 OpenWrt 的设备，使用 Factory 映像在首次刷机时刷入。

- 随后开启Telnet或者SSH或者TTL串口连接到路由器，将对应的Uboot刷入，如果没有适配的包就无法刷openwrt。

- 通过Uboot的网络界面刷入Factory包，随后就可以在后台（如192.168.1.1）进入openwrt的管理界面。



## 在ubuntu上编译openwrt的ipk包

这里以ubuntu环境为例，我们假设你有一台虚拟机或者WSL。

> ``注意编译不能使用Root用户！``

**随后安装编译依赖的各个包：**

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
随后下载我们**刷入openwrt的对应的SDK包**，如

```
git clone https://github.com/immortalwrt/immortalwrt.git
```

**下载和安装仓库信息**
```
./scripts/feeds update -a
./scripts/feeds install -a
```
**下载并选中我们需要编译的包,这里以inyn为例：**
```
git clone https://github.com/diredocks/openwrt-inyn.git ./package/inyn
make menuconfig
```

在 `menuconfig` 的命令行界面中，选中 `Network -> inyn` 将其首部调整为 `<M>` 表示按需编译，最后选中 `Save -> OK -> Exit` 保存配置信息，然后 `Exit` 退出配置。

**编译 inyn 软件包**
```
make package/inyn/compile V=s
## 如果不行则需要先编译工具链，即为 make j=4 ，j为CPU核数
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
> 新版本的openwrt（24.10）已经改用APK包管理器。

## 参考

- [Openwrt wiki](https://openwrt.org/zh/docs/start)
- [OpenWrt在线定制编译](https://openwrt.ai/?target=ipq807x%2Fgeneric&id=xiaomi_ax3600)
- [openwrt luci 页面无法访问 问题排查](https://www.cnblogs.com/tfel-ypoc/p/17226064.html)
- [超详细，多图，简单，OpenWRT 设置，IPV6配置](https://post.smzdm.com/p/axz6369w/)
- [保姆级-光猫改桥接-路由拨号-openwrt端口转发](https://blog.csdn.net/weixin_44548582/article/details/121064734)
