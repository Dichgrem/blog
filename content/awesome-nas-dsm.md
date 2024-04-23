+++
title = "综合工程:黑群辉NAS极速安装"
date = 2023-08-17

[taxonomies]
tags = ["Tech","Nas"]
+++


前言 作为NAS家族中的重要一员，Synology的DSM以完善的服务和较高的售价闻名于世，因此，一般玩家倾向于工控机加黑群晖的方案组建自己的文件服务器。本文就黑群辉安装做了详细阐述。

<!-- more -->

准备工作：需要一个U盘，一台主机(包括屏幕与键盘)以及至少一块硬盘。
注意：
- 该U盘作为启动器，在安装完成后不可取下；
- 截至本文撰写的日期，蜗牛星际被黄牛抬价，可使用路由器工控机加硬盘的方案；
- 硬盘需机械硬盘，SSD阵列会出奇怪的问题。


![图片](ipfs://bafybeibaxr6eci2rgl3ft3hbcgwr4qi6amapiyxhf4l5aealn4rfpvqzmu)

> 安装流程：ISO镜像烧入，BIOS启动，选择版本编译安装，进入群辉界面安装DSM。

## **一、ISO镜像烧入**
首先我们从 https://www.52pojie.cn/thread-1745197-1-1.html 下载arpl-1.0-beta2.img文件，随后通过写盘工具（这里推荐Rufus）将该文件写入U盘中。Rufus：https://www.423down.com/10080.html

![图片](ipfs://bafkreiarjme4fe2z7bfjnrb5pjylbewllz2d4u3l6yhz3jfvcxsh27qyre)

注意U盘将被格式化，随后我们弹出U盘，并将其插到主机上面。

## **二、BIOS启动，编译安装**
开机并按F2或DEL键进入BIOS，设置boot顺序为U盘优先，随后可进入如下界面：

![图片](ipfs://bafybeia4z2sjvqogh4v36fj3v7hzhuxmfh3b53u7yc2jmaqclzgkwsodou)


记下Access后面的地址，随后在另一台主机的浏览器上访问，可以看到如下界面：

![图片](ipfs://bafkreih3i3vasmx6v5jjs2jmqzxn43gfz2vyvrqwjk6bxciqa6mmuc5bea)


选择DSM版本，这里我们以DS3615为例（注意DS918对网卡要求高，需确认好再选择），回车确认；
PS：群晖设备型号与套件架构参表.https://www.shenzhuohl.com/syno_list.html


![图片](ipfs://bafkreidetgtagebvqbi3yvdzbfsofdjyogzsowl7ctevxb3eziqvjlbk5a)

接着选择版本号，以42962为例（后面需下载对应的系统）



![图片](ipfs://bafkreictqk53a72xrx446nw4ii6dly53vf43sbjvspzqyre35ynu5x3exy)

然后输入一个序列号，由于是黑群晖所以让其随机生成。

![图片](ipfs://bafkreig3jngft4hy7bston2msao5eyg3cw4fp3mht5diks7lkinm6jhi7u)

随后build the loader,进入跑码界面，编译安装需要一些时间；

![图片](ipfs://bafkreibe4kvhwjrc22k7lb6ubfc2mnx3weqgcaq6iwp4zvezvwsunlfbs4)


完成后boot the loader,出现如下界面：

![图片](ipfs://bafkreic5i4cqovresmp5ljtfq3vztaygo3prucdw2fzagv67ra7d7ddpz4)



当出现内网地址时便成功了，记下版本号与对应的型号；

![图片](ipfs://bafkreiflqf2jvo2i6ri2x4ugnsre2ihsi23fhxx55r4bbpd2y5k5u4gyhy)

## **三、进入群辉界面安装DSM**
进入刚刚看到的内网地址（或用群辉助手https://finds.synology.com/#自动查找），可以看到如下界面：

![图片](ipfs://bafkreidnrukj4e5dagqapst5kplpzwa74vw3vbqvq3zrcgkeeuaevf76la)

从群辉的官网下载对应的系统，进入安装程序，这里有个重启时间为10分钟，此时可以饮口茶先。

![图片](ipfs://bafkreidoupg7ehmsdf2vmfzr4rqdc6qfe4vihgsfho45ug7myedxnhmiwq)

安装完成后即可进入界面，随后添加储存池。


![图片](ipfs://bafybeihwm3vkui7gn6ztyac3cmgavs6rv3vkl6atrxzjepkgafougmdrpm)


## **后记**

黑群辉的一些功能缺失，因此套件中心是可玩性的集中体现，推荐 https://imnks.com/1780.html 添加该源开始你的NAS之旅吧！


