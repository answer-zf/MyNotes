# 记录一些常见的电脑问题

## 微软账号登录不上

1. 找到：控制面板\网络和 Internet\网络和共享中心

2. 打开 Internet 选项

3. 点击 高级

4. 点击 重置 按钮，选上 删除个人设置，重置！

5. win + R :输入`services.msc`

6. 进入本地服务管理，查看以下服务是否正常启用了

   - `Microsoft Account Sign-in Assistant` 如果未启用，请将其类型设置为自动，然后点击"启动"

## ftp 启动问题

### 普遍情况解决方法

1. 找到：控制面板\网络和 Internet\网络和共享中心

2. 打开 Internet 选项

   - 找到：`使用被动FTP(用于防火墙和DSL调制解调器的兼容)`
   - 将默认的勾选取消

### 特殊情况解决方法

1. 修改  设置

   - 找到：控制面板\网络和 Internet\网络和共享中心

   - 打开 Internet 选项
      - 找到：`启用 FTP 文件夹视图 （在 InternetExplorer 之外）`
      - 勾选改项

2. 修改注册表

   - 将以下内容保存成任意文件名的以.reg 作为后缀的文件，并双击导入即可

    ```reg
      Windows Registry Editor Version 5.00
      [HKEY_CLASSES_ROOT\ftp]
      @="URL:File Transfer Protocol"
      "AppUserModelID"="Microsoft.InternetExplorer.Default"
      "EditFlags"=dword:00000002
      "FriendlyTypeName"="@C:\\Windows\\System32\\ieframe.dll,-905"
      "ShellFolder"="{63da6ec0-2e98-11cf-8d82-444553540000}"
      "Source Filter"="{E436EBB6-524F-11CE-9F53-0020AF0BA770}"
      "URL Protocol"=""
      [HKEY_CLASSES_ROOT\ftp\DefaultIcon]@=hex(2):25,00,53,00,79,00,73,00,74,00,65,00,6d,00,52,00,6f,00,6f,00,74,00,25,\    00,5c,00,73,00,79,00,73,00,74,00,65,00,6d,00,33,00,32,00,5c,00,75,00,72,00,\    6c,00,2e,00,64,00,6c,00,6c,00,2c,00,30,00,00,00
      [HKEY_CLASSES_ROOT\ftp\Extensions]  
      ".IVF"="{C69E8F40-D5C8-11D0-A520-145405C10000}"
      [HKEY_CLASSES_ROOT\ftp\shell]
      @="open"
      [HKEY_CLASSES_ROOT\ftp\shell\open]
      [HKEY_CLASSES_ROOT\ftp\shell\open\command]
      @="\"C:\\Program Files\\Internet Explorer\\IEXPLORE.EXE\" %1"
      [HKEY_CLASSES_ROOT\ftp\shell\open\ddeexec]
      @="\"%1\",,-1,0,,,,"
      "NoActivateHandler"=""
      [HKEY_CLASSES_ROOT\ftp\shell\open\ddeexec\Application]
      @="IExplore"
      [HKEY_CLASSES_ROOT\ftp\shell\open\ddeexec\ifExec]
      @="*"
      [HKEY_CLASSES_ROOT\ftp\shell\open\ddeexec\Topic]
      @="WWW_OpenURL"
    ```
