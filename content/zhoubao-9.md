+++
title = "谈天说地:狄奇周刊(九)"
date = 2024-08-08

[taxonomies]
tags = ["谈天说地"]
+++

前言 这里是Dich的周刊,通过博客的形式讲述每周互联网形势,以及分享一些文章。

<!-- more -->
# **事记**

## **一.[威瑞信宣布.COM域名将于9月1日起涨价](https://t.me/xhqcankao/12469)**

运营 .COM 的域名注册局威瑞信将于9月1日将批发价从9.59美元提高到10.26美元。大多数域名注册商都会在此基础上加价，但有些注册商会向客户收取批发价。因此，许多注册商都会提高价格。这是 .COM 域名价格连续第四年上涨7%，这是威瑞信与互联网名称与数字地址分配机构签订的合同以及与美国政府达成协议允许的最高涨价幅度。总而言之，自四年前以来，威瑞信已将 .COM 域名批发成本提高了31%。对于域名注册者来说，好消息是威瑞信未来两年内不得涨价。但此后该公司可以连续四年每年涨价7%。


## **二.[微软 Authenticator 应用被曝设计缺陷，账号被覆盖、锁定](https://www.csoonline.com/article/3480918/design-flaw-has-microsoft-authenticator-overwriting-mfa-accounts-locking-users-out.html)**

微软的 Microsoft Authenticator 身份验证应用存在设计缺陷，用户在扫描QR码添加新账号后，应用会覆盖之前的账号，导致多重身份验证（MFA）被锁定。

这个问题的核心在于，Microsoft Authenticator 会用相同的用户名覆盖账户。由于多数用户的用户名是电子邮件地址，这个问题尤为严重。

而Google Authenticator等应用通过添加银行、汽车公司等信息来避免此问题。覆盖账号后，系统难以识别哪个账号被覆盖，可能导致新旧账号均出现验证问题。用户可能在几周或几个月后才发现账号被注销。


## **三.[马来西亚正在强制劫持 DNS 请求以阻止用户访问被屏蔽的网站](https://t.me/xhqcankao/12531)**

马来西亚互联网服务提供商 (ISP) 似乎实施了透明 DNS 代理，即强制所有互联网流量返回到其本地 DNS，即使使用谷歌和 Cloudflare 等替代 DNS 也是如此。实施该策略的 ISP 包括 Time、Maxis、U Mobile、CelcomDigi 和 Unifi。通过透明 DNS 代理，ISP 可以拦截并重定向 DNS 请求至本地 DNS。

为了遵守规定，所有电信公司/互联网服务提供商都必须通过其本地 DNS 阻止被认定为违法的网站。目前尚不清楚这项针对本地服务的透明 DNS 代理实施时间，但由于已有四家以上的电信公司实施，因此这似乎是针对所有电信公司的全国性指令。据称该指令早在今年2月就已发布。 马来西亚监管机构尚未对此有关的询问作出回应。

—— SoyaCincau、Sinarprojec

## **四.[1Password发现高危安全漏洞，Mac用户需立即升级到最新版本](https://www.landiannews.com/archives/105295.html)**

知名密码管理器1Password的Mac版本被发现存在一个高危安全漏洞，该漏洞允许恶意软件绕过进程间的通信保护，窃取用户的解锁密钥。

这一安全问题由参与DEFCON黑客大赛的安全研究人员发现，并计划在一次演讲中公开。1Password已经收到通知并及时修复了这一漏洞，敦促用户升级到8.10.38或之后的版本以确保安全。

该漏洞被标识为CVE-2024-42219，目前没有证据表明它已被恶意黑客利用，更多关于该漏洞的细节将在DEFCON大会上演讲后公布。



# **文推**

[当你不再是宇宙的正中央](https://www.velasx.com/am/6237)

[你在电脑上看到的字到底是怎么显示出来的？](https://ssshooter.com/font-and-charcode/)

[放轻松 | 治愈「被互联网影响的心智」](https://sspai.com/post/85420)



---

# **项目**

[将你的 Telegram Channel 转为微博客](https://github.com/ccbikai/BroadcastChannel)

[FList：静态页面生成的一个简洁的在线文件列表](https://github.com/jianjianai/FList)

[互联网仍有记忆！那些曾经在校招过程中毁过口头offer、意向书、三方的公司](https://github.com/forthespada/CampusShame)