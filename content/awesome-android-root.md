+++
title = "综合工程:安卓刷机与root教程"
date = 2023-08-13

[taxonomies]
tags = ["Tech","android","root"]
+++



前言 自安卓系统诞生以来，root 一直是玩机的必备过程。时至今日，在安卓定制系统日益完善的情况下，能 root 的机型越来越少，本文以小米手机为例，介绍 root 的具体方法。
<!-- more -->
## **一.什么是root**

这涉及安卓的权限系统。Andoird 系统是基于 Linux 内核的，其中的权限大致可以分为四级，即一般软件权限，用户权限，ADB 权限和超级管理员权限（su）。而所谓 Root 也就是使手机获得超级管理员的权限，但是出于种种原因，厂商默认不提供超级管理员的权限，因此，root的本质就是一个提权的过程。

## **二.为什么要root**

以权限系统为例，一般软件权限需要经过用户同意，即每次安装前出现的各种请求弹窗；而 ADB 权限常常用于开发者模式，可以调试一些比较深层的设置；至于 root 权限则为系统的最高权限，与 Windows 的 system 权限相当（比 administer 还高）。因此，当我们具备了 root 权限后，就可以实现许多功能，例如屏蔽广告，虚拟定位，安装 Google 框架和软件，满血运行CPU等等。

## **三.如何root**

首先我们要了解安卓系统的分区和启动。安卓的分区包括

- recovery 分区，类似PC端的PE环境，手机上的恢复出厂设置即为从 recovery 恢复；

- cache 分区，保存系统最常访问的数据和应用程序。 擦除这个分区，不会影响个人数据，只是删除了这个分区中已经保存的缓存内容；

- boot 分区，类似PC端的MBR分区，用来引导系统启动，擦除后手机会卡在开机 logo 的界面；

- system 分区，包括操作系统与软件，vendor 定制文件与库文件等等，擦除后会卡在开机的动画界面；

- data 分区，存放用户数据和系统设置，擦除后不影响系统的运行。

手机启动阶段存在名为 bootloader 的程序，与 PC 端的 BIOS 类似，被称为 fastboot 模式，厂商一般会将其锁定。

早些年间，存在大量一键 root，kingroot 之类的软件，可以直接刷写 root 包，获得 root 权限，但成功率不高；

因此，现在主流的刷机步骤为

- 解开 bootloader，俗称解BL锁.

- 打开机器上的允许USB调试;

- 备份Data分区(可使用Neobackup或系统自带),字库/基带/官方固件,桌面样式截图;

- 下载该机器的官方原厂包以及要刷的第三方系统包;

- 提取以上两个包中的boot.img和recovery.img文件备份;

- 将机器与PC等设备连接,并进入bootloader;

- 刷入第三方 recovery，比如大名鼎鼎的 TWRP,或者对应新系统的recovery.img；

- 进入 recovery 模式，清空原系统数据;然后刷入原系统作为底包，避免出现固件问题;

- 刷入新系统Zip包,随后重启,再次进入 recovery .

- 刷入 Magisk (面具)工具，随后重启进入桌面,安装Magisk(apk),通过修补boot.img文件获得 root 权限；

- 安装 Magisk 模块和 Lsposed 框架（可在其中下载许多模块，推荐一键救砖，系统优化和 root 隐藏）

- 安装 Momo 软件检测系统环境是否正常。

## **四.哪些机型可以root**

看到这里很多小伙伴肯定跃跃欲试，不过在 2023 年的今天，能 root 的机型还是比较少。首先是最容易的一加和小米，可以申请官方解 BL 锁，需要等待7天；其次是联想，索尼等海外品牌，也比较容易；而 oppo 和 vivo 及其子品牌 iqoo 和 realme 有些是不行的；苹果的越狱在10代前是可以的，而华为全部机型都是不可以的，除非上万能的淘宝收费解锁，直接烧录芯片；至于三星，BL 锁一旦解开就会触发芯片物理熔断机制，无法使用 pay 以及升级系统，体验极差。因此，刷机有风险，root 需谨慎！刷机前要了解相应的厂商，考虑保修和变砖的问题！

## **五.具体操作流程**

1.笔者以 redmi k30pro 5G 这款手机为例，首先我们进入手机设置界面，进入“我的设备”，在“全部参数”中找到“ MIUI 版本”，连续点击后开启开发者模式，随后在“更多设置”中开启 USB 调试， USB安装 功能。

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

> 截至2024年5月1日,该机型已经刷入crdroid 类原生系统,非常丝滑,步骤同上,但需要刷入[新固件](https://xiaomifirmwareupdater.com/firmware/lmi/stable/V14.0.1.0.SJKMIXM/).

## **六.Root后的模块安装**

包括大名鼎鼎的yc调度，Lposed 框架，李跳跳和 scene 软件都是必备的。
## **后记**

关于 root 的其他用途还有很多，这里就不一一列举，分享一些常用模块：

``https://sspai.com/post/68531``

## **参考**
 
 - [lineageos镜像](https://download.lineageos.org/devices/lmi/builds)
 - [lineageos教程](https://wiki.lineageos.org/devices/lmi/install/variant1/)
 - [机型介绍](https://wiki.lineageos.org/devices/lmi/variant2/)
 - [XDA-没有声音的问题解决](https://xdaforums.com/t/no-sound-issue-can-u-help.4479225/)
 - [XDA-更换内核](https://xdaforums.com/t/kernel-overclocked-no-gravity-2023-08-28-protonclang.4531497/)


## **后记**

安卓目录常见文件夹

/.7934039a：支付宝生成，可删除，会重生。
/.android：不清楚是什么软件生成的缓存，可删除，会重生。
/.android_随机串：不清楚是什么软件生成的一堆目录，可删除，会重生。
/.Application：未知
/.BD_SAPI_CACHE：百度一些接口的缓存，可删除。
/.cc：未知
/.com.kwai.weapon.conjure：应该是快手相关软件生成的目录，可删除。
/.com.pingan.paces.ccms：平安口袋银行的旧版目录，可删除。
/.DataStorage：应该是淘宝或者天猫生成的，可删除，会重生。
/.GidConfig：未知
/.gs_file：应该是应用调用谷歌的某些协议，可删除。
/.gs_fs数字：同上，可删除，但是删了很快又会生成。
/.iFly、iFlyIME：讯飞输入法相关，不用可删。
/.jdd：未知
/.jds：未知
/.jpush：未知
/.knights：未知
/.kspdf：未知
/.lm_device：未知
/.migamesdk：小米游戏生成，用于记录小米游戏设备id，可删除。
/.mobo：MoboPlayer生成，不用可删。
/.mtdfp：美团生成，可删除。
/.OAIDSystemConfig：阿里的跟广告个性化推荐有关的配置项，里面有一个Global的文件夹，应该是全局标识。可删除。
/.PASpeechSDK：平安的某个SDK，可删除。
/.pns：未知
/.protected_image：是手机管家防止应用删除照片设置的。可以在手机管家-隐私保护-照明弹-防止应用删除照片关闭，删除后就不会再出现，可删除。
/.push：美图秀秀的推送标识，可删除。
/.qqpim_guid：QQ同步助手的全局唯一标识，在备份数据时会生成，可删除。
/.saf_device：未知
/.tomb：QQ浏览器日志，可删除。
/.ttcryptofile：腾讯文件管理器日志，可删除。
/.turingdebug：未知
/.tv.acfundanmaku.video：AcFun相关，可删除，会重生。
/.ufs：未知
/.UTSystemConfig：也是阿里的全局标识，可删除，会重生。
/.uxx：未知
/.vivo：下级目录为pushsdk，可能是手机的某个app用了vivo的推送服务，可删除。
/.Xiaomi_xl：未知
/1.1.6.3、1.1.7：某软件的版本号，可删除。
/360：360保存的设备id等信息，不用可删。
/AcFun：AcFun创建的目录
/Cache：缓存目录，可删除。
/core：看名字是什么核心吧，但是其实没那么重要，可删除。
/Devlog：日志目录，可删除，但是打开AcFun会生成。
/aegis：未知（可能与验证有关）
/ag：未知
/Alarms：未知
/alipay：支付宝钱包
/AlivcData：未知
/amap：未知
/Android：
/.vy：未知
/com：
/iflytek：讯飞生成的，不用可删。
/data：该目录下为各app主要目录，如果没有卸载app不建议删除，不用可删。但是目录下的cache目录是缓存目录，可删除。
/.dat：不清楚是什么，可删除。
/cmb.pb：招商银行
/cn.com.bmac.nfc：北京一卡通
/cn.deepink.reader：厚墨
/cn.gov.tax.its：个人所得税
/cn.missfresh.application：每日优鲜
/cn.wps.moffice_eng：WPS
/cn.wps.moffice_eng.xiaomi.lite：WPS
/cn.yonghui.hyd：永辉生活
/com.android.*：系统自带应用
/com.autonavi.minimap：高德地图
/com.baidu.BaiduMap：百度地图
/com.baidu.netdisk：百度网盘
/com.citiccard.mobilebank：中信银行
/com.cmbchina.ccd.pluto.cmdActivity：招商银行
/com.cnwinwin.seats：babyfirst安全座椅
/com.didi.es.psngr：企业滴滴
/com.douguo.recipe：豆果美食
/com.duokan.reader：多看阅读器
/com.ebank.creditcard：阳光惠生活
/com.eg.android.AlipayGphone：支付宝
/com.google.android.gms：谷歌GMS
/com.gotokeep.keep：keep
/com.hexin.plat.android：同花顺
/com.hpbr.bosszhipin：BOSS直聘
/com.jd.jxj：京喜
/com.jianshu.haruki：简书
/com.jingdong.app.mall：京东商城
/com.jyysxz.xyz：简易影视
/com.keke.ysh1.tz08051h：可可影视
/com.lingan.yunqi：宝宝记
/com.linkfungame.ag：阿哥美剧
/com.mangshe.tvdown：蟒蛇下载
/com.mfashionggallery.emag：小米画报
/com.mi.health：小米健康
/com.milink.service：米联服务
/com.mipay.wallet：小米钱包
/com.miui.：MIUI系统相关，慎删*。
/com.mmbox.xbrowser.pro：X浏览器
/com.MobileTicket：铁路12306
/com.mt.mtxx.mtxx：美图秀秀
/com.oray.sunlogin：向日葵
/com.pa.health：平安健康保险
/com.picovr.assistantphone：PICO VR
/com.pingan.carowner：平安好车主
/com.pingan.paces.ccms：平安口袋银行
/com.psyone.brainmusic：小睡眠
/com.qiyi.video：爱奇艺
/com.qti.ltedc：可能是高通相关的东西，不建议删。
/com.sankuai.meituan：美团
/com.sdu.didi.psnger：滴滴
/com.sgcc.wsgw.cn：网上国网
/com.sinovatech.unicom.ui：中国联通
/com.snssdk.api：字节跳动相关app
/com.snssdk.api.embed：字节跳动相关app，看了下这个里面只有缓存，可删除。
/com.sohu.inputmethod.sogou.xiaomi：搜狗输入法小米版
/com.ss.android.lark：飞书
/com.ss.android.ugc.aweme：抖音
/com.systoon.beijingtoon：北京通
/com.taobao.etao：一淘
/com.taobao.idlefish：闲鱼
/com.taobao.taobao：淘宝
/com.tencent.androidqqmail：QQ邮箱
/com.tencent.ams：可能是广告相关东西，不是很确定，可删除。
/com.tencent.karaoke：全民K歌
/com.tencent.lolm：英雄联盟手游
/com.tencent.mm：微信
/com.tencent.mobileqq：QQ
/com.tencent.qqlive：腾讯视频
/com.tencent.qqpim：QQ同步助手
/com.tencent.tbsad：腾讯的文件浏览服务，以及广告相关
/com.tencent.weread：微信读书
/com.tencent.wework：企业微信
/com.tmri.app.main：交管12123
/com.unionpay：云闪付
/com.valvesoftware.android.steam.community：steam
/com.wuba.zhuanzhuan：转转
/com.x.webshuttle：网梭
/com.xiaomi.*：小米相关
/com.ximalaya.ting.android：喜马拉雅
/com.xingin.xhs：小红书
/com.xunmeng.pingduoduo：拼多多
/com.yaya.zone：叮咚买菜
/com.youdao.note：有道
/com.youku.phone：优酷
/com.zcbl.bjjj_driving：北京交警
/com.zhihu.android：知乎
/com.zhouyu.music：微音乐
/idm.internet.download.manager.plus：idm+
/leancloud：应该是人工桌面app生成的
/me.ele：饿了么
/pushSdk：应该是某个app的推送开发包日志，可删除。
/remix.myplayer：APlayer
/System：不清楚是哪个app生成的，里面比较乱，全是加密串，可删除。
/tv.acfundanmaku.video：ACFUN
/tv.danmaku.bili：哔哩哔哩
/video.player.videoplayer：影视
/youqu.android.todesk：todesk
.MD5串：某个游戏的配置文件，可删除。
.DataId：某个app的唯一标识，可删除。
.dxData.db：某个app的数据库文件，可删除。
.m5s：某个app的唯一标识，可删除。
.mn_数字：某个app生成的临时数据文件，可删除。
.nomedia：这个文件用于告诉各种多媒体浏览器（比如图片查看器）这个文件夹没有多媒体文件，不建议删。
/media：看了一下里面的文件夹都是空的，可删除。但是再次打开某个APP时还是会生成。
/obb：看字面意思是安卓游戏数据包，里面没几个文件，不建议删。
/obj：空文件夹，可删除。
/sdk_patch：看字面意思是某app的开发包补丁，可删除。
/system：里面的文件都是加密文件，不清楚有什么作用，不建议删。
._85BtqpOT4h：不清楚是什么，可删除。
._numtest：某个app测试用的，可删除。
.10010init：中国联通生成的初始化文件，可删除。
.10010uid：中国联通生成的用户唯一标识，可删除。
.MD5串：不清楚是什么，可删除。
.android.db：不清楚是什么东西的数据库文件，不建议删。
.firstStatus：不清楚是什么，可删除。
.iacovnfld.：某个app的唯一标识，可删除。
.mt_dp_union_id：某个app的唯一标识，可删除。
.sunique：某个app的唯一标识，可删除。
.sys：不清楚是什么东西的数据库文件，不建议删。
.system_android_l2：不清楚是什么，不建议删。
.td-3：某个app的唯一标识，可删除。
.tdck：某个app的唯一标识，可删除。
.thumb_y：某个app的唯一标识，可删除。
.uniqdafc：某个app的唯一标识，可删除。
.vpcache_iq：某个app的唯一标识，可删除。
MD5串（多个）：不清楚是什么，可删除。
ANDROID.PERMISSION.TEST：空文件，某个app用于权限测试，可删除。
ZHVzY2Lk：某个app的唯一标识，可删除。
/aplayerlog：APlayer播放器日志，可删除。
/auf：不清楚是什么app，下载相关的吧，可删除。
/autonavi：高德地图临时文件，可删除。
/backup：app的备份数据，由于现在的app数据基本都在线上，所以想删旧删吧。可删除。
/backups：备份相关，不建议删。
/baidu：百度，只要用百度系app就会生成这个文件夹，不用可删。
/BaiduMap：百度地图，空文件夹，可删除。
/BaiduNetdisk：百度网盘，空文件夹，可删除。
/bcs：不清楚是什么，空文件夹，可删除。
/beijingtoon：北京通，不用可删。
/cache：缓存，可删除。
/log：日志，可删除。
/bjjj：北京交警缓存，可删除。
/bobo：某播放器缓存，可删除。
/browser：某个浏览器的缓存，可删除。
/bwton：不清楚是什么，空文件夹，可删除。
/ByteDownload：字节下载文件夹，删除之前记得看看里面的东西要不要保留。可删除。
/Cardboard：跟谷歌相关的，不清楚是什么，可删除。
/Catfish：可能是某游戏生成的，可删除。
/challenge：
/cmb：招商银行，不用可删。
/RunningLog：日志，可删除。
/cmb.pb：招商银行，不用可删。
/log：日志，可删除。
/cn.com.bmac.nfc：北京一卡通，不用可删。
/logcatic：日志，可删除。
/cn.xuexi.android：学习强国，不用可删。
/com.alibaba.wireless：阿里巴巴，不用可删。
/WXOPENIM：
/tcmslog：日志，可删除。
/com.baidu.netdisk：百度网盘，不用可删。
/com.gotokeep.keep：keep，不用可删。
/com.linkfungame.ag：阿哥美剧，不用可删。
/com.mangshe.tvdown：蟒蛇下载，不用可删。
/com.miui.guardprovider_TMF_TMS：安全卫士相关，不用可删。
/com.miui.voiceassist：看名字是声音助手，空文件夹，可删除。
/com.MobileTicket：铁路12306，不用可删。
/com.mt.mtxx.mtxx：美图秀秀，不用可删。
/com.netease.cloudmusic：网易云音乐，不用可删。
agorasdk.log：日志文件，可删除。
/com.ormatch.android.asmr：耳萌，不用可删。
/com.picovr.assistantphone：Pico VR助手，不用可删。
/nim：
/log：日志，可删除。
/com.sankuai.meituan：美团，不用可删。
/com.sgcc.wsgw.cn：网上国网，不用可删。
/com.shinyv.cnr：云听，不用可删。
/nim：
/log：日志，可删除。
/com.sina.heimao：黑猫投诉，不用可删。
/com.tencent.android.qqdownloader_TMF_TMS：腾讯相关下载，不用可删。
/com.tencent.karaoke_KcSdk：全民K歌相关，不用可删。
/com.tencent.lolm_KcSdk：英雄联盟手游相关，不用可删。
/com.tencent.mm：微信相关，不用可删。
/com.tencent.mobileqq：QQ相关，不用可删。
/com.tencent.mobileqq_KcSdk：QQ相关，不用可删。
/com.tencent.mtt_KcSdk：QQ浏览器相关，不用可删。
/com.tencent.qqpim：QQ同步助手，不用可删。
/com.tnscreen.main：多屏互动相关，不用可删。
/config：某个app的下载位置配置文件，可删除。
/content：不清楚是什么软件生成的，空则可删。
/data：不清楚是什么软件生成的，空则可删。
/DCIM：系统相册目录，慎删。
/didi_es：滴滴企业版相关，不用可删。
/DkMiBrowserDemo：小米浏览器debug日志，可删除。
/doctorcareforuser：
/Documents：文档目录，里面是些app的下载和缓存，可删除，但是注意是否又自己下载的东西需要保留。
/Download：下载目录，里面是大部分app的下载文件，可删除，但是注意是否又自己下载的东西需要保留。
/dubbing：应该是配音秀生成的目录，里面是下载和缓存，可删除。
/duilite：
/eastmoneyjj：天天基金，不用可删。
/EasyBike：哈罗出行，不用可删。
/FileExplorer：文件浏览器，慎删。
/Fonts：字体文件夹，慎删。
/geetest：某个app使用了极验的验证码，可删除。
/GlideCache：图片缓存文件夹，可删除，查看图片的时候会重生，但已删除的图片的缓存就算被清理了。
/guardlibs：里面只有一个京东图片缓存的文件夹，可删除。
/hexin：同花顺，不用可删。
/cache：缓存，可删除。
/HibyMusic：海贝音乐，不用可删。
/hubble：不清楚是什么app生成的，唯一标识之类的东西，可删除。
/ICBC：工商银行，里面有好几层文件夹，点到底只有一个头像，可删除，但是应该会重生。
/Ice：应该是什么视频app创建的，可以看一下手机里有没有哪个视频app是跟冰啊雪啊有关的，不用可删。
CacheVideo：缓存文件夹，可删除。
/IDMP：某浏览器扩展（火狐或者Edge），可删除。
/image_loader_cache：不知道时什么app生成的，看名字是图片缓存，可删除。
/Installation：里面只有一个文件，文件内容是一排0，可删除。
/IntsigLog：云闪付的错误日志，可删除。
/jidata：不知道是什么app生成的，里面是几个提示语，可删除。
/JRTyrell：不知道是什么app生成的，里面是设备信息，可删除。
/keep：keep相关，不用可删。
/Lark：飞书生成的，空目录，可删除。
/libs：慎删。
/lk：不知道是什么app生成的，里面都是各地的地标、美食等的JSON文件，可删除。
/Local：慎删。
/lucky_prize_sdk_log：应该是某个app抽奖的sdk日志，可删除。
/mace_run：好像是小米开源框架相关的东西，慎删。
/mangshedown：蟒蛇下载相关，不用可删。
down：蟒蛇下载下载的文件位置。
/mbo_buffer：什么软件的缓存，可删除。
/mediaop：优酷的图片缓存文件夹，可删除，会重生。
/mhealth365：掌上心电，不用可删。
/mishop：小米商城相关，空文件夹，可删除。
/MIUI：小米系统相关
/.ringtone：铃声
/backup：备份
/BugReportCache：bug报告缓存，可删除。
/Camera：相机相关
/debug_log：debug日志，可删除。
/Gallery：相册
/cloud：云相册
/owner：自定义相册
/secretAlbum：私密相册
/Mirror：直接翻译是镜子，但不知道是什么app用的。
/MiVideo_privacy：小米视频私密文件夹。
/Mms：短信相关
/music：音乐
/sound_recorder：录音机
/Video：视频
/VipAccount：vip账户，里面也没啥
.td-3：可删除。
.tdck：可删除。
.theme_migrate：可删除。
.theme_migrate_back：可删除。
/mivideo：小米视频，空文件夹，可删除。
/Mob：慎删。
/MomentCamSys：魔漫相机目录，不用可删
/monitor：未知，空文件夹，可删除。
/Movies：手机视频目录，慎删。
/Music：手机音乐目录，慎删。
/netease：网易
cloudmusic：网易云音乐，不用可删。
/netease_onepushservice：应该是网易的推送服务，可删除。
/nfcUtils：应该是北京一卡通相关，不用可删。
/Notifications：应该是系统自带的提醒相关，慎删。
/oray：向日葵相关，里面只有一个图标，可删除。
/OSSLog：阿里云OSS日志目录，可删除。
/PASpeechSDK：平安口袋银行相关，空目录，可删除。
/Pictures：手机图片目录，慎删。里面有些app保存图片的文件夹，如果不用了可以删，但是删了之后保存的图片也就没了，所以自己检查一下。
/.gs：里面是一些app的缓存图片，全部可删除，有些会重生。
/.gs_fs数字：都是缓存图片，可删除。
/.thumbnails：相册缩略图，可删除。
/com.baidu.BaiduMap：百度地图相关，空文件夹，可删除。
/com.netease.cloudmusic：网易云音乐相关，空文件夹，可删除。
/douyin：抖音相关，空文件夹，可删除。
/dragImgs：不知道是什么app生成，可删除。
/Gallery：相册
/JDImage：看名字应该是京东的图片保存地，可删除。
/Lark：飞书保存图片的地方，可删除。
/meituan：美团保存图片的地方，空文件夹，可删除。
/Pab：应该是平安银行吧，空文件夹，可删除。
/paintpad：画板保存图片的地方。
/scanner：手机扫描保存图片的地方。
/share：分享保存的图片。
/taobao：淘宝，可删除。
/WeiXin：微信保存图片的地方，里面可能有很多你保存的图片，慎删。
/wv_save_image：
/知乎：知乎保存图片的地方，可删除。
其他文件：慎删。
/Qmap：QQ地图（QQ中位置应用），可删除。
/QQBrowser：QQ浏览器，不用可删。
/qqmusic：QQ音乐，不用可删。
/cache：缓存，可删除。
/log：日志，可删除。
/playerlog：日志，可删除。
/xlog：日志，可删除。
/thirdApiCaches：缓存，可删除。
common_statics.log：日志文件，可删除。
delete_song_log：日志文件，可删除。
ImageCache：图片缓存，可删除。
playLog：日志文件，可删除。
safemode.slog：日志文件，可删除。
/qqmusicconfig：QQ音乐配置文件，不用可删。
/qqmusiclite：QQ音乐简洁版，不用可删。
/cache：缓存，可删除。
/qqmusicmlive：fan直播（应该是QQ音乐带的），不用可删。
/qqpim：QQ同步助手，不用可删。
/qqstory：未知
.tmp：临时文件，可删除。
/qsvf：应该是爱奇艺相关的目录，里面只有一个筛选相关的配置文件。可删除。
/ramdump：系统出错时的日志，不用可删。
/RDModules：不知道是什么，可删除。
/record：不知道是哪个app生成的，空则可删。
/responses：不知道哪个app网络相应的缓存，可删除。
/save_data：一些游戏的存档，不用可删。
/com.custom.spillit.mg：Spill It，一个丢球砸杯子的游戏
/com.disney.WMW.elm：小鳄鱼爱洗澡
/pampam.ibf2.xsl：翻转瓶子
/sina：新浪相关，不用可删。
/sinaheimao：黑猫投诉，不用可删。
/SM：里面有一个jar包，不清楚是什么东西，可删除。
/SmartHome：米家摄像头生成的，如果家里有其他米家设备可能也会在里面，但是里面的文件应该没什么用。
/ImiCameraLog：米家摄像头的日志，可删除。
/sogou：搜狗输入法，不用可删。
/corelog：日志，可删除。
/statistic：不知道是什么app生成的，里面是设备和用户的唯一标识，可删除。
/storage：这个应该是阿里巴巴对目录识别错误，把本来要放到/Android/data下的文件放到了这里，不用可删。
/Subtitles：不知道是什么app生成的，应该是字幕相关的东西，空则可删。
/sysdata：不知道是什么app生成的，里面是一个用户标识，可删除。
/system：不知道是什么app生成的，可删除。
/tmp：临时文件。
dp_sys_log.trace：日志，可删除。
/systemgameinfo：不知道是什么app生成的，看名字是保存了某些游戏信息，可删除。
/tad：不知道是什么app生成的，可删除。
/tbs：怀疑是腾讯浏览服务生成的，里面都是空的文件，可删除。
/TbsReaderTemp：腾讯浏览服务的临时文件，可删除。
/tencent：腾讯相关
/alliance_sdk：好像是一个全局SDK相关文件，慎删。
/ams：里面只有一个cache缓存文件夹，可删除。
/assistant：什么助手？空则可删。
/audio：音频，空则可删。
/blob：未知
/mqq：未知
atb.idx：未知
atb.数字：未知
/bssdk：微视下载目录
/com.tencent.mtt：QQ浏览器，不用可删。
euplog.txt：日志文件，可删除。
/com.tencent.qt.qtl：掌上英雄联盟，不用可删。
/ZhangMengAlbum：掌盟相册
/com.tencent.weishi：微视，里面只有一个日志文件，可删除。
/com.tencent.weread：微信读书，不用可删。
/MicroMsg：微信，不用可删。
/MD5串：可能是缓存一类东西，可删除。
/emoji：表情
/finder：查找
/image2：图片
/package：包
.nomedia：隐藏媒体文件
/browser：内置浏览器
/facedir：未知
/recovery：恢复相关
/WeiXin：微信保存的图片和视频都在里面，慎删。
/Midas：未知
/Log：日志，可删除。
/Runtime：未知，可删除。
/MobileQQ：QQ，不用可删。
/qduc：未知
/QQ_Images：QQ保存的图片，慎删。
/QQfile_recv：QQ保存的文件，慎删。
/.tmp：临时文件，可删除。
/QQmail：QQ邮箱，不用可删。
/card_cache：缓存，可删除。
emailIcon：邮箱图标，缓存图标，可删除。
/imagecache：图片缓存，可删除。
/log：日志，可删除。
/nickIcon：图标，里面是一堆空文件夹，可删除。
/qmlog：日志，可删除。
/tmp：临时文件，可删除。
/QQSec：未知
/log：日志，可删除。
/tbs：文件浏览服务，里面应该都是QQ或微信浏览文件时生成的文件夹，可删除。
/tbs_audio_data：文件浏览服务音频相关，可删除。
/tbs_live_log：文件浏览服务日志，可删除。
/tga：腾讯游戏平台相关，不用可删。
/liveplugin：
/log：日志，可删除。
/TMAssistantSDK：未知
Download：下载
com.tencent.mm：微信
/tmp：临时文件，可删除。
/WeixinWork：企业微信，不用可删。
/weread：微信读书，不用可删。
fonts：字体
/wesee：微视，不用可删。
wxa：
v5_wxa_dynamic_pkg_release_armeabi-v7a.zip：感觉没什么用，可删除。
/wns：新闻，不用可删。
Logs：日志，可删除。
._nana：一个唯一标识，可删除。
._U6LD8yYY8f：密文，可删除。
.DrvZPZQ：密文，可删除。
.随机串.sm：密文，可删除。
.tencent_id：一个唯一标识，可删除。
.thumb_y：唯一标识，可删除。
dp_sys_log.trace：日志，可删除。
Midas.xml：唯一标识，应该是上面的Midas生成的，可删除。
/TMRI_12123：交警12123，不用可删。
cache：缓存，可删除。
download：下载
/toon：北京通，不用可删。
/TurboNet：百度系app
/BaiduMap_mainprocess：百度地图（不知道是百度地图的什么），不用可删。
/BaiduMap_voice：百度地图（声音文件），不用可删。
/haokan：好看视频，不用可删。
/vse：未知
/tv.acfundanmaku.video：ACFUN，不用可删。
/tv.danmaku.bili：哔哩哔哩
/Unicom：中国联通
/unicom_cache_image：中国联通图片缓存，可删除。
/unisdk_push：网易推送SDK
com.netease.dwrg.：第五人格，不用可删*。
com.netease.push.*：推送相关
com.netease.wyclx.：楚留香，不用可删*。
/unity_jail：未知
/userexperience：用户体验
/UVideo：UVideo，不用可删。
/UZMap：怀疑是微信缓存的一些图片，可删除。
/voip-data：应该是MIUI系统带的，里面都是些日志文件，可删除。
com.android.camera：相机
com.miui.gallery：相册
com.xiaomi.gamecenter：游戏中心
com.xiaomi.shop：小米商城
/WChat：微信，不用可删。
/wesee_interaction_sdk：微视交互SDK，不用可删。
/interaction_demo_temp：临时文件，可删除。
/interaction_sdk_debug：debug文件，可删除。
/widgetone：
/apps：
/A10071600：未知，里面的文件夹空则可删。
audio：音频
myspace：我的空间
photo：照片
video：视频
/HelloWorld：这个应该是测试用的，可删除。
audio：音频
myspace：我的空间
photo：照片
video：视频
/zhny-wsgw：网上国网，里面的文件夹空则可删。
audio：音频
myspace：我的空间
photo：照片
video：视频
/widgets：未知
/wiseapm：应该是平安银行的监控相关的
sdk：标准开发包，空则可删。
/wsgw_h5：网上国网H5相关的东西，不用可删，空则可删。
/X Video Player：XPlayer，不用可删。
.cache：缓存，可删除。
.private：隐私列表相关
Screenshot：截图
/Xiaomi：小米相关
com.xiaomi.vipaccount：vip账户，里面也没啥，可删除。
misdk：空文件夹，可删除。
service：
logs：日志，可删除。
/xzone：未知
system：唯一标识，可删除。
/ycmedia：虎牙直播相关，不用可删。
com.duowan.kiwi
com.huya.ice
/yplogfile：不知道是什么app的日志，可删除。
/za：阿里相关，可删除。
/ZCSDK：阿里相关，可删除。
/zcsmart：阿里相关，可删除。
/zeus：未知，里面是一个唯一标识，可删除。
/zhihu：知乎，不用可删。
/简白：简白，不用可删。
..ccdid：可删除。
..ccvid：可删除。
._android.dat：可删除。
._driver.dat：可删除。
.3518910b：可删除。
.a.dat：可删除。
.Android_1e0502514d961b4f876836f7088：可删除。
.astable：可删除。
.dev_id.txt：可删除。
.deviceId：可删除。
.duid：可删除。
.emcoookie：可删除。
.f_dfpid_com.sankuai.meituan：可删除。
.huid：可删除。
.imei.txt：可删除。
.lich_udid_prefs_2017_#@#：可删除。
.mn_数字串：可删除。
字母_字母：可删除。
.oukdtft：可删除。
.qm_guid：可删除。
.rwtest：可删除。
.skvec：可删除。
.sys：可删除。
.sys.log：可删除。
.syslog：可删除。
.system_log.trace：可删除。
.tcookieid：可删除。
.td-3：可删除。
.tdck：可删除。
.thumbcache_idx_001：可删除。
.thumbcache_idx_McqWd0osIPezODQX95O1：可删除。
.turing.dat：可删除。
.wps_preloaded_2.txt：可删除。
.X_U_I_D.txt：可删除。
.xs-1：可删除。
.ymuid：可删除。
.zzz：可删除。
3dplayer.log：可删除。
MD5串：可删除。
20211105211504.png：可删除。
aiscene.bin：可删除。
bobo_gyro.txt：可删除。
BRAnyChatCore.log：可删除。
c75a39d064426996：可删除。
com.yyzy.mi-gadcsdk：可删除。
dctp：可删除。