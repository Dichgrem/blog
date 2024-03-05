+++
title = "机械革命键盘失灵拯救记"
date = 2023-09-04

[taxonomies]
tags = ["keyboard"]
+++

前言
机械革命作为一款极高性价比的笔记本，其优惠的力度和问题不断的故障让玩家们爱恨交织；作者在其上安装Linux时遇到了键盘失灵的问题，为了避免更多人踩坑，故写本文。
<!-- more -->
一.故障情况
作者在一台锐龙平台的笔记本上安装Debian时，先在虚拟机中尝试安装，没有发现异常；随后在实体机安装时发现liveCD中键盘不可用，无法设置密码和主机名，在连接外接键盘后安装完成，却发现Fn快捷键仍然可用。

二.具体思路
根据机革一贯的“特性”，作者猜测是因为BIOS或者ACPI出现了故障，随后在翻阅论坛时发现是由于锐龙笔记本键盘中断描述与其他键盘不同：其为边缘敏 感、低电平有效的；而蛟龙16K机器键盘实际是边缘敏感、高电平有效（Edge ActiveHigh）。结合linux的特性，读取到边缘敏感、低电平有效的中断时，会认为BIOS有Bug，会直接当成边缘敏感、高电平有效进行处理。因此，键盘就没有了响应。

三.解决方法
既然是高低电平表述的错误，那么我们只需建立DSDT表副本，将其修改，然后让它优先启动，从而让键盘配置正常；另外还存在BIOS修复，内核编译的方法，比较复杂，这里不做说明。

# 首先建立一个DSDT文件夹：
sudo su
mkdir -p /home/dsdt

# 随后将系统的DSDT表读取到里面，并安装acpica-tools：
cat /sys/firmware/acpi/tables/DSDT > dsdt.dat
apt install acpica-tools
iasl -d dsdt.dat

# 进入vim编辑DSDT表，没有vim的先安装一个：
apt install vim
vim dsdt.dsl

# 搜索并替换 Device(PS2K) 下面的
IRQ (Edge, ActiveLow, Shared, ) 
为
IRQ (Edge, ActiveHigh, Shared, ) 

# vim搜索的方法为在命令模式下按下“/”，键入搜索字符并回车即可，按“n”跳转到下一处，“N”跳转到前一处，按: wq保存：

# 随后搜索DefinitionBlock，将其步进值增加一（十六进制），即更改
DefinitionBlock ("", "DSDT", 2, "ALASKA", "A M I", 0x01072009)
为
DefinitionBlock ("", "DSDT", 2, "ALASKA", "A M I", 0x0107200A)

# 关闭DSDT，并设置其优先启动
iasl dsdt.dsl
mkdir -p kernel/firmware/acpi
cp dsdt.aml kernel/firmware/acpi/
find kernel | cpio -H newc --create > acpi_override
cp acpi_override /boot/acpi_override
echo "GRUB_EARLY_INITRD_LINUX_CUSTOM=\"acpi_override\"" >>/etc/default/grub

# 更新并重启：
update-grub2
reboot
                      
不出意外，重启后键盘即可使用，若系统不同将apt替换即可。

