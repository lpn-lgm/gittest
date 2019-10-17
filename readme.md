Git

服务器

    一 Git简介
    二 安装Git
    三 自报家门
    四 客户端代码管理
    五 服务器端代码管理
    六 本地仓库代码推送到远程仓库
    七 团队协作
    八 版本回退
    九 分支管理

一 Git简介

Git是目前世界上最先进的分布式版本控制系统（没有之一）
1.1 何为分布式？与集中式相比有何特点？

以svn为例
mark
而git是这样的
mark
每个开发者的电脑上,都有完整的版本,日志,及分支信息.
但开发者不依赖于服务器,可以查看日志,回退版本,创建分支.
当然,世界各地的开发需要交换最新的版本信息,因此,git 往往也需要服务器.

但是,本质的区别在于:
git 服务器是供开发者"交换"代码,服务器数据丢了没关系,分分钟再建一台. svn的服务器,不仅交换代码,还控制着日志,版本,分支.服务器数据丢了就完了.
1.2 发展历史

Linux 之父 Linus Torvalds 在 1991 年创建了 linux 开源项目,并把项目放在互联网上,引来世界大量的黑客,大神为项目贡献代码.

问题是,这么多的人同时贡献代码,如何管理代码成了一件头疼的事.

随着 linux 内核的管理工作越来越吃力,linus 选择了一款商业版本控制器-BitKeeper.

BitKeeper 是 BitMover 公司旗下的产品.

公司的老大 Larry 也希望借机扩大产品的影响力,因此授权 Linux 社区免费使用 BitKeeper.

这件事,在开源圈引起了不小的骚动.

因为,BitKeeper 只是 free(免费),而非 free(自由).

开源教主 RMS 为这事儿还说过 linus.

2002 年 2 月,Linus 开始用它来管理 Linux 内核代码主线,Linus 对 BitKeeper 的评价是 the best tool for the job. 确实,自从 Linus 使用 BitKeeper 之后,Linux 的开发步伐加快了两倍.

可惜的是,就像黑帮电影中,老大蒸蒸日上的事业,往往坏在一个不懂事的小弟手中.

这帮视 free(自由)如信仰的牛人中,一个叫 Andrew 的,试图破解 BitKeeper 的协议,且被 BitMover 公司警告几次后仍不停手.

最终,出事了!

Linus 在 Andrew 和 Larry 两人间费力调停,但没有成功.

既如此,Linus 说:"我的兄弟只是做错事不是做坏事. 我扛!"

于是,10 天后,git 诞生了!
二 安装Git

git 在 Linux,Mac,Win 下都可以安装.

本文是以 Win7 系统为环境编写的.

Window 环境:

到 https://git-for-windows.github.io/ 下载软件, 双击,一路"Next",安

装完毕.到开始菜单找"git bash",如下图
mark

Linux 环境安装 git:

    # ubuntu,debian#
    $ sudo apt-get install git

centos,redhat 系统

    # yum install git

三 自报家门

人在江湖,岂能没有名号.

开源教主 Richard Matthew Stallman 的江湖名号 RMS.

在你用 git 之前,要先报家门,否则代码不能提交.

    $ git config --global user.name #你是谁
    $ git config --global user.email #怎么联系你

mark
四 客户端代码管理
4.1 工作区和版本库

Git和其他版本控制系统如SVN的一个不同之处就是有暂存区的概念。

先来看名词解释。

工作区（Working Directory）

就是你在电脑里能看到的目录，比如我的learngit文件夹就是一个工作区：
mark

版本库（Repository）

工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。
mark

分支和HEAD的概念我们一会再讲。

前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；
mark
第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。
mark
因为我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改。

你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。
五 服务器端代码管理
5.1 GitHub

我们一直用GitHub作为免费的远程仓库，如果是个人的开源项目，放到GitHub上是完全没有问题的。其实GitHub还是一个开源协作社区，通过GitHub，既可以让别人参与你的开源项目，也可以参与别人的开源项目。
5.2 GitHub创建远程仓库
5.2.1 注册并登录GitHub官网

    https://github.com/

5.2.2 创建远程仓库

点击new repository按钮创建仓库
mark
填写仓库名称、项目描述、是否公开仓库、是否用readme文件初始化仓库
mark

返回如下界面表示仓库创建成功
mark
5.2.3 查看远程仓库

查看远程仓库:git remote

查看仓库地址:git remote -v

例:

    git remote -v
    origin https://git.oschina.net/lianshou/test.git (fetch) origin https://git.oschina.net/lianshou/test.git (push)

5.2.4 删除远程库

命令:git remote remove <远程库名>

    示例:git remote remove origin

5.2.5 修改远程库名称

    git remote rename <旧名称> <新名称>

例:

    git remote rename online oschina

六 本地仓库代码推送到远程仓库
6.1 现在，我们根据GitHub的提示，在本地的test1仓库下运行命令：

    git remote add origin https://github.com/gitfenger/test1.git

请千万注意，把上面的gitfenger替换成你自己的GitHub账户名，否则，你在本地关联的就是我的远程库，关联没有问题，但是你以后推送是推不上去的，因为你的SSH Key公钥不在我的账户列表中。

添加后，远程库的名字就是origin，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。

下一步，就可以把本地库的所有内容推送到远程库上：

    $ git push -u origin master
    Counting objects: 19, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (19/19), done.
    Writing objects: 100% (19/19), 13.73 KiB, done.
    Total 23 (delta 6), reused 0 (delta 0)
    To git@github.com:gitfenger/test1.git
     * [new branch]      master -> master
    Branch master set up to track remote branch master from origin.

把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。

由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样：

从现在起，只要本地作了提交，就可以通过命令：

    $ git push origin master

把本地master分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！
七 团队协作
7.1 生成公私钥
7.1.1 设置Git的user name和email：

    $ git config --global user.name "jiaohuafeng"
    $ git config --global user.email "414582343@qq.com"

7.1.2 生成SSH密钥过程：

1.查看是否已经有了ssh密钥：cd ~/.ssh

如果没有密钥则不会有此文件夹，有则备份删除

2.生存密钥：

    $ ssh-keygen -t rsa -C "414582343@qq.com"

按3个回车，密码为空。

    Your identification has been saved in /home/tekkub/.ssh/id_rsa.
    Your public key has been saved in /home/tekkub/.ssh/id_rsa.pub.
    The key fingerprint is:
    ………………

最后得到了两个文件：id_rsa和id_rsa.pub

3.添加密钥到ssh：ssh-add 文件名

需要之前输入密码。

4.在github上添加ssh密钥，这要添加的是“id_rsa.pub”里面的公钥。

打开https://github.com/ ，登陆账号，然后添加ssh。
mark
5.测试：ssh git@github.com
7.2 添加项目协作者

mark
八 版本回退

你不断对文件进行修改，然后不断提交修改到版本库里，就好比玩RPG游戏时，每通过一关就会自动把游戏状态存盘，如果某一关没过去，你还可以选择读取前一关的状态。有些时候，在打Boss之前，你会手动存盘，以便万一打Boss失败了，可以从最近的地方重新开始。Git也是一样，每当你觉得文件修改到一定程度的时候，就可以“保存一个快照”，这个快照在Git中被称为commit。一旦你把文件改乱了，或者误删了文件，还可以从最近的一个commit恢复，然后继续工作，而不是把几个月的工作成果全部丢失。

每个文件/目录发生的版本变化,我们都可以追溯.
命令为:"git log "

常用格式:

    git log 查看项目的日志 
    git log <file> 查看某文件的日志 
    git log . 查看本目录的日志

例:

gitlog显示如下
mark
如果感觉 log 有点乱,可以 gitlog--pretty=oneline ,让日志单行显示.
mark

gitreflog 查看版本变化
mark

HEAD 指向当前版本 5d5df86
mark

mark

也可以利用版本号来切换,例

    $ git reset --hard 6207e59 HEAD is now at 6207e59 three

注意:版本号不用写那么长,能要能保证不与其他版本号重复就行.

例 git reset--hard 6207
九 分支管理
9.1 分支有什么用?

在开发中,遇到这样的情况怎么办?

网站已有支付宝在线支付功能,要添加"微信支付".

修改了 3 个文件, wechat.php ,pay.php

刚做到一半,突然有个紧急 bug: 支付宝支付后不能修改订单状态.你需要立即马上修改这个 bug,需要修改的文件是,ali.php ,pay.php .

问题是:pay.php ,已经被你修改过,而且尚未完成.直接在此基础上改,肯定有问题.

把 pay.php 倒回去? 那我之前的工作白费了.

此时你肯定会想: 在做"微信支付"时,能否把仓库复制一份,在此副本上修改,不影响原仓库的内容.修改完毕后,再把副本上的修改合并过去.

好的,这时你已经有了分支的思想.

前面见过的 master ,即是代码的主干分支,事实上,在实际的开发中,往往不会直接修改和提交到 master 分支上.

而是创建一个 dev 分支,在 dev 分支上,修改测试,没问题了,再把 dev 分支合并到 master 上.

如果有了分支,刚才的难题就好解决了,如下图:

mark

在做"微信支付"时,我们创建一个 wechat 分支.

把 wechat 分支 commit ,此时,master 分支内容不会变,因为分支不同.

当遇到紧急 bug 时,创建一个 AliBug 分支.

修复 bug 后,把 AliBug 分支合并到 master 分支上.

再次从容切换到 wechat 分支上,接着开发"微信支付"功能,开发完毕后,

把 wechat 分支合并到 master 分支上.
9.2 查看分支

查看所有分支 gitbranch
例

    git branch
    * master # 说明只有 master 分支,且处于 master 分支.

9.3 创建分支

创建 dev 分支 git branch dev

    git branch dev # 创建 dev 分支
    git branch #查看分支dev
    *master # dev 分支创建成功,但仍处于 master 分支

9.4 切换分支

切换到 dev 分支 git checkout dev

再次查看

    $ git branch
    *dev
    master # 已切换到 dev 分支上

9.5 合并分支

当我们在 dev 上开发某功能,并测试通过后,可以把 dev 的内容合并到 master 分支.例:
当前的 readme.txt 内容为"so so",在 dev 分支下,添加一行"from dev"

并提交

    git add readme.txt
    git commit -m "mod in dev"

再次切换到 master ,查看 readme.txt 的内容,仍为'so so'
合并 dev 分支,gitmergedev , 如下:

    $ git merge dev
    Updating c5364fe..412926b Fast-forward
    readme.txt | 1 +
    1 file changed, 1 insertion(+)

再次查看 readme.txt 的内容,已变为"soso from dev";
9.6 删除分支

    git branch -d dev
    Deleted branch dev (was 412926b).

9.7 快速创建和切换分支

    git checkout-b dev # 创建 dev 分支并立即切换到 dev 分支即起到 git branch dev 和 git checkout dev 的共同作用.