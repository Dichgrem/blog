+++
title = "乱七八糟:Git使用简明手册"
date = 2023-12-15

[taxonomies]
tags = ["Tech","Git"]
+++



前言 Git，作为现代软件开发中不可或缺的版本控制工具，常常让初学者感到困惑。本文旨在介绍 Git 的全流程安装和基本使用，希望能够帮助新手更轻松地理解和掌握 Git 的基本概念和操作。
<!-- more -->
## 安装git

Windows：[https://git-scm.com/download/](https://git-scm.com/download/)

Archlinux：`sudo pacman -S git`

安装完成后检查版本：`git --version`

## 创建新仓库

如果你的目录不是一个 Git 仓库，你需要先初始化。

创建新文件夹，在你的项目目录中运行以下命令：  
`git init`

设置默认仓库为 main,避免因为 main/master 名称不同的牛马问题：

`git init --initial-branch=main`

## 克隆仓库

执行如下命令以创建一个本地仓库的克隆版本：  
`git clone /path/to/repository`  
如果是远端服务器上的仓库，你的命令会是这个样子：  
`git clone username@host:/path/to/repository`

## 连接远程仓库

生成密钥：

`ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`

可以生成多个不同名字的密钥：

`ssh-keygen -t rsa -b 4096 -C "your_email@example.com" -f ~/.ssh/github_key1`

`ssh-keygen -t rsa -b 4096 -C "your_email@example.com" -f ~/.ssh/github_key2`

使用 `ssh-add` 命令将生成的密钥添加到 SSH 代理中。

`ssh-add ~/.ssh/github_key1`

`ssh-add ~/.ssh/github_key2`

在 `~/.ssh/config` 文件中配置不同的主机别名以及相应的密钥文件。编辑该文件并添加以下内容：

`# GitHub repository 1`

`Host github1`

    HostName github.com

    User git

    IdentityFile ~/.ssh/github_key1

`# GitHub repository 2`

`Host github2`

    HostName github.com

    User git

    IdentityFile ~/.ssh/github_key2

连接到github：

`ssh -T git@github.com`

添加远程仓库：

`git remote add origin <remote_repository_url>`

比如我已经在 GitHub 上创建了一个名为 `dichos` 的仓库，你可以使用以下命令将其添加为远程仓库：

`git remote add origin git@github.com:Dichgrem/dichos.git`

设置远程仓库

`git remote set-url origin git@github.com:Dichgrem/dichos.git`



## 工作流

本地仓库由 git 维护的三棵“树”组成。第一个是你的 `工作目录`，它持有实际文件；第二个是 `暂存区（Index）`，它像个缓存区域，临时保存你的改动；最后是 `HEAD`，它指向你最后一次提交的结果。

## 创建一个分支

`git branch main`

这将创建一个名为 main 的分支。

## 删除分支

`git branch -d master`

使用大写强制删除

`git branch -D master`



## 添加和提交

你可以提出更改（把它们添加到暂存区），使用如下命令：  
`git add <filename>`  
`git add *`  
这是 git 基本工作流程的第一步；使用如下命令以实际提交改动：  
`git commit -m "代码提交信息"`如：

`git commit -m "Initial commit"`  
现在，你的改动已经提交到了 **HEAD**，但是还没到你的远端仓库。

## 推送改动

你的改动现在已经在本地仓库的 **HEAD** 中了。执行如下命令以将这些改动提交到远端仓库：  
`git push origin master`  
可以把 *master* 换成你想要推送的任何分支。

如果你还没有克隆现有仓库，并欲将你的仓库连接到某个远程服务器，你可以使用如下命令添加：  
`git remote add origin <server>`  
如此你就能够将你的改动推送到所添加的服务器上去了。

## 合并分支

分支是用来将特性开发绝缘开来的。在你创建仓库的时候，*master* 是“默认的”分支。在其他分支上进行开发，完成后再将它们合并到主分支上。

创建一个叫做“feature_x”的分支，并切换过去：  
`git checkout -b feature_x`  
切换回主分支：  
`git checkout master`  
再把新建的分支删掉：  
`git branch -d feature_x`  
除非你将分支推送到远端仓库，不然该分支就是 *不为他人所见的*：  
`git push origin <branch>`

## 更新与合并

要更新你的本地仓库至最新改动，执行：  
`git pull`  
以在你的工作目录中 ***获取**（fetch）* 并 ***合并**（merge）* 远端的改动。  
要合并其他分支到你的当前分支（例如 master），执行：  
`git merge <branch>`  
在这两种情况下，git 都会尝试去自动合并改动。遗憾的是，这可能并非每次都成功，并可能出现_冲突（conflicts）*。 这时候就需要你修改这些文件来手动合并这些_冲突（conflicts）*。改完之后，你需要执行如下命令以将它们标记为合并成功：  
`git add <filename>`  
在合并改动之前，你可以使用如下命令预览差异：  
`git diff <source_branch> <target_branch>`

## 标签

为软件发布创建标签是推荐的。这个概念早已存在，在 SVN 中也有。你可以执行如下命令创建一个叫做 *1.0.0* 的标签：  
`git tag 1.0.0 1b2e1d63ff`  
*1b2e1d63ff* 是你想要标记的提交 ID 的前 10 位字符。可以使用下列命令获取提交 ID：  
`git log`  
你也可以使用少一点的提交 ID 前几位，只要它的指向具有唯一性。

## 日志

如果你想了解本地仓库的历史记录，最简单的命令就是使用:  
`git log`  
你可以添加一些参数来修改他的输出，从而得到自己想要的结果。 只看某一个人的提交记录:  
`git log --author=bob`  
一个压缩后的每一条提交记录只占一行的输出:  
`git log --pretty=oneline`  
或者你想通过 ASCII 艺术的树形结构来展示所有的分支, 每个分支都标示了他的名字和标签:  
`git log --graph --oneline --decorate --all`  
看看哪些文件改变了:  
`git log --name-status`  
这些只是你可以使用的参数中很小的一部分。更多的信息，参考：  
`git log --help`

## 替换本地改动

假如你操作失误（当然，这最好永远不要发生），你可以使用如下命令替换掉本地改动：  
`git checkout -- <filename>`  
此命令会使用 HEAD 中的最新内容替换掉你的工作目录中的文件。已添加到暂存区的改动以及新文件都不会受到影响。

假如你想丢弃你在本地的所有改动与提交，可以到服务器上获取最新的版本历史，并将你本地主分支指向它：  
`git fetch origin`  
`git reset --hard origin/master`



## 修改默认分支

git目前默认的主分支为 master，和 github 默认分支 main 不同，这使得默认配置下 git 往往连接失败。可以通过下两种方法改变默认分支。在本地 git init 时将默认分支修改成main

```
1. git --version  //查看版本
2. git config --global init.defaultBranch main  //将默认分支修改成main
3. git init //本地项目文件夹内创建.git文件夹
4. git add . //添加到暂存区
5. git commit -a [描述的内容]  //记录修改行为
6. git pull --rebase origin main  //拉github上的readme.md
7. git push origin main   //上传代码
```

也可以不修改git上的默认分支，而是修改github上库的默认分支。

## Windows下git使用代理

由于网络环境的差异，Git连接github需要代理，或者全局模式。Git支持四种协议，而除本地传输外，还有：git://, ssh://, 基于HTTP协议，这些协议又被分为哑协议（HTTP协议）和智能传输协议。对于这些协议，要使用代理的设置也有些差异：

- 使用 `git协议` 时，设置代理需要配置 `core.gitproxy`

- 使用 `HTTP协议` 时，设置代理需要配置 `http.proxy`

- 而使用 `ssh协议` 时，代理需要配置ssh的 `ProxyCommand` 参数

由于个人需求仅仅是 HTTP 的代理（相对来说，HTTP 有比较好的通适性，Windows 配置git/ssh比较棘手），设置的时候，只需要针对单个设置 `http.proxy` 即可，在需要使用代理的项目下面使用 `git bash` 如下命令进行设置（你的Uri和port可能和我的不同）：

`git config http.proxy` [http://127.0.0.1:2080](http://127.0.0.1:8088)  `# 也可以是uri:port形式`

这个是不需要鉴权的代理设置，如果需要鉴权，可能需要添加用户名密码信息：

`git config http.proxy` [http://username:password@127.0.0.1:2080](http://username:password@127.0.0.1:8088)

如果git的所有项目都需要启用代理，那么可以直接启用全局设置：

`git config --global http.proxy` [http://127.0.0.1:2080](http://127.0.0.1:8088)

为了确认是否已经设置成功，可以使用 `--get` 来获取：

`git config --get --global http.proxy`

这样可以看到你设置在global的 `http.proxy` 值。 

需要修改的时候，再次按照上面的方法设置即可，git 默认会覆盖原有的配置值。

当我们的网络出现变更时，可能需要删除掉原有的代理配置，此时需要使用 `--unset` 来进行配置：

`git config --global --unset http.proxy`

在命令之后，指定位置的设置值将会被清空，你可以再次使用 `--get` 来查看具体的设置情况。

如果使用了 HTTPS，肯呢个会碰到 HTTPS 证书错误的情况，比如提示： `SSL certificate problem` ，此时，可以尝试将 `sslVerify` 设置为 `false` ：

`git config --global http.sslVerify false`

恩，到此，可以试试 git 来获取/更改项目了，此时，项目应该是使用代理来进行通讯的。

## 注意

- 不要多次使用不同的参数来设置代理，一般使用文中两种方式酌情选用即可， `--global` ， `--system` ， `--local` 各级设置后，可能会给自己带来不必要的麻烦。git默认是先到 git Repository 的配置文件中查找配置文件，如果没有才会到 `--global` 设置的文件中查找，因此，单个项目文件中的设置会覆盖 `--global` 的设置。
- 使用 `--global` 来配置的信息保存在当前用户的根目录下的 `.config` 文件中，而仓库中的配置保存在项目仓库的根目录下的 `.git/config` 文件中。
- 如果是 Linux 的用户，建议全局代理。





