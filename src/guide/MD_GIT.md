---
title: Git 使用简明教程
type: guide
order: 1001
---

<p class="tip">供以后多核组新生看</p>

## 学会使用**Vmware**安装Linux 虚拟机
不喜欢**Vmware**的可以选择 VirtualBox 或 Parallel Desktop (for Mac)

虚拟机选择 Ubuntu 或者 CentoOs 都可以,或其他 Linux 发行版
>Ubuntu:
- 14 * gcc 版本太老 *
- 16或18  	[下载](http://mirrors.hust.edu.cn/ubuntu-releases/)	 *推荐64位*
## Git
- 为什么要使用 Git? [廖雪峰的Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001373962845513aefd77a99f4145f0a2c7a7ca057e7570000) *仅做参考*
- git 常用指令速查 [Git简明教程-简书](https://www.jianshu.com/p/16ad0722e4cc)

## 安装 git
`$ sudo apt-get install git` 或[网页链接下载Git](https://git-scm.com)
![](https://s1.ax1x.com/2018/06/14/CjibjI.png)
<p class="tip">Windows用户下载 Git 安装后可使用 `Git Bash`作为命令行,
其它命令行操作和 Ubuntu 命令行一样</p>
初次安装git需要配置用户名和邮箱，否则git会提示：please tell me who you are.
你需要运行命令来配置你的用户名和邮箱：
![](https://s1.ax1x.com/2018/06/14/CjiHgA.png)
<p class="tip">其实这里的 `user.name`不需要和你注册的账号相同,换句话说这里的 `user.name` 可以任意填写,不过当你有多台Git设备时可以通过定义这个 `user.name`来区分是由哪台设备上传的 `commit`,关于这一点在后文的 `git log`会有图片演示.
总之建议使用`"姓名缩写"."操作系统"`来作为 `user.name`,如`xxx.win10`</p>

## 生成 ssh 密钥 
为了连接 Github 我们需要使用 ssh [参考链接](https://www.cnblogs.com/superGG1990/p/6844952.html)
git使用ssh密钥时，免去每次都要求输密码的麻烦
使用`ssh-keygen -t rsa -C "your@email.com"`指令生成 ssh 密钥,生成密钥时遇到冒号提示输入内容的时候直接按Enter就好
![](https://i.loli.net/2018/06/14/5b2204fb8d82c.png)
用`cat ~/.ssh/id_rsa.pub`查看你的 ssh 公钥,复制下面字符串
![](https://i.loli.net/2018/06/14/5b2205ae66151.png)
登陆你的github帐户。点击右上角你的头像，然后 `Settings -> 左栏点击 SSH and GPG keys -> 点击 New SSH key`,把上面复制的公钥内容，粘贴进“Key”文本域内。 title域，自己随便起个名字例如上文的 user.name。点击 Add key。
完成以后，验证下这个key是不是正常工作：
```
$ ssh -T git@github.com
Attempts to ssh to github
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
Are you sure you want to continue connecting (yes/no)? yes
```
如果看到：
`Hi xxx! You've successfully authenticated, but GitHub does not # provide shell access.`
恭喜你，你的设置已经成功了(如果你没有忘记设置`user.name`和`user.email`的话)。

## 下载项目
```bash
# 简单的 clone 项目使用 git clone 即可
$ git clone XXX/XXX.git 
# 若要编辑实验室网站文档则需安装 nodejs 和 hexo
$ npm install -g hexo-cli hexo-server #hexo是一款流行的博客生成工具,用来把.md生成.html 静态网页
$ git clone git@github.com:DML308/cn.costream.org.git #中文站点
$ git clone git@github.com:DML308/costream.org.git    #英文站点
$ cd costream.org
$ npm install #hexo 工具的package.json里定义了一些Dependencies插件的名字,这些插件并不会把包内容上传github而只是上传它们的名字和版本号以节省网络空间, 所以此时在本地把它们按照定义好的规则下载下来
$ hexo serve #开启 localhost:4000以后这是一个本地的 Web 服务器,如果按下 Ctrl+C 那么该服务器就会停止. 如果既想开着 hexo serve 又想动 git 那么最好开两个Git Bash 窗口
```
## 修改 & 提交
```bash
# edit your code
$ git add -A
$ git commit -m "your comment" 
$ git pull #拉取别人修改的代码,无冲突时默认合并
#不建议一点小改动就 commit-push ,熟练以后我们可以本地修改多次并本地commit多次后再执行push一次就ok
$ git push
```
## 版本回滚
```bash
$ git reset --hard
```

## 学习要求✭
- 基本要求: 熟练使用`git clone | status | add | commit | push | git reset --hard` *这里足够80%日常使用了*
- 进阶要求: 了解**分支**,使用`git branch | checkout`等指令
- 更高要求: 了解开源项目的`tag`,`release`,`Pull Request`等项目版本管理内容
- 边角料 : 在使用时遇到问题可以随时百度`git 版本回滚`,`.gitignore`,`git 分支合并`,`git log 详解`等内容

## 更多
### 更好看的 git log
```bash
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit" 
```
现在你每次在终端输入`git lg`，就能看到下面漂亮的git log了。
![git-lg](https://i.loli.net/2018/06/14/5b22158769764.png)
>需要注意的是:
- 前文设置的 user.name 这里会出现在 `git log` 记录里, 所以说给不同用户和设备设置不同的 user.name 是有益的.
- 专业团队的`git commit`的`comment`要求清晰、风格统一, 可以参考[阮一峰的网络日志-Commit](http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)
### 其他
```bash
git config --global alias.st "status"  #简写 git st
git config --global core.editor "vim"  #设置 vim 为git默认编辑器
```
### for Mac:
Mac 用户可以通过 SSH 来连接自己的Vmware虚拟机 
*为什么要多此一举呢,是因为这里有超好看的终端 iTerm2,配置可以参考[ITerm2配色方案-简书](https://www.jianshu.com/p/33deff6b8a63)*
1. **Ubuntu安装 openssh-server**
```bash
$ sudo apt-get install openssh-server
$ ps -e | grep sshd #查看 sshd 服务是否启动
```
1. **Mac 修改 hosts**
```bash
$ sudo vim /etc/hosts
# 202.114.18.98 dell #vim操作保存退出
$ ssh username@dell #
```
1. **ssh 免密码登录**
复制 Mac 的 `cat ~/.ssh/id_rsa.pub`内容,复制入Linux 上的`vim ~/.ssh/authorized_keys`文件即可实现免密连接,连接时使用`ssh username@dell`即可,其中`dell`是上一步添加的 hosts 名字

### for Linux:
目前常见的一个问题是*中文 windows *上装的*虚拟机 linux* 上 vscode 的控制台`git lg`或`git log`出现乱码, 乱码格式一般为`<A1><E6>`的形式,解决方法:
在`~/.gitconfig`中添加如下内容:
```
[core]
    quotepath = false
[gui]
    encoding = utf-8
[i18n]
    commitencoding = utf-8
[svn]
    pathnameencoding = utf-8
```
执行
```
export LESSCHARSET=utf-8
```

### for Windows:
Windows的命令行也是能用的且功能齐全,缺点就是不是很好看.
**推荐的工具**
- `VSCode` `Brackets`*推荐程度一般* 
   这些编辑器人各有所爱, 注意**编辑器不是IDE**, 编辑器不含开发环境, 本质上和**vim**无区别, 编译出错时请更多考虑自己编译流程中的问题.
- `Bitvise` *强烈推荐✭✭✭✭✩*
一个软件就包含了`ssh 连接`+`打开命令行`+`与服务器传文件`, ~~妈妈再也不用提醒我下载 FileZilla~~
- `WOX` 	*超级强烈推荐✭✭✭✭✭*
 使用`alt+space`来打开窗口执行快捷命令,功能强大,爱不释手,[http://wox.one/](http://www.wox.one/),如果觉得好用记得在 Github 上给一个✭,现在已有<iframe src="https://ghbtns.com/github-btn.html?user=Wox-launcher&repo=Wox&type=star&count=true" frameborder="0" scrolling="0" height="20px" style="vertical-align:bottom;line-height:20px;margin:1px 0px"></iframe>
 ![](https://camo.githubusercontent.com/9db33546d3a905a9ad915e0948d3ba3f47f57b64/687474703a2f2f692e696d6775722e636f6d2f4474784e424a692e676966)
