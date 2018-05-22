# 【宅印团队】动手用 git flow 工作流进行团队协作开发
# 简介
<p>
Git的出现，使软件开发协作变得非常灵活方便，使用git 可以说是当今程序员的必备技能。Git 是一个布式版本控制系统，只要在团队内部约定特定的工作流程，即可很方便、很有条理地进行团队协作开发。这里是介绍几种工作流的两篇文章：[《Git 工作流程》（By 阮一峰）](http://www.ruanyifeng.com/blog/2015/12/git-workflow.html)， [《常见工作流比较》（翻译 By 童仲毅）](https://github.com/geeeeeeeeek/git-recipes/wiki/3.5-%E5%B8%B8%E8%A7%81%E5%B7%A5%E4%BD%9C%E6%B5%81%E6%AF%94%E8%BE%83)。
我们团队也使用 git 作为团队开发协作工具，并使用 git flow 作为工作流。接下来我们从 git 的安装和配置开始, 通过 *简单任务* 的形式，一步步让大家在实践中使用 git flow 进行团队项目开发。

下面我们先来看看我们要来一起完成的**集体任务**：一个超简单的 ["团队成员介绍网站"](http://birdylee.github.io)。该网站的架构很简单，包含一个主页和许多个团队成员的介绍页面，网站的原型如下：

![网站主页： /index.html](http://upload-images.jianshu.io/upload_images/1234637-59553b6e80d8c82c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![个人介绍页： /members/xiaoming](http://upload-images.jianshu.io/upload_images/1234637-a1c50a808a139758.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


这里是已经部署的网站地址: [http://birdylee.github.io/](http://birdylee.github.io/), 点开看看有哪些人的大名已经在上面了！每个 **个人介绍页** 是由不的同人各自开发的，并通过我们将要介绍的工作流更新到我们的这个网站里面！跟着这个教程一步步做，你也可以在上面放上你的专属页哦！~

## 1、安装和配置 git
Git 几乎可以在任何平台上使用，这里是 [Git 官方下载地址](https://git-scm.com/download/)。如果下载较慢，可以从[国内镜像](http://pan.baidu.com/s/1skFLrMt#path=%252Fpub%252Fgit)下载(感谢廖雪峰老师的维护)，下载后按默认选项安装即可。
如果你是 windows 用户，git 安装后，可以在任意目录的右键菜单找到 *git bash here* 选项，如下：

![git bash here 选项](http://upload-images.jianshu.io/upload_images/1234637-af9cbb2e7a460003.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击即可在该目录打开 git 命令终端：

![git 命令终端](http://upload-images.jianshu.io/upload_images/1234637-80a97378dbbfb5ab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

接下来我们来配置全局git基本信息，包括用户名和邮箱，运行如下命令（<...>表示需要你自行填写）：
`$ git config --global user.name <你的名字>`
`$ git config --global user.email <你的邮箱>`

这些信息指明你的身份，这样当你把代码分享给出去的时候，别人可以通过这些git信息获悉是哪些代码是你写的，并可以通过邮箱联系你。


## 2、创建git仓库
git仓库（git repository）是一个项目目录，这个目录被 git 作为一个单位管理起来，该目录下的每个文件都可以被 git 跟踪。 git 仓库分为 *本地仓库* 和 *远程仓库*，下面我们都来动手创建一个。为了演示协作工作流，我们从远程仓库开始讲起。

#### 2.2 创建远程(云端) git仓库：
git远程仓库就是放在 git 服务器的 git 仓库。目前流行的 git 托管服务提供商有 [github](http://github.com), [gitlab](http://gitlab.com) 等，国内的有 [git@osc(码云)](http://git.oschina.net/), [coding](https://coding.net/) 等，我们**这个项目**将使用 github 存放项目代码 (若无github账号，请注册一下，程序员必备)。
创建远程仓库有两种方式：
- **生成一个空的远程仓库**
- **从其他仓库fork到自己的账户下**

前者请参看git个人主页，这里为了演示协作工作流，介绍后者。
首先，我们的项目会有一个创建好的 **主仓库**，该仓库由项目管理员管理，用来整合来自各个团队成员的代码，并对外发布。这个仓库有两个分支，dev 分支和 master 分支，dev 分支用来收集来自各个成员的更新，可以做到每夜更新，当 dev 分支达到特定的版本要求时，就可以合并入master 分支来进行发布一个版本。图示如下：

![主仓库分支演进](http://upload-images.jianshu.io/upload_images/1234637-02dcb449cd73b984.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

只有项目管理员才有对这个仓库的 “写权限”， 其他普通成员只有对该仓库 “读权限”。
我们的学习任务主仓库在这里：https://github.com/BirdyLee/birdylee.github.io

接下来，我们开始动手把自己的自我介绍的代码放到该仓库里面！我们先来介绍 **"fork"** , 简单的理解就是复制一个仓库到自己的 git 账户(组织)下，这样子你就拥有了对该仓库的写入权限，我们来动手试试：
- 打开主仓库地址： https://github.com/BirdyLee/birdylee.github.io ，点击右上角 “fork”

![主仓库](http://upload-images.jianshu.io/upload_images/1234637-98f419e01a290151.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

看到如下界面时，你已成功地把主仓库地内容复制到自己的账户下，这时你对该仓库有**读写权限**：
注意到在仓库名称下面多了一行：'' forked from BirdyLee/birdylee.github.io" ,表明这个仓库的 fork 来源。

![对应的项目在自己的仓库下](http://upload-images.jianshu.io/upload_images/1234637-1e7f02521c712853.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

接下来，我们就可以在自己的仓库里修改代码了~当然，我们要把项目文件拉到本地来进行开发，下一节将介绍本地仓库。

#### 2.2 创建本地 git仓库：
git 本地仓库就是存放在你的电脑里的git仓库。
创建本地仓库有两种方式：
 - **从本地现有文件夹创建**
 -  **拷贝远程仓库到本地**

前者请参看[廖雪峰git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013743256916071d599b3aed534aaab22a0db6c4e07fd0000) ，这里为了演示工作流，演示如何从远程仓库获取。

从远程仓库获取项目代码一般有两种连接方式，HTTPS 和 SSH , 为了节省一些配置，在此使用 HTTPS 进行演示。关于SSH连接方式，请参看[这里](http://cuiqingcai.com/423.html)。
登录 **你的账户** 下的对应项目仓库，复制项目的 HTTPS 地址：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234637-130d54c7de0f8326.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

选择存放项目的父文件夹（如桌面），右键打开本地git bash终端，运行
 `$ git clone https://github.com/<你的用户名>/birdylee.github.io.git`
（clone 后面的内容直接张贴即可）
运行成功后，可以看到生成一个目录 “birdylee.github.io”, 该目录就是项目目录，是一个被 git追踪的文件夹（可以暂时理解为本地git仓库），打开该目录，可见目录结构如下：


![目录结构](http://upload-images.jianshu.io/upload_images/1234637-ed5f0ac3c20dc098.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

项目中的 members 目录保存着两个模板（_template-advanced，_template-basic）和各个成员的文件夹（xiaoming, xiaojiu 等），打开项目根目录 index.html 就可以看到网站的样子。


![主页](http://upload-images.jianshu.io/upload_images/1234637-5d4daf139db7ca18.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 3. 写入自己的代码

下面我们来看看，怎么把自己的名字加上去，我们使用基础模板做演示：

打开members文件夹，在里面新建一个文件夹，命名为你想要的名字（最好是英文，不冲突即可） ，比如 xiaobai:

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234637-1898b6f9779349b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

拷贝 members/_template-advanced 目录下的 index.html 到 xiaobao 目录下，并 **用编辑器** 打开 members/xiaobai/index.html，
把这三个地方修改成你的信息，可自由发挥：
![修改前](http://upload-images.jianshu.io/upload_images/1234637-f3364c8b43ee8284.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![修改后](http://upload-images.jianshu.io/upload_images/1234637-64a3cb3d711458ec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

修改后记得保存，这样就做好了你的介绍界面，接下来我们在主页里添加一个按钮链接到该页面即可,用编辑器打开**项目根目录**下的 index.html, 按照已有a标签的格式添加一个a按钮，把中间的文件夹名称改成你的：

![添加一个 a 标签： <a href="members/xiaobai/index.html">小白</a>](http://upload-images.jianshu.io/upload_images/1234637-20a83e111bd62644.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

保存！大功告成，你可以用浏览器打开它预览啦~

![主页](http://upload-images.jianshu.io/upload_images/1234637-ab7ddf54b1cfe39c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![小白介绍页](http://upload-images.jianshu.io/upload_images/1234637-5d461e9a30c39a70.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

还有高级模板，大家可以试试哦~

## 4. 逐步提交

接下来我们要把我们的修改逐层提交上去，提交流程如下，一共3步：

![git 提交流程](http://upload-images.jianshu.io/upload_images/1234637-1d2b60fa443ab84a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们来一步一步操作！

#### 4.1 把修改提交到本地 git 仓库

在你的项目目录里，你可以发现一个 .git 文件夹（需要显示 隐藏文件夹 才可以看见），这个文件夹就是一个实质性的git本地仓库，里面保存着该项目目录的版本、分支信息：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234637-698afe7f7ebb1318.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

首先，我们要把我们对该目录所做的修改生成一个提交(commit)，存入本地仓库，分两小步，原理如下：


![提交一个版本](http://upload-images.jianshu.io/upload_images/1234637-f5e12145ce184d2b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在项目根目录打开git bash, 首先，我们使用 add 命令 `$ git add --all` 把所有修改存入暂存区，准备生成一个提交。接下来我们使用commit命令 `$ git commit -m "<一些备注>"` （如 `$ git commit -m "来自小白的更新"`）把暂存区的修改生成一个提交(commit)。这样完成了第一步了，你对项目所做的修改已经在本地git仓库保存成一个提交版本。

#### 4.2 把本地 git 仓库 提交到 你的远程git 仓库

接下来，我们把本地 git 仓库 提交到 你的远程git 仓库, 运行如下命令
`$ git push`
因为我们是用https方式连接的，所以需要输入账密，请按提示输入。
（提示，运行 `$git config --global credential.helper store` 可以长期保存输入的密码，下次输入账密之后，以后就不用输入了。）
（虽然https连接方式被号称比较快，但本人还是推荐使用[ssh连接方式]((http://cuiqingcai.com/423.html))，这样可以不用在电脑保存你的 github 账密。）

![把新的 commit 推送到 远程仓库](http://upload-images.jianshu.io/upload_images/1234637-e98fdfb4a118c5cf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
dssd

这个操作可以定期完成，如每晚一次，这样可以大大减少由于你的电脑故障带来的代码损失。

#### 4.3 通过 pull request 把你的远程git仓库更新提交到主git仓库
当你的代码成功的完成某个指定的小功能时，你就可以申请把你的新代码提交到主仓库了~流程图如下：


![通过 pull request 提交更新](http://upload-images.jianshu.io/upload_images/1234637-d09d7b3e728996f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
首先，登录你的github仓库，点击 “New pull request”：

![](http://upload-images.jianshu.io/upload_images/1234637-de14d9e66e84dc9d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后就会出现一个 **改动比较界面**，在标题下面这一栏选择合并到哪个分支，我们选择 dev :

![base fork 选择 BirdLee/birdylee.github.io, base 选择 dev](http://upload-images.jianshu.io/upload_images/1234637-19ce99b158923da3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这样的意思是把你的账户下的 birdylee.github.io 仓库里的 master 分支的更新合并到 BirdLee/birdylee.github.io 的 dev 分支。当出现 “ **Able to merge** ” 时，即可自动合并，这是你可以直接点击 " **Create pull request** "。填入更新说明和详情，即可成功创建 pull request：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234637-b83f77acd22e0941.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这样项目管理员和其他项目成员就可以清楚的看到你对项目所做的修改，并进行讨论或评论：

![pull request 概况](http://upload-images.jianshu.io/upload_images/1234637-8db89f61603e01b8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![查看变更的代码](http://upload-images.jianshu.io/upload_images/1234637-3310bea87ee3a90a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

创建 pull request 后，项目管理员就会收到邮件通知，并开始审核你的 pull request。当代码符合要求时，管理员会接受这个 pull request，这时，你的代码就被整合到 主 git 仓库，这样你就对该项目做出了自己的贡献了！这时登录 [http://birdylee.github.io](http://birdylee.github.io/)，你就可以看到你专属的按钮了 ~

就这样，我们就跑了一遍git flow工作流，是不是觉得还挺简单的呢~ 整个 git flow，可以总结为下面这图：

![git工作流 （摘自阮一峰博客）](http://upload-images.jianshu.io/upload_images/1234637-799154956fa5c09e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面介绍的 git flow 是基本完整的，但为了尽量简单，暂时忽略了诸如 *代码合并冲突* ，*定时 rebase* 等问题，这些问题大家当遇到时再学习如何操作即可。

你做成功了吗~？如果有任何疑问，欢迎在项目的 [Issues](https://github.com/BirdyLee/birdylee.github.io/issues) 提出哦~
