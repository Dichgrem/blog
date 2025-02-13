+++
title = "乱七八糟:服务器初始化与安全"
date = 2024-06-12

[taxonomies]
tags = ["乱七八糟"]
+++

前言 本文记录服务器安装docker，1panel和设置禁止root登录的步骤。
<!-- more -->



## Doamin

建站不一定需要服务器、域名和备案。尤其不要买腾讯云，不要买CN域名；备案过程要填很多个人信息，且一周没有解析网站，备案就会自动注销。

- **cn 域名没有隐私保护（whois protection）**。国内域名注册商说的隐私保护根本是自欺欺人，在它们隐私保护就是在自己的查询服务隐藏注册人信息。但在别的地方是能查到的，在[中国互联网络信息中心](https://whois.cnnic.cn/)可以查到每个 cn 域名注册人的姓名和邮箱。

- **注册 cn 域名必须使用真实身份信息**。要是你想用假信息注册来保护隐私，那就太天真了。审核的时候不会通过的。真的不想用自己的信息注册的话，要么以公司名义注册，但公司的法定代表人还是能查到的。要么叫别人注册域名之后给你用，可谁愿意冒这种风险呢。

- **cn 域名无法删除**。如果你觉得 cn 域名暴露了你的隐私，那对不起，这是不能注销的[3](https://cyrusyip.org/zh-cn/post/2021/05/25/damn-cn-domain/#fn:3)。你只能修改邮箱地址，然后等到它过期。或者转让给别人，不过感觉把别人推到火坑里不太好啊。

- **cn 域名有被停用的风险**。2008 年，有人以跳水奥运冠军吴敏霞拼音注册了 wuminxia.cn，[结果被中国互联网络信息中心（CNNIC）回收了域名](https://www.cnbeta.com/articles/tech/62209.htm)，并转交给国家体育总局。此域名在 2021 年 2 月 28 日被优视科技[注册](https://whois.cnnic.cn/WhoisServlet?queryType=Domain&domain=wuminxia.cn)，呵呵。2009 年，牛博网被域名注册商万网停止解析。

## Docker

1. 安装基础工具

````
sudo apt-get update
 sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
````

2. 安装docker的gpg key：

````
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
````

3. 安装docker源

````
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
````

上面命令中的`lsb_release -cs`返回`bullseye`，也就是debian11的代号。

4. 安装docker

````
apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
sudo systemctl enable docker
sudo systemctl start docker
````

至此安装完成。

在debian系的Linux发行版上，docker会开机启动启动。

如果平时使用非root账户，又不想每次执行docker命令之前都加上sudo，参考docker的[文档](https://link.zhihu.com/?target=https%253A//docs.docker.com/engine/install/linux-postinstall/%2523manage-docker-as-a-non-root-user)，可以添加`docker`组，并将非root账户加入到该组中。下面的命令创建`docker`组并将当前用户加入`docker`组，执行完成之后重新登陆生效：

````
sudo groupadd docker
sudo usermod -aG docker $USER
````

### 使用 Docker 存储库安装

我建议使用此方法的主要原因是您可以轻松升级，因为存储库可以轻松更新！

首先，使用以下命令安装此方法的先决条件：

````
sudo apt update && sudo apt install ca-certificates curl gnupg
````

现在，让我们使用以下命令创建一个目录来存储密钥环：

````
sudo install -m 0755 -d /etc/apt/keyrings
````

接下来，使用给定的命令下载 GPG 密钥并将其存储在 `/etc/apt/keyrings/etc/apt/keyrings` 目录中：

````
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
````

完成后，使用 chmod 命令更改 docker.gpg 文件的权限：

````
sudo chmod a+r /etc/apt/keyrings/docker.gpg
````

最后，使用以下命令为 Docker 设置存储库：

````
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
````

上述命令中每行末尾的额外 `\` 只是添加新行的一种方式，以便您可以轻松查看整个命令。就是这样！

现在，您可以使用以下命令更新存储库索引并安装 Docker：

````
sudo apt update && sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
````

要验证 Docker 安装，您可以安装 hello-world 映像：

````
sudo docker run hello-world
````

hello-world docker 镜像很小，仅用于检查 Docker 是否运行正常。



### 使用 Docker 而不使用 sudo

如果您注意到，在运行 hello-world 映像时，我使用了 sudo。

这可能不太方便。那么如何将其配置为不必使用 sudo 呢？

为此，首先使用 groupadd 命令创建一个 docker 组：

````
sudo groupadd docker
````

现在，将用户添加到组（docker）：

````
sudo usermod -aG docker $USER
````

现在从终端注销并重新登录以使更改生效。

> 如果您在虚拟机中安装 Docker，需要重新启动才能使您所做的更改生效。

让我们通过运行 hello-world 图像来测试它：

````
docker run hello-world
````

正如您所看到的，我无需使用 sudo 即可获得相同的结果。

### 卸载 Docker

首先，使用以下命令停止 docker 服务：

````
sudo systemctl stop docker
````

然后按以下方式使用 apt purge 命令从系统中删除 Docker：

````
sudo apt purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin 
````

如果您打算进行全新安装或者不想拥有任何以前的数据，那么您可以使用 rm 命令删除 Docker 文件：

````
sudo rm -rf /var/lib/docker
````

````
sudo rm -rf /var/lib/containerd
````

就是这样！ Docker 已成功删除。

## 1panel

执行如下命令一键安装 1Panel:

````
curl -sSL https://resource.fit2cloud.com/1panel/package/quick_start.sh -o quick_start.sh && sudo bash quick_start.sh
````

## Safe

### 设置禁止Root直接登陆

使用root账户或已经有sudo权限的用户登录到系统。

执行以下命令来创建新用户，例如，将`newuser`替换为你想要的用户名：

````
sudo adduser dich
````

接下来，将新用户添加到sudo组，以赋予sudo权限。执行以下命令：

````
sudo usermod -aG sudo dich
````

确认一下sudo权限已经生效。

PS:删除用户及其主目录
```
sudo userdel -r 用户名
```
此命令不仅删除用户，还会删除用户的主目录及邮件存储目录（如果有）。

可以尝试使用新用户执行一个需要sudo权限的命令，如：

````
sudo ls /root
````

系统会要求输入密码，如果成功执行，说明新用户已经成功获得sudo权限。

打开SSH配置文件`sshd_config`，可以使用文本编辑器如nano或vi。以下是使用nano编辑器的示例：

````
sudo vim /etc/ssh/sshd_config
````

在配置文件中找到以下行：

````
#PermitRootLogin yes
````

将上述行的注释符号(#)去掉，并将`yes`改为`no`，如下所示：

````
PermitRootLogin no
````

这将禁止直接使用root账户进行SSH登录。

保存并关闭文本编辑器。

重新启动SSH服务，以应用更改：

````
sudo service ssh restart
````

或者，如果你的系统使用systemd，可以使用以下命令：

````
sudo systemctl restart ssh
````

### 更换SSH端口

使用root账户或已经有sudo权限的用户登录到系统。

打开SSH配置文件`sshd_config`，可以使用文本编辑器如nano或vi。以下是使用nano编辑器的示例：

````
sudo vim /etc/ssh/sshd_config
````

在配置文件中找到以下行：

````
Port 22
````

这是SSH默认的端口号，你可以将其更改为你想要的任何未被占用的端口号。例如，将端口更改为2222：

````
Port 2222
````

保存并关闭文本编辑器。重新启动SSH服务，以应用更改：

````
sudo service ssh restart
````

或者，如果你的系统使用systemd，可以使用以下命令：

````
sudo systemctl restart ssh
````

### 安装ufw和Fail2ban

**1、更新软件包**

````
sudo apt update
````

**2、安装 UFW**

````
sudo apt install ufw
````

**3、如果你在远程位置连接你的服务器，在启用 UFW 防火墙之前，你必须显式允许进来的 SSH 连接。否则，你将永远都无法连接到机器上。**

````
sudo ufw allow 22/tcp
````

> 如果 SSH 运行在非标准端口，你需要将上述命令中的 22 端口替换为对应的 SSH 端口。

**4、放开 1Panel 系统端口。**

````
sudo ufw allow 8090/tcp
````

> 上述命令中的 8090 端口需要替换为安装 1Panel 系统时自定义的端口。

**5、启动 UFW**

````
sudo ufw enable
````

**1、安装 Fail2ban**

````
sudo apt-get install fail2ban
````

**2、Debian 12 及以上的版本需要手动安装 rsyslog**

````
sudo apt-get install rsyslog
````

**3、启动 Fail2ban 服务**

````
sudo systemctl start fail2ban
````

**4、开机自启动**

````
sudo systemctl enable fail2ban
````

**5、查看 Fail2ban 服务状态。**

````
sudo systemctl status fail2ban
````

## Ban IPv6

手动 禁用 VPS 的 IPv6 命令:
```
sysctl -w net.ipv6.conf.all.disable_ipv6=1
sysctl -w net.ipv6.conf.default.disable_ipv6=1
```
如果想重启系统也生效， 执行：
```
echo 'net.ipv6.conf.all.disable_ipv6=1' >> /etc/sysctl.conf
echo 'net.ipv6.conf.default.disable_ipv6=1' >> /etc/sysctl.conf
```
手动 启用 VPS 的 IPv6 命令:
```
sysctl -w net.ipv6.conf.all.disable_ipv6=0
sysctl -w net.ipv6.conf.default.disable_ipv6=0
```
重新载入 sysctl 配置
```
sysctl --system # reload sysctl
```
如果重载, 还无效果, 可能要 reboot 重启下.
查看 VPS 的 IPv6 信息
```
ip -6 addr show scope global

或者 curl ipv6.ip.sb
```

## 改为密钥登录

- 在本地执行 ``ssh-keygen``，随后生成.pub后缀的公钥和无后缀的密钥，注意不同密钥对名称不能相同；同时可以为这两个文件用密码加密；

- 随后将.pub后缀的公钥中的内容写入服务器的``~/.ssh/authorized_keys``中；

- 使用命令``vim /etc/ssh/sshd_config``编译服务器的SSH配置，将其中的该行改为``PasswordAuthentication no``，保存退出；随后使用``sudo systemctl restart sshd``重启SSH即可禁用密码登录；

- 使用``sudo cat /etc/ssh/sshd_config | grep -E 'PasswordAuthentication|PubkeyAuthentication'
`` 命令查看输出，如有**PasswordAuthentication no → 禁用密码登录**以及**PubkeyAuthentication yes → 允许密钥登录**则成功。    ``  

- 注意**authorized_keys**的权限为600，如果不是则需要改正：``chmod 600 ~/.ssh/authorized_keys``

- 随后可以在本地尝试登录，命令为``ssh -i ~/.ssh/id_xxx -p 端口 用户名@服务器IP``，第一次登录会提示服务器公钥的哈希值，需要选Yes。

