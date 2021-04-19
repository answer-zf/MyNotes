# 使用 VMware 安装 Linux

## VMware 安装重点：

-   稍后安装操作系统
-   点选将虚拟磁盘存储为单个文件(O)
-   点击“编辑虚拟机设置”
    -   点选“使用ISO映像文件(M)”，并添加 iso 镜像
-   VM Ware 网络配置
    1.  网络适配器：桥接模式
    2.  编辑 => 虚拟网络编辑器 => 更改设置 => Vmnet0 桥接到：自动
    3.  虚拟机 => 设置 => 勾选 启动时连接 和 复制物理网络连接状态
-   使用 VM Ware 虚拟机，安装 Ubuntu 系统，（切换中英文输入法 win + space）

## Linux7 安装

-   配置时区 (DATE & TIME)
    -   时区设置为 Region：Asia    City：Shanghai
    -   Network Time ：
-   设置软件选择 (SOFTWARE SELECTION)
    -   选择：Minimal install or Gnome Desktop
-   设置安装位置 (INSTALLATION DESTINATION)
    -   I will configuire parttioning => down
    -   添加 /boot 分区 => 存放启动文件，例如内核kernel
    -   添加 swap 分区 => 交换分区，虚拟内存，当内存耗尽时，把硬盘当内存用(8G/16G)
    -   最后 添加 根分区(`\`, 不填大小即剩余空间都给根)
-   KDUMP: 关闭
-   NETWORK & HOSTNAME:
    -   配置：常规第一个选项勾选

# Linux

1.  操作系统(os)的作用：管理计算机硬件与软件资源的计算机程序。（ 跑在硬件上，给其他软件提供平台，让其他软件更容易接触操作系统的硬件 ）

2.  Linux操作系统的组成

    -   Linux 内核：Linux 操作系统的核心代码
    -   文件系统：管理磁盘数据的系统，可以将数据以目录或文件的型式存储。每个文件有自己的特殊格式与功能。
    -   命令行 shell：接收用户命令，然后调用相应的应用程序，并根据用户输入的指令来反馈给用户信息。
    -   图形界面
    -   桌面环境
    -   一系列应用工具和软件

3.  Linux 系统快捷键

    -   打开终端 Ctrl + Atl + T
    -   打开同级目录下的终端 Ctrl + Shift + N
    -   切换终端 Atl + Tab

4.  安装 gz 源码包

    -   `tar -zxvf django-cors-headers-3.0.2.tar.gz` # 解压
    -   `cd django-cors-headers-3.0.2.tar.gz`
    -   `python3 setup.py install` # 安装

5.  安装 whl 文件

    -   `pip install django_cors_headers-3.0.2-py2.py3-none-any.whl`

## shell 命令行

1.  命令行组成：

    ```bash
        命令行：  ...  @   ...   :     ~   ...   $
                用户名   计算机名      当 前 路 径
                                     ~ ：家目录/主目录（home/用户名）
    ```

2.  修改更目录下的内容需要加管理权限，即命令前增加 sudo

3.  常用命令：

    -   cd
        -   作用：切换工作目录 (默认切换到主目录)
    -   pwd
        -   作用：显示当前所在的路径
    -   ls
        -   作用：显示指定目录的文件或子目录信息
        -   语法：`ls [选项] [目录名/文件名]`
        -   常用选项
            -   `-l`  列表显示文件的详细信息
            -   `-a`  显示隐藏文件（以 . 开头的是隐藏文件）
    -   cp
        -   作用：复制（一个目录下重名则覆盖）
        -   语法：
            -   `cp [被复制的文件] [目标路径]`
            -   `cp [被复制的文件夹] [目标路径] -r`
    -   mv
        -   作用：移动
        -   语法：
            -   `cp [被移动的文件/文件夹] [目标路径]`
    -   rm
        -   作用：删除
        -   语法：
            -   `rm [被删除的文件]`
            -   `rm [被删除的文件夹] -rf`
        -   常用选项
            -   `-r` 删除全部文件或文件夹
            -   `-i` 删除前的提示(y代表yes，n代表no)
            -   `-f` 强制删除
    -   mkdir
        -   作用：创建一个或者多个文件夹
        -   语法：`mkdir [选项] 文件夹1 文件夹2 ...`
        -   常用选项
            -   `-p` 逐层创建目录 (ex. `mkdir -p a/b/c/d`)
    -   rmdir
        -   作用：删除一个或多个空的文件夹(文件夹内容必须为空)
        -   语法：rmdir 文件夹
    -   touch
        -   作用：创建文件
            -   如果文件不存在则直接创建
            -   如果文件存在，则用系统时间更新它的修改时间
        -   语法：touch 文件名1 文件名2
    -   cat
        -   作用：在终端显示文件内容
        -   语法：cat 文件名
    -   clear / Ctrl + L
        -   作用：作用清屏
    -   sudo -i
        -   作用：长时间切换到超级用户模式
        -   Ctrl C 可停止当bai前正在执行的命令
        -   Ctrl D 退出root权限
    -   chmod 666 file_name
        -   将文件权限设置为： rw-rw-rw-

4.  软件管理

    -   升级软件包：sudo apt-get update
        -   sudo apt-get upgrade
    -   安装软件：sudo apt-get install
    -   卸载软件：sudo apt-get remove --purge

## ssh 服务

> ssh 是一种安全协议，主要用于给远程登陆会话数据进行加密，保证数据传输的安全，在数据传输方面有很多应用。

### Linux 下的 ssh 服务

> 在 Linux 下 SSH 服务端是一个在后台运行的程序，响应来自客户端的连接请求，SSH服务端的远程名为 sshd ，负责实时监听远程 SSH 客户端的远程连接请求，并进行处理。

-   安装： `sudo apt-get install openssh-server`
-   查看： ssh 服务状态： ps -e|grep ssh
-   启动和关闭：`sudo service ssh start/restart/stop`

### ssh 命令

#### ssh 登录远程主机

```bash
    ssh [-p port] username@ip
    # 退出：exit / Ctrl + D
```

#### scp命令

> scp 命令可以用来通过安全，加密的连接在机器间传输文件

```bash
    # 把本地文件传输给远程系统
    scp localfile username@tohostname:/newfilename
    # 把远程文件传输给本地系统
    scp username@tohostname:/remotefile /localfile
```

### ssh 密钥

> 每次登录远程主机都需要输入密码不是很方便，如果要加速这一步骤，可以利用密钥对进行连接，主要思路：生成一对公钥私钥，私钥在local主机上，公钥在远程服务器上，每次建立 ssh 连接自动检查密钥对是否匹配。

#### 生产 ssh 密钥步骤

1.  生产密钥对命令：`ssh-keygen`一路回车，执行以后会在主目录下生产一个 .ssh 文件夹，其中包含私钥文件id_rsa和公钥文件 id_rsa.pub

2.  在服务器主机上 将 id_rsa.pub 文件的内容附加 ~/.ssh/authorized_keys 文件中，并修改其权限。


## PIP

> 管理 Python 的标准第三方库中的第三方软件包

-   安装：`sudo apt-get install python3-pip`

-   常用命令：

    -   安装软件：pip3 install [package]

    -   查看当前 python 软件包：pip3 list

    -   搜素某个名字的python包：pip3 search [name]

    -   查看软件包信息： pip3 show [package]

    -   升级软件包：pip3 install --upgrade [package]

    -   卸载软件包：sudo pip3 uninstall [package]

    -   导出软件包环境： pip3 freeze > requirements.txt

    -   根据文档自动安装： pip3 install -r requirements.txt
