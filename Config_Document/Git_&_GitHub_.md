# Git

[维基百科 - Git](https://zh.wikipedia.org/wiki/Git)

## Git 介绍

- [Git 教程 - 廖雪峰](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/)
- [Pro Git](http://git.oschina.net/progit/)
- [git - 简明指南](http://rogerdudler.github.io/git-guide/index.zh.html)
- [猴子都能懂的 GIT 入门](http://backlogtool.com/git-guide/cn/)

## Git 使用交互流程

![git交互模型](http://localhost:9710/blog/Config_Document/git.png)

## 安装和配置 Git 环境

下载地址：https://git-scm.com/

### git-bash 常用命令

git-bash

### 初始化 Git 仓库

```shell
# 找到目标文件夹并进入

# 右键  选择 Git Bash Here

git init

git init 项目名称
# 自动创建 项目目录 并且 初始化git 仓库

```

### 配置个人信息

在用户目录下生成 `.gitconfig` 文件

```shell

# 方便每次备份将使用者信息备份起来，方便操作

# 设置用户名
git config --global user.name "answer-zf"

# 配置用户邮箱
git config --global user.email "feng18255163789@gmail.com"
# 设置 gitk 图形查看工具中文显示默认编码（防止乱码）
git config --global gui.encoding utf-8

# 查看配置列表项
git config --list

```

## 基本使用

### 将文件备份到 Git 仓库中

**操作 Git 的基本工作流程就是先修改文件，然后执行 `git add` 命令。**
**`git add` 命令会把文件加入到暂存区，接着就可以执行 `git commit` 命令，将文件存入文档库，**
**从而形成一次历史记录。**

**暂存区：**

- 可作为临时版本库
- 可将修改的多个文件分多次提交
  - 按照不同的修改分类提交

```shell

# 先存放置暂存区
$ git add ./readme.md

# 再提交到版本库
$ git commit -m "complete the first function！"    # -m : 说明信息（便于查看）

----------------------
 	# 不输入 -m 会进去vim编辑器环境输说明信息。
 	# 在vim里输入说明信息后 Esc键 + :Wp 即可    # w：保存 p：退出
 	# 强制退出：Esc键 + :p

##**************************************************************************

# 把所有的文件存放到暂存区
$ git add --all
$ git add .
$ git add ./	# 当前文件夹所有文件

# 一次性把所有修改后的文件直接放到版本库中
$ git commit --all -m "这是一次性操作"        # --all : 把所有修改后的文件直接放到版本库中
# 前提所有文件均被版本管理（提交过一次）

# 使用一次新的commit，替代上一次提交 （修改commit的提交信息）
# 如果代码没有任何新变化，则用来改写上一次commit的提交信息
$ git commit --amend -m [message]

# 重做上一次commit，并包括指定文件的新变化
$ git commit --amend [file1] [file2] ...
```

### 删除、修改工作区文件

```bash

# 删除工作区文件，并且将这次删除放入暂存区
$ git rm [file1] [file2] ...

# 停止追踪指定文件，但该文件会保留在工作区
$ git rm --cached [file]

# 改名文件，并且将这个改名放入暂存区
$ git mv [file-original] [file-renamed]

```

### 查看状态

```bash

$ git status

# Changes not staged for commit
# modified （红色）  =>  文件已修改，但未放入暂存区

# Changes to be committed
# modified （绿色）  =>  文件已经放入暂存区

# nothing to commit, working tree clean  ==> 文件已经在工作区

```

### 查看提交日志

```shell

# 查看历史日志
$ git log

# 查看精简日志，单行显示
$ git log --oneline

# **************************************************

# 日志过长会自动进入 less 查看模式

# 查看每次切换版本记录
$ git reflog

```

### 打开图形界面

```shell
$ gitk
```

### 撤销、回滚

```bash
# 恢复暂存区的指定文件到工作区
$ git checkout [file]

# 恢复某个commit的指定文件到暂存区和工作区
$ git checkout [commit] [file]

# 恢复暂存区的所有文件到工作区
$ git checkout .

# 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
$ git reset [file]

# 重置暂存区与工作区，与上一次commit保持一致
$ git reset --hard

# 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
$ git reset [commit]

# 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
$ git reset --hard [commit]

# ex-----------------------------------------
$ git reset --hard Head~0    # Head~0   回退到上一次提交代码时的状态
							 # Head~1   回退到上上一次提交代码时的状态

							 # hard     允许工作区代码被以前的代码覆盖

# 可以通过版本号精确回退到某一次提交的状态
$ git reset --hard b0750f9   ## b0750f9 ：版本号
# -------------------------------------------

# 重置当前HEAD为指定commit，但保持暂存区和工作区不变
$ git reset --keep [commit]

# 新建一个commit，用来撤销指定commit
# 后者的所有变化都将被前者抵消，并且应用到当前分支
$ git revert [commit]

# 暂时将未提交的变化移除，稍后再移入
$ git stash
$ git stash pop
```

## 分支（ 默认有一个主分支 master）

### 分支基础

默认在 git 的仓库中，会有个分支的原点：master

```shell

# 列出所有本地分支
$ git branch

# 基于当前分支新建一个分支，但依然停留在当前分支
$ git branch [branch-name]

# 基于当前分支新建一个分支，并切换到该分支
$ git checkout -b [branch]

# 切换到指定分支，并更新工作区
$ git checkout [branch-name]

# 切换到上一个分支，交替和上一个分支进行切换
$ git checkout -

# 合并 指定分支 到 当前分支
$ git merge [branch]

# 删除分支(不能是当前所处分支)
$ git branch -d [branch-name]

```

- 对于没有提交到暂存区的工作区的修改文件，无论切换到那个分支，内容不变，处于待提交到暂存区状态
- 对于没有提交到版本库的暂存区的文件，无论切换到那个分支，内容不变，处于待提交到版本库状态
- 也就是说 只有 commit 到的指定分支 才能对该分支进行操作

### 合并分支冲突

```shell

## 在dev分支提交了代码以后
## 又在主分支master提交了代码
## 合并分支以后，需要手动处理，在提交到主分支中

```

## Git 工作流程：分支策略

[Git 工作流程](http://www.ruanyifeng.com/blog/2015/12/git-workflow.html)

### Git Flow

[Git 分支管理策略](http://www.ruanyifeng.com/blog/2012/07/git.html)

### Github Collabrators

这种方式公司团队项目使用居多

- 进入项目
- settings - > Collaborators
- 添加 协作者

### Github Flow

这种方式开源项目使用居多

- fork
- clone 到你的本地
- 在 clone 下来的项目中拉出一个新的分支
  - 修改的时候最好是基于 master 拉出一个修改的分支，例如这个分支是用来添加某个功能的
- 在新分支上开发或者修改完成之后，提交到本地仓库，然后 push 推到自己的账户中 fork 过来的仓库
- 最后，在 Github 上你 fork 过来的仓库界面中找到 New Pull Request 发起提交请求
- 对方就会在仓库的 Pull Requests 中收到你发起的提交请求
  - 然后双方就可以使用社会化交流方式进行沟通协作
  - 例如 Code Review 代码审查
- 最后对方审查通过没有问题之后，选择 Merge Request
- 到此，一个完整的 Github 工作流结束
- 这种方式开源项目更多一些（大家都不认识）

### Gitlab Flow

#### Gitlab

[Gitlab](https://about.gitlab.com/)

Gitlab 是一个开源的类似于 Github 的一个系统，开源免费部署到自己的公司内容。

## GIT 报错集合

### 将文件存放置暂存区警告：

> warning: LF will be replaced by CRLF in Config_Document/VS Code_Config/.vscode/settings.json.
>
> The file will have its original line endings in your working directory

解决：

```shell
$ git config --global core.autocrlf false
```

### Git Bash 中文乱码：

![Snipaste_2019-10-27_14-04-08](http://localhost:9710/blog/Config_Document/Snipaste_2019-10-27_14-04-08.png)

# GitHub

**让你使用社交化的方式进行编程协作**

**主要作用：**

- 可以免费在线托管你的仓库
- 可以实现多人协作
- 提供了一个可视化界面（Web Page）让你能直观清晰的了解你的项目源代码

## 基本使用

- 注册
- 登陆
- 创建远程仓库
- 通过 `git clone` 命令下载远程仓库到本地
  - git clone 会自动帮你把远程仓库下载到本地，不需要再去 git init 了
  - 通过 clone 下来的仓库，git 有一个远程仓库地址列表，git 默认会把你 clone 的地址起一个别名：origin
  - 然后你执行 push 的时候实际上就是将本地的版本提交到 origin 上
- 在本地进行操作，通过 `git commit` 形成历史记录
- 通过 `git push` 将本地仓库中的历史记录提交到远程仓库

## 本地已有仓库，需要提交到线上

如果是本地 `git init` 出来的仓库，进行 `push` 提交的时候就不知道要往哪里 push。

所以，这里通过 `remote` 相关命令进行设置：

```bash
# 查看所有的远程仓库信息
$ git remote show
# 根据别名查看指定的远程仓库信息
$ git remote show 远程仓库地址别名
# 添加远程仓库信息
$ git remote add 别名 远程仓库地址
```

通过上面的 `git remote add` 添加完远程仓库地址信息之后，还不能直接 `git push`，必须在每一次
`push` 的时候加上 `git push 仓库地址别名 master` 就可以提交了。

如果想要省略 `git push` 后面需要指定的 `仓库地址别名 master` 可以通过下面的命令修改：

```bash
$ git push --set-upstream origin master
```

这样就可以直接使用 `git push` 进行提交而不需要指定 `origin master` 了

## GitHub 远端交互

### 基本操作

```bash
# 下载一个远程仓库
$ git clone [url]

# 显示所有远程仓库
$ git remote -v

# 显示某个远程仓库的信息
$ git remote show [remote]

# 增加一个新的远程仓库，并命名
$ git remote add [shortname] [url]

# 修改远程仓库地址
$ git remote set-url [shortname] [url]

# 取回远程仓库的变化，并与本地分支合并
$ git pull [remote] [branch]

# 上传本地指定分支到远程仓库
$ git push [remote] [branch]

# 强行推送当前分支到远程仓库，即使有冲突
$ git push [remote] --force
```

### Clone 与 Pull

**Pull**

```bash
# 开发优先推荐

# 新建文件并初始化 git仓储

# 从服务器下载 git

$ git pull https://github.com/asnwer-zf/Common-Document.git master
```

**Clone**

```bash

# 不用初始化git仓储，会在本地新建根文件夹
# 如果多次执行会覆盖本地内容
$ git clone https://github.com/asnwer-zf/Common-Document.git
```

### Push 与 Pull 简化操作

```bash

# 增加一个新的远程仓库，并命名 后
$ git push origin master

##*********************************
# -u 参数关联
$ git push -u origin master
# 后面的提交简化：
$ git push
# 仍然提交上去了
# -u：把当前分支与远程的指定分支进行关联，即：记住操作
# 关联以后 git pull 同样可以

## -u 关联在push中进行

```

### Https 上传 与 SSH 上传

**Http 上传**

```shell

# 格式 git push + url地址 + master    master指的是 将本地master分支提交到服务器master分支
$ git push https://github.com/answerooo3/Common-Document.git master

```

**SSH 上传**

```shell

# 优势：不需要输入用户名密码，依旧可以进行身份验证

# 公钥（GitHub存放），私钥（个人存放）
# 生成公钥，私钥
$ ssh-keygen -t rsa -C "feng18255163789@gmail.com"

# 三个回车生产秘钥
# 找到C:\Users\answer_zf\.ssh内的
# id_rsa	    私钥
# id_rsa.pub   公钥

# 复制 id_rsa.pub 内代码
# 进去GitHub => 点击我的头像 => Settings => SSh and GPG keys => NEW SSH key
# 添加已复制的代码 起个名字（Title内填写） => Add SSH key
# 再用push上传即可 （传参为SSH地址）

# 换主机：
# 在路径  C:\Users\ + 主机用户名  的目录下找到 .ssh文件
# 在目标主机更换.ssh 文件

```

## GitHub 多人开发的冲突问题

```shell

## 修改同一个文件的冲突
## 先pull到本地手动修改冲突后 ，再push到GitHub

## 修改不同文件的冲突
## 先pull到本地进入vim填写说明系统自行合并 ，再push到GitHub

```

## GitHub Pages

Github Pages 提供了一个免费在线托管静态资源的服务，叫做：Github Pages。

使用方法如下：

1. 在个人的 Github 账户中创建一个仓库
2. 仓库名称必须为 `个人账户名称.github.io`
3. 往该仓库根目录中提交一个 `index.html` 文件
4. 然后就可以在地址栏输入 `个人账户名称.github.io` 地址，就可以看到 `index.html` 网页内容了

注意：上面创建的仓库名称必须是 `个人账户名称.github.io` ，否则无法访问
