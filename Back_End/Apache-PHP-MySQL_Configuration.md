# APACHE

## Apache 安装与配置

```shell

##安装Apache

##找到配置文件    C:\Develop\apache\conf    ==>    httpd.conf

##根目录修改

$ Define SRVROOT "C:/Develop/apache"
$ ServerRoot "${SRVROOT}"

##根目录修改cmd安装

$cd C:\Develop\apache\bin
$httpd -k install -n "Apache"
$httpd -t    ==>  Syntax OK

```

## Apache 虚拟主机配置

```shell

##1.Apache配置文件中解锁虚拟主机配置

	##1.1 找到    ==>    # Virtual hosts

	##1.2 解锁    ==>    Include conf/extra/httpd-vhosts.conf

##1.Apache配置文件中解锁虚拟主机配置

	##2.1 找到    ==>	   C:\Develop\apache\conf\extra    ==>    httpd-vhosts.conf

	##2.2 配置虚机主机

		<VirtualHost *:80>
		    DocumentRoot "E:/host-demo/answer-zf"
		    <Directory "E:/host-demo/answer-zf">
		        Options Indexes FollowSymLinks
		        AllowOverride None
		        Require all granted
		    </Directory>
		    ServerName answer01.com
		    ErrorLog "logs/answer01.com-error.log"
		    CustomLog "logs/answer01.com-access.log" common
		</VirtualHost>

		<VirtualHost *:80>
		    DocumentRoot "E:/host-demo/virtualHost02"
		    <Directory "E:/host-demo/virtualHost02">
		        Options Indexes FollowSymLinks
		        AllowOverride None
		        Require all granted
		    </Directory>
		    ServerName answer02.com
		    ErrorLog "logs/answer02.com-error.log"
		    CustomLog "logs/answer02.com-access.log" common
		</VirtualHost>

		<VirtualHost *:80>
		    DocumentRoot "E:/host-demo/virtualHost03"
		    <Directory "E:/host-demo/virtualHost03">
		        Options Indexes FollowSymLinks
		        AllowOverride None
		        Require all granted
		    </Directory>
		    ServerName answer03.com
		    ErrorLog "logs/answer03.com-error.log"
		    CustomLog "logs/answer03.com-access.log" common
		</VirtualHost>

```

## 开启 Apache 的 gzip 压缩

要让 apache 支持 gzip 功能，要用到 deflate_Module 和 headers_Module。打开 apache 的配置文件 httpd.conf，大约在 105 行左右，找到以下两行内容：（这两行不是连续在一起的）

```
#LoadModule deflate_module modules/mod_deflate.so
#LoadModule headers_module modules/mod_headers.so
```

然后将其前面的“#”注释删掉，表示开启 gzip 压缩功能。开启以后还需要进行相关配置。在 httpd.conf 文件的最后添加以下内容即可：

```
<IfModule deflate_module>
    #必须的，就像一个开关一样，告诉apache对传输到浏览器的内容进行压缩
    SetOutputFilter DEFLATE
    DeflateCompressionLevel 9
</IfModule>
```

最少需要加上以上内容，才可以生 gzip 功能生效。由于没有做其它的额外配置，所以其它相关的配置均使用 Apache 的默认设置。这里说一下参数“DeflateCompressionLevel”，它表示压缩级别，值从 1 到 9，值越大表示压缩的越厉害。

## 使用 ngrok 将本机映射为一个外网的 Web 服务器

注意：由于默认使用的美国的服务器进行中间转接，所以访问速度炒鸡慢，访问时可启用 FQ 软件，提高网页打开速度！

# PHP 配置

## Apache ==> PHP 配置

```shell

##Apache 配置文件加载 php模块

	##找到    ==>    LoadModule

	##添加    ==>    LoadModule php7_module C:/Develop/php/php7apache2_4.dll


##Apache 配置文件添加 php MINE Type

	##找到    ==>    AddType

	##添加    ==>    AddType application/x-httpd-php .php

```

## php ==> 配置扩展

```shell

##1.在 PHP 根目录下找到

	##1.1 PHP官方配置模板

		php.ini-development

	##1.2 复制一份，并更改名称

		php.ini

	##1.3 查找extension_dir

		##1.3.1 加上扩展目录配置

			extension_dir = "C:/Develop/php/ext"

		##1.3.2 再解开相应扩展的注释

 			extension = xxx.dll

 			extension = php_mbstring.dll

	##1.4 Apache配置文件加载php.ini

		 	PHPIniDir C:/Develop/php

			LoadModule php7_module C:/Develop/php/php7apache2_4.dll

	##1.5 重启Apache

```

## PHP ==> REPL 环境配置

```shell

$ cd C:\Develop\php
$ php -a                 ## 输出：Interactive shell

```

## PHP ==> 时区配置

```shell

## php.ini文件内：

date.timezone = PRC


## 更推荐代码设置

date_default_timezone_set('PRC');

```

## PHP ==> 警告配置

```shell

## php.ini文件内：


	## 找到 display_errors

 			=  on  			出现notice级别的警告（开发阶段需要打开）

			=  off  		隐藏notice级别的警告（生产阶段需要关闭）

```

## PHP ==> 上传文件限制配置

```shell

## php.ini文件内：


	## 上传文件大小限制

	   upload_max_filesize = 2M

	## 请求报文大小限制

	   post_max_size = 8M

```

## PHP ==> SESSION 配置

```ini

;一般情况下回修改session.name  避免麻烦
session.name = PHPSESSID

```

# MySQL 配置

## MySQL 安装与配置

```shell

## 初始化

$ cd C:\Develop\mysql\bin

$ mysqld --initialize --user=mysql --console （ 运行框中有临时密码需要保存 ）

$ mysqld --install MySQL


## 运行

$ mysql -u root -p( 此处直接回车保护密码 )

$ Enter password:（ 初始化后给的临时密码 ）


## 修改初始密码

mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';
			(8.04以后版本'新的密码验证插件：caching_sha2_password')

mysql> set password for root@localhost = password('123');(8.04以前版本)
mysql> set password=password('[修改的密码]');(8.04以前版本)


## 打印所有数据库

mysql> show databases;

```

## MYSQL ==> 设置默认字符集

```shell

## 新建  my.ini

	## 添加

		[mysqld]
		# 默认字符集
		character-set-server=utf8

```

# PHP 连接 MYSQL8.0+版本 解决报错

```shell

## 进入MySQL中REPL环境

mysql> use mysql;   	## 输出：Database changed

mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';
					## 输出：Query OK, 0 rows affected (0.14 sec)

mysql> FLUSH PRIVILEGES;

					## 输出：Query OK, 0 rows affected (0.02 sec)
```

# NODE 安装

```shell

## cmd输入

$ npm init -f


## 找到：C:\Users\answer_zf

package.json


## 添加：

"description": "这里是------description",
"repository": {
    "type": "",
    "url": ""
}

```
