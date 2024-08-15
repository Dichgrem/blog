+++
title = "谈天说地:狄奇周刊(十)"
date = 2024-08-15

[taxonomies]
tags = ["谈天说地"]
+++

前言 这里是Dich的周刊,通过博客的形式讲述每周互联网形势,以及分享一些文章。

<!-- more -->
# **事记**

## **一.[研究员：Windows 系统防火墙无法阻止 IPv6 远程代码执行漏洞](https://x.com/XiaoWei___/status/1823532146679799993)**

根据赛博昆仑实验室研究员Wei透露的部分消息，攻击者构造的IPv6数据包在到达防火墙进行数据处理之前就可能被触发，因此仅通过开启系统的IPv6防火墙并不能有效阻止攻击。

结合该漏洞的特性，这进一步验证了之前的推测，即该漏洞可以用于在内网中进行横向攻击，特别是通过内网IPv6地址实施攻击。由于部分场景下的内网设备长期无法进行系统更新，该漏洞对这些没有网络隔离且长期无法更新系统的环境构成了更大的威胁。

## **二.[婚姻登记条例拟修订，结婚和离婚不再需要户口簿](https://weibo.com/1638782947/OskWtCNkJ)**

8月13日，民政部发布了新的《婚姻登记条例》修订草案，面向社会公开征求意见。草案对结婚和离婚程序进行了调整，取消了户口簿的要求，登记地点也不再受限。

此外，新增了30天离婚冷静期的具体规定，“离婚冷静期“期间任何一方可申请撤回，并明确了隐瞒重大疾病可导致婚姻被撤销。草案还要求婚姻登记必须免费，确保个人隐私保密，并规定婚姻登记机关要提供婚姻辅导服务。（这部分细则看全文）


## **三.[韩国监管机构：KakaoPay向支付宝擅自提供用户信息](https://m-cn.yna.co.kr/view/ACK20240813002700881?input=tw)**

韩国金融监督院8月13日表示，韩国移动支付服务商 KakaoPay 近六年来未经用户同意向中国支付平台支付宝擅自提供了 4000 万名用户的 542 亿条个人信用信息。据悉，为了满足与苹果公司合作的先决条件，支付宝要求 KakaoPay 提供全体用户的信用信息。而 KakaoPay 未经用户同意就提供了支付宝所要求的全部信息，其中甚至包括未使用过跨境支付服务的用户信息。根据双方合作内容，KakaoPay 用户可在与支付宝签署支付服务协议的线上线下商店进行支付。

## **四.[华为HarmonyOS NEXT对Webview加入了风险网站检查API](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/js-apis-webview-V5#enablesafebrowsing11)**

ArkTS API 11+ 可以启用检查网站安全风险的功能。开发文档显示违规和诈骗网站的检查是默认启用的，不能通过此接口关闭；风险网站的检查是默认关闭的，可以通过此接口开启或关闭。

违规和诈骗网站的检查会发送 URL 哈希前缀和网站域名到服务器做检测，风险网站的检查会发送 URL 哈希前缀和脱敏 URL（删除 URL 的查询参数）到服务器做检测。


# **文推**



[text软考高级架构师备考记录](https://nekonull.me/posts/ruankao-architect/)

[2024年上半年无线路由器选购攻略](https://www.acwifi.net/27983.html)

[Exploring TLS certificates and their limits](https://0x00.cl/blog/2024/exploring-tls-certs/)


---

# **项目**


[基于 LaTeX 的简历生成器](https://ppresume.com)

[记录 5 个世纪技术与社会发展的巨型图谱](https://calculatingempires.net)

[免费的开源跨平台游戏引擎](https://github.com/cocos/cocos-engine)