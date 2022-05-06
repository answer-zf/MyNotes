# Django 框架

## 简介：

![Python-Django](http://localhost:9710/blog/Python/Python-Django.png)

-   组件：

    1.  路由
    2.  URL解析
    3.  原生 HTML 模板系统
    4.  数据库连接 和 ORM 数据库管理
    5.  用户管理认证系统
    6.  ORM模型系统
    7.  电子邮件发送系统
    8.  CSRF跨站点请求伪造的保护
    9.  表单验证
    10. 数据库后台管理系统
    11. 自带强大的后台管理功能

-   DJango的用途

    1.  网站后端开发
    2.  微信公众号后台开发
    3.  微信小程序后台开发
    4.  基于HTTP/HTTPS 协议的后台服务器开发
        -   在线语音图像识别服务器
        -   在线第三方身份验证服务器等

## 安装：

1.  在线安装：`$ sudo pip3 install django==1.11.8`

2.  离线安装：

    -   下载安装包： `$ Django-1.11.8.tar.gz`
    -   `$ tar -xvf Django-1.11.8.tar.gz`
    -   `$ cd Django-1.11.8`
    -   `$ sudo python3 setup.py install`

3.  使用 wheel 安装

    -   下载:`pip3 download -d 路径`
    -   安装：`pip3 install Django-1.11.8.whl`

4.  卸载：`pip3 uninstall Django`

5.  版本查询：`pip3 freeze | grep 'Django'`

## Django 框架简介

1.  创建项目:`$ django-admin startproject project_name`

2.  运行:

    -   `cd project_name/`
    -   `python3 manage.py runserver`
        -   `python3 manage.py runserver 5000`
            -   指定设备有5000端口
        -   `python3 manage.py runserver 192.168.40.129:8000`
            -   指定IP+端口
            -   指定IP需要修改settings.py
            -   ALLOWED_HOSTS = \[] #空列表只允许127.0.0.1访问
            -   ALLOWED_HOSTS = ['*'] 允许任意访问 仅限调试使用

3.  目录解析

    -   manage.py
        -   此文件是项目管理的主程序在开发阶段用于管理整个项目的开发运行的调式
        -   manage.py 包含项目管理的子命令
        -   `python3 manage.py runserver` 启动服务
        -   `python3 manage.py startapp` 创建应用
        -   `python3 manage.py migrate` 数据库迁移
    -   project_name(项目包文件夹[默认与项目名称一致])

        -   `__init__.py`

            -   项目初始化文件,服务启动时自动运行

        -   `wsgi.py`

            -   WEB服务网关接口的配置文件,仅部署项目时使用

        -   `urls.py`

            -   项目的基础路由配置文件,所有的动态路径必须先走该文件进行匹配

        -   `settings.py`

            -   Django的配置文件,此配置文件中的一些全局变量将为 Django框架的运行传递一些参数
            -   settings. py配置文件,启动服务时自动调用
            -   此配置文件中也可以定义一些自定义的变量用于作用全局作用域的数据传递

4.  文件详解

    -   settings.py 文件介绍

        1.  BASE_DIR
            -   用于绑定=当前项目的绝对路径(动态计算出来的),所有文件都可以依懒此路径
        2.  DEBUG用于配置 Django项目的启用模式
            1.  True表示开发环境中使用调试模式 (用于开发中)
            2.  False表示当前项目运行在生产环境中(不启用调试)
        3.  ALLOWED_HOSTS
            -   设置允许访问到本项目的网络地址列表
            -   取值：
                -   `ALLOWED_HOSTS = []` 如果为空列表，只允许127.0.0.1访问本项目
                -   `ALLOWED_HOSTS = ['*']`表示任何网络地址都能访问到当前项目 仅限调试使用
        4.  INSTALLED_APPS
            -   指定当前项目中安装的应用列表
        5.  MIDDLEWARE
            -   用于注册中间件
        6.  ROOT_URLCONF
            -   用于配置根级 url 配置
        7.  TEMPLATES
            -   用于指定模板的配置信息
        8.  DATABASES
            -   用于指定数据库的配置信息
        9.  LANGUAGE_CODE
            -   用于指定语言配置
            -   取值：
                -   中文：`LANGUAGE_CODE = "zh-Hans"`
        10. TIME_ZONE
            -   用于指定当前服务器端时区
            -   取值：
                -   中国时区：`TIME_ZONE = "Asia/Shanghai"`


        -   缺省配置
            -   模块
                -   `import django.conf.global_settings`
                -   Linux 下文件位置
                    -   `/usr/lib/python3/dist-packages/django/conf/global_settings.py`

## 路由

> url 即统一资源定位符 Uniform Resource Locator

-   作用
    -   对互联网上得到的资源的位置和访问方法的一种简洁的表示,是互联网上标准资源的地址。互联网上的每个文件都 有一个唯一的URL,它包含的信息指出文件的位置以及浏览器应该怎么处理它。

> 路由：不看域名部分

### 在Django 中的 URL 路由配置

1.  settings.py 中 的 ROOT_URLCONF

    -   通过 ROOT_URLCONF 指定顶级url配置
        -   默认存在于主文件夹内，主路由配置文件

2.  urlpatterns 是一个 url() 实例的列表

    -   作用：该文件会包含 urlpatterns的列表用于表示路由-视图映射,通过url()表示具体映射

3.  url() 函数

    -   用户描述路由与视图函数的对应关系
    -   模块
        -   `from django.conf.urls import url`
    -   语法：
        -   `url(regexp,views,kwargs=None,name=None)`
            1.  regexp:字符串类型,匹配的请求路径,允许是 正则表达式
                -   自上而下的匹配，遇到满足条件直接执行视图函数，后续匹配不再执行。
            2.  views:指定路径所对应的视图处理函数的名称
            3.  kwargs:向视图中传递的参数
            4.  name:为地址起别名,反向解析时使用
        -   注：
            -   每个正则表达式前面的 r 表示 \\ 不转义的原始字符串
            -   当 urlpatterns 内有多个url对象时,按自上而下的顺序进行配置,一但有路由与url配置成功,则后面的所有url被忽略

4.  视图 view

    -   用于接收请求,处理请求并做出响应
    -   视图处理的函数的语法格式

        ```py
            def xxx(request[,其他参数...]):
                return 响应对象
        ```

    -   url 和带有参数的视图函数

        -   在视图函数内,可以用正规表达式分组()提取参数后传送给视图函数
        -   一个分组表示一个参数,多个参数需要使用多个分组,并且使用个/隔开

    -   url 正则表达式命名分组(?Pxx)和带有参数的视图函数
        -   在url 的正则表达式中可以使用命名分组(捕获分组)
        -   正则表达式的名称必须在view中以关键字传参方式传入,因此视图函数能接收此参数
        -   每个捕获的参数都作为一个普通的 python字符串传递给视图
        -   urlpatterns中的每个正则表达式在第一次访问它们时被编译, 这使得系统相当快

    ```py
        ################### urls.py ###################
        from . import views

        urlpatterns = [
            url(r'^admin/', admin.site.urls),
            url(r'^$', views.homepage),
            url(r'^year/(\d{4})$', views.pageyear),
            url(r'^birthday/(\d{4})/(\d{1,2})/(\d{1,2})$', views.birthday),
            url(r'^person/(?P<name>\w+)/(?P<age>\d+)', views.person),
        ]

        ################### views.py ###################
        from django.http import HttpResponse

        def homepage(request):
            return HttpResponse("this .is .homepage...")

        def pageyear(request, y):
            return HttpResponse("this.is.yearpage.%s" % y)

        def birthday(request, y, m, d):
            return HttpResponse('birthday is %s-%s-%s' % (y, m, d))

        def person(request, name, age):
            return HttpResponse("this is %s, %s" % (name, age))
    ```

## 请求和响应

### HTTP请求

-   根据HTTP标准,HTTP请求可以使用多种请求方法。
-   HTTP1.0 定义了三种请求方法: GET , POST 和 HEAD 方法(最常用)
-   HTTP1.1新增了五种请求方法: OPTIONS,PUT, DELETE, TRACE 和 CONNECT方法。
-   HTTP1.1请求详述:

| 序号  |  方法   | 描述                                                                                                                                |
| :---: | :-----: | :---------------------------------------------------------------------------------------------------------------------------------- |
|   1   |   GET   | 请求指定的页面信息,并返回实体主体                                                                                                   |
|   2   |  HEAD   | 类似于get请求,只不过返回的响应中没有具体的内容,用于获取报头                                                                         |
|   3   |  POST   | 向指定资源提交数据进行处理请求(例如提交表单或者上传文件)。数据被包含在请求体中。POST请求可能会导致新的资源的建立和/或巳有资源的修改 |
|   4   |   PUT   | 从客户端向服务器传送的数据取代指定的文档的内容。                                                                                    |
|   5   | DELETE  | 请求服务器删除指定的页面。                                                                                                          |
|   6   | CONNECT | HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器。                                                                            |
|   7   | OPTIONS | 允许客户端查看服务器的性能。                                                                                                        |
|   8   |  TRACE  | 回显服务器收到的请求,主要用于测试或诊断。                                                                                           |

-   HttpRequest 对象

    -   视图函数的第一个参数是 HttpRequest 对象
    -   服务器接收到http协议的请求后,会根据请求数据报文创建 HttpRequest 对象
    -   HttpRequest 属性
        -   path:字符串,表示请求的路由信息
        -   ✦ method:字符串,表示HTTP请求方法,常用值:'GET'、'POST'
        -   encoding:字符串,表示提交的数据的编码方式
            -   如果为 None 则表示使用浏览器的默认设置,一般为'utf-8'
            -   这个属性是可写的,可以通过修改它来修改访问表单数据使用的编码,接下来对属性的任何访问将使用新的 encoding值
        -   GET:QueryDict 查询字典的对象,包含 get 请求方式的所有数据
        -   POST: QueryDict 查询字典的对象,包含 post 请求方式的所有数据
        -   FILES:类似于字典的对象,包含所有的上传文件
        -   COOKIES: Python字典,包含所有的 cookie,键和值都为字符串
        -   session:似于字典的对象,表示当前的会话
        -   body:字符串,请求体的内容(POST或PUT)
        -   environ:字符串,客户端运行的环境变量信息
        -   scheme:请求协议('httpt'/'https')
        -   path_info:URL字符串
        -   request.get_full_path():请求的完整路径
        -   request.get_host():请求的主机
        -   request.META:请求中的元数据（消息头）
            -   request.META['REMOTE_ADDR']&#x3A;客户端IP地址
            -   request.META['HTTP_REFERER']&#x3A;请求源地址

### HTTP响应

-   当浏览者访问一个网页时,浏览者的浏览器会向网页所在服务器发出请求。当浏览器接收并显示网页前,此网页所在的服多器会返回一个包含HTTP状态码的信息头(server header)用以响应浏览器的请求.

-   HTTP状态码的英文为 HTTP Status Code

-   下面是常见的 HTTP 状态码

    -   200: 请求成功
    -   301: 资源(网页等)被永久转移到其它URL
    -   404: 请求的资源(网页等)不存在
    -   500: 内部服务器错误

-   HTTP 状态码分类

    -   HTTP 状态码由三个十进制数字组成,第一个十进制数字定义了状态码的类型,后两个数字具有分类的作用。HTTP状态码共分为5种类型

| 分类  | 描述                                          |
| :---: | --------------------------------------------- |
| 1\*\* | 信息,服务器收到请求,需要请求者继续执行操作    |
| 2\*\* | 成功,操作被成功接收井处理                     |
| 3\*\* | 重定向,需要进一步的操作以完成请求             |
| 4\*\* | 客户端错误,请求包含语法错误或无法完成请求     |
| 5\*\* | 服务器错误,服务器在处理请求的过程中发生了错误 |

-   Django中的响应对象 Httpresponse

    -   构造函数格式:

        -   `HttpResponse(content=响应体,content_type=响应数据类型,status=状态码)`

    -   作用：

        -   向客户端浏览器返回响应,同时携带响应体内容

    -   参数：

        -   content：表示返回的内容
        -   status_code:返回的HTTP响应状态码。
        -   content\_ type:指定返回数据的的 MIME 类型(默认为"text/html")。浏览器会根据这个属性,来显示数据。如果是text/html,那么就会解析这个字符串,如果text/ plain,那么就会显示一个纯文本。

            -   常用的 Content-Type如下

                -   text/html (默认的，html文件)
                -   text/plain (纯文本)
                -   text/css  (css文件)
                -   text/javascript (js 文件)
                -   multipart/from-data (文件提交)
                -   application/json (json 传输)
                -   application/xml (xml 文件)

            -   关键字 MIME(Multipurpose Internet Mail Extensions)是指多用途互联网邮件扩展类型

-   其他 Httpresponse 响应对象

    -   HttpresponseRedirect   重定向，状态码 301
    -   HttpresponseNotModified   未修改，状态码 304
    -   HttpresponseBadRequest   错误请求，状态码 400
    -   HttpresponseNotFound   没有对应的资源，状态码 404
    -   HttpresponseForbidden    请求被禁止，状态码 403
    -   HttpresponseServerError   服务器错误，状态码 500

### GET方式传参

>  GET请求方式中可以通过查询字符串( Query String)将数据传递给服务器

#### GET方式传参参数获取(查询字符串 Query String)

-   客户端传递参数给服务器端

    -   URL 格式：`网址?参数名1=值1&参数名2=值2...`

-   服务端接收参数

        1.  判断 request.method 的值判断请求方式是否是 get 请求

            ```py
            if request.method == 'GET':
                去往指定的模板进行显示
            else：
                接收其他请求提交的数据
            ```

        2.  获取客户端请求 GET 请求提交的数据

            1.  语法

                -   `request.GET['参数名']`
                -   `request.GET.get('参数名','默认值')`
                -   `request.GET.getlist('参数名')`

            2.  能产生 get 请求方式的场合

                1.  地址栏手动输入
                2.  `<a href='地址?参数=值&参数=值'>`
                3.  表单的method为get

                    ```html
                        <form method="get" action="/user/login">
                            姓名：<input type="text" name="uname">
                        </form>
                    ```

            ```py
                urlpatterns = [
                    url(r'^get_birthday$', views.get_birthday),
                ]

                def get_birthday(request):
                    if request.method == 'GET':
                        year = request.GET.get('year', '0000')
                        month = request.GET.get('month', '00')
                        day = request.GET.get('day', '00')
                        return HttpResponse("birthday is %s-%s-%s" % (year, month, day))
            ```

        3.  用 `request.GET` 获取的是类字典，即查询字典。

            -   `{"name": ['zf']}`
            -   `request.GET['name'] / request.GET.get('name') -> 'zf'`

        4. 一键多值时使用。eg. 复选框提交使用，用get提交其他选项会被最后一个值覆盖，只显示最后一个值

### POST方式传参

-   客户端通过表单等POST 请求将数据传递给服务器端

    ```xml
        <form method="post" action="/user/login">
            姓名：<input type="text" name="uname">
        </form>
    ```

-   服务器端接收参数

    -   通过request.method 来判断是否为 POST 请求，如：

        ```py
        if request.method == 'POST':
            处理POST 请求的数据并响应
        else：
            处理其他请求提交的数据并响应
        ```

    -   使用 POST 方式接收客户端数据

        1.  语法

            -   `request.POST['参数名']`
            -   `request.POST.get('参数名','默认值')`
            -   `request.POST.getlist('参数名')`

    -   取消 csrf 验证，否则 Django 将会拒绝客户端发来的请求
        -   删除 settings.py 中 MIDDLEWARE 中的 CsrfViewMiddleware 的中间件

    ```py
        def search(request):
            html = '''
            <html lang="en">
            <head>
                <title>Document</title>
            </head>
            <body>
                <form action="/search" method="POST">
                    <input type="text" name="username">
                    <input type="submit">
                </form>
            </body>
            </html>
            '''
            if request.method == 'GET':
                return HttpResponse(html)
            elif request.method == 'POST':
                tbl = request.POST.get('username', 'zf')
                return HttpResponse('tbl : ' + tbl)
            else:
                return HttpResponse("other ....")
    ```

### Django 框架模式

-   MVC设计模式

    -   MVC代表 Model-View-Controller(模型-视图-控制器)模式
    -   作用:降低模块间的耦合度(解耦)
    -   MVC
        -   M 模型层(Model),主要用于对数据库层的封装
        -   V 视图层(View),用于向用户展示结果
        -   C 控制 (Controller),用于处理请求、获取数据、返回结果(重要)

-   MTV模式

    -   MTV代表 Model-Template-View(模型-模板-视图)模式。这个模式用于应用程序的分层开发
    -   作用：降低模块间的耦合度(解耦)
    -   MTV
        -   M 模型层(Model),负责与数据库交互
        -   T 模板层(Templates),负责呈现网页内容
        -   V 视图层(View),负责接收请求 获取数据 返回结果（核心）

### 模板 Templates

-   什么是模板

    1.  模板是html页面,可以根据视图中传递的数据填充值相应的√页面元素。
    2.  模板层提供了一个对设计者友好的语法用于渲染向用户呈现的信息。

-   模板的配置

    -   创建模板文件夹 &lt;项目名>/templates
    -   在 settings.py 中有一个 TEMPLATES 变量
        1.  BACKEND: 指定模板的引擎
        2.  DIRS：指定保存模板的目录
        3.  APP_DIRS：是否要在应用中搜素模板版本
        4.  OPTIONS:有关模板的选项

-   默认的模板文件夹 templates

-   默认settings.py 文件，设置 TEMPLATES 的 DIRS 值为

    -   `'DIRS':[os.path.join(BASE_DIR,'templates')],`

-   模板的加载方式

    1.  通过 loader 获取模板，通过 HttpResponse 进行响应

        ```py
            # views.py
            from django.http import HttpResponse
            from django.template import loader

            def page1_template(request):
                t = loader.get_template('page1.html')  # 加载模板
                html = t.render()  # 渲染成字符串
                return HttpResponse(html)
        ```

    2.  使用 render() 直接加载并响应模板

        ```py
            # views.py
            from django.shortcuts import render

            def page1_template(request):
                return render(request, 'page1.html')
        ```

-   模板的传参

    -   模板传参是指把数据形成字典,传参给模板,由模板渲染来填充数据
    -   在模板中调用函数不需要加（）
        -   [函数传参](https://www.cnblogs.com/Python666/p/7225808.html)

    1.  使用 loader 加载模板

        ```py
            t = loader.get_template('page1.html')
            html = t.render(字典数据)
            return HttpResponse(html)
        ```

    2.  使用 render 加载模板

        ```py
            return render(request,'xx.html',字典数据)
        ```

#### Django模板语言:( The Django template language)

-   模板的变量

    1.  在模板中使用变量语法：`{{ 变量名 }}`

        -   后端中必须将变量封裝到字典中才允许传递到模板上dic={'变量1':'值1','变量2':'值2',}

-   模板的标签

    1.  作用 将一些服务器端的功能嵌入到模板中
    2.  标签语法

    ```html
        {% 标签 %}
        ...
        {% 结束标签 %}
    ```

    3.  if标签

        ```html
            {% if 条件表达式 %}
            ...
            {% elif 条件表达式 %}
            ...
            {% else %}
            ...
            {% endif %}
        ```

        -   if 条件表达式 里可以使用的运算符

            -   `== != < > <= >= in not in is is not not and or`

        -   在 if 标记中使用实际括号是无效的语法，如果需要它们指定优先级，则应使用嵌套的 if 标记。

    4.  for 标签

        1.  语法

        ```html
            {% for 变量 in 可迭代对象 %}
            ...
            {% endfor %}

            # eg.
            <ul>
                {% for i in fav %}
                    <li>{{ i }}</li>
                {% endfor %}
            </ul>
        ```

        2.  内置变量 forloop

|        变量         | 描述                                 |
| :-----------------: | ------------------------------------ |
|   forloop.counter   | 循环的当前迭代(从1开始索引)          |
|  forloop.counter0   | 循环的当前迭代(从0开始索引)          |
| forloop.revcounter  | 循环结束的迭代次数(从1开始索引)      |
| forloop.revcounter0 | 循环结束的迭代次数(从0开始索引)      |
|    forloop.first    | 如果这是第一次通过循环,则为真        |
|    forloop.last     | 如果这是最后一次循环,则为真          |
| forloop.parentloop  | 对于嵌套循环，这是围绕当前循环的循环 |

        2.  for ... empty 标签

            ```html
            /* 可迭代对象没用任何数据时 */
            {% for 变量 in 可迭代对象 %}

            {% empty %}

            {% endfor %}
            ```

    4.  cycle 标签

        -   循环从 cycle 列表后的参数中进行取值，每次调用进行一次更换
        -   这个标签经常用于循环中，eg. 处理表格的隔行变色
        -   语法：

            ```html
                {% for o in some_list %}
                    <tr class="{% cycle 'row1' 'row2' %}">
                        ...
                    </tr>
                {% endfor %}

                /* eg. */
                {% for i in fav %}
                    <tr class="{% cycle 'row1' 'row2' %}">
                        <td>{{ forloop.counter }}</td>
                        <td>{{ i }}</td>
                    </tr>
                {% endfor %}
            ```

    4.  注释 和 comment 标签

        -   单行注释：`{# ...注释 #}` 该范围内的文字信息会被模板的渲染系统忽略
        -   多行注释：`{% comment %} ...注释 {% endcomment %}` 之间的内容会被忽略
            -   comment 标签不能嵌套使用
        -   模板级的注释，html 源码不显示

#### 过滤器

1.  作用：

    -   在变量输出前对变量的值进行处理
    -   您可以通过使用过滤器来改变变量的显示。

2.  语法:`{{ 变量1 | 过滤器1:参数1 }}`

    -   eg.: `{{ string | truncatechars:9 | upper }}`

3.  常用过滤器

| 过滤器          | 备注                                                                                       |
| --------------- | ------------------------------------------------------------------------------------------ |
| default         | 如果 value 的计算结果为 False,则使用给定的默认值。否则使用该 value                         |
| default_if_none | 如果(且仅当) value 为 None ,则使用给定的默认值。否则,使用该 value                          |
| floatformat     | 当不使用参数时,将浮点数舍入到小数点后一位,但前提是要显示小数部分。                         |
| truncatechars   | 如果字符串字符多于指定的字符数量,那么会被截断。截断的字符串将以可翻译的省略号序列("…")结尾 |
| truncatewords   | 在一定数量的字后截断字符串                                                                 |
| lower           | 将字符串转换为全部小写                                                                     |
| upper           | 将字符串转换为大写形式                                                                     |

#### escape 转译

-   escape 转义字符串的HTML。具体来说,它使这些替换:

-   `<`转换为`&lt;`
-   `>`转换为`&gt;`
-   `'`转换为`&#39;`
-   `"`转换为`&quot;`
-   `&`转换为`&amp;`

```html
    {% autoescape on/off %} # 启动 / 关闭 转译
        {{ a }}
    {% endautoescape %}
```

#### 模板的继承

-   模板继承可以使父模板的內容重用，子模板直接继承父模板的全部内容并可以覆盖父模板中相应的块
-   定义父模板中的块 blcok 标签

    -   标识出哪些在子模块中是允许被修改的
    -   block 标签：在父模板中定义，可以在子模板中覆盖

    ```html
        {% block block_name %}
        定义模板块,此模板块可以被子模板重新定义的同名块覆盖
        {% endblock block_name %}
    ```

-   重写的覆盖规则

    -   不重写,将按照父模板的效果显示
    -   重写,则按照重写效果显示

-   注意: 模板继承时服务器端的动态内容无法继承

-   在 child_html 中继承父模板的全部内容

    ```html
    {% extends '父模板的名称' %}

    {% block block_name %}
    这是child_html的内容
    {% endblock block_name %}
    ```

#### url 反向解析

-   url()的name 关键字参数

    -   作用
        -   根据 url 列表中的name 关键字传参给 url 唯一确定一个名字，在模板中可以通过这个名字方向推出此url 信息。
        -   服务端变化与客户端分离降低耦合度。服务端的url 正则匹配变化，客户端无需变化。
        -   通俗的说正则匹配的别名
    -   语法
        -   url(regexp,views,kwargs=None,name='别名')
    -   通过别名实现地址的反向解析，在模板中：
        -   `<a href="{% url '别名' %}">页面</a>`
        -   `<a href="{% url '别名' 参数1 %}">页面</a>`

## 静态文件

1.  什么是静态文件

    -   不能与服务器端做动态交互的文件都是静态文件
    -   如图片、 CSS、JS、音频、视频、html文件(部分)

2.  静态文件配置

    -   在 settings.py 中配置两项内容：

        1.  配置静态文件的访问路径

            -   STATIC_URL='/static/' (静态文件的路由的起始位置)
            -   说明：

                -   指定访问静态文件时是需要 通过 /static/xxx 或 127.0.0.1:8000/static/xxx
                -   xxx 表示具体的静态资源位置

            -   配置静态文件的存储路径

                -   静态文件的服务其端的保存位置(定义静态文件的查找路径的实际位置)
                -   STATICFILES_DIRS=(os.path.join(BASE_DIR,'static'),)

        2.  通过 `{% static %}` 标签访问静态文件，`{% static %}`表示的就是静态文件访问路径

            1.  加载 static {% load static %}
            2.  使用 静态资源时 语法 `{% static '静态资源路径' %}`
            3.  作用：动态加载

            ```xml
                {% load static %}
                <img src="{% static './images/pk.jpg' %}" alt="">

                <!-- html 渲染后源代码 -->
                <img src="/static/images/pk.jpg" alt="">
            ```

## Django 中的应用 app

-   什么是应用

    -   应用在 Django 项目中是一个独立的业务模块,可以包含自己的路由,视图...
    -   Django 中,主文件夹是不处理用户具体请求的，主文件夹的作用是做项目的初始化以及请求的分发(分布式请求处理)具体的请求是由应用来进行处理的

-   创建应用 app

    -   创建应用的指令

        -   `pyhton3 manage.py startapp 应用名称`
            -   应用名称：变量名命名规则

-   Django 应用的结构组成

         1.  `migrations` 文件夹
             -   保存数据迁移的中间文件
         2。  `__init__. py`
             -   应用子包的初始化文件
         3.  `admin.py`
             -   应用的后台管理配置文件
         4.  `apps.py`
             -   应用的属性配置文件
         5.  `models.py`
             -   与数据库相关的模型映射类文件
         6.  `tests.py`
             -   应用的单元测试文件
         7.  `models.py`
             -   定义视图处理函数的文件
         8.  `urls.py`
             -   自己创建 分支路由
         9.  `templates/`
             -   自己创建 应用模板

-   配置安装应用

    -   在 settings.py 中配置应用 -> 在 INSTALLED_APPS 添加应用名

        ```python
            INSTALLED_APPS = [
                ...
                'bookstore',
            ]
        ```

-   应用的分布式路由

    -   使用 include 函数让某个正则匹配后关联分支到某个app。
    -   步骤：
        -   主模块的 urlpatterns 中添加 `url(r'^music/', include('music.urls')),`
        -   music 模块的 urlpatterns 中添加 `url(r'^list$', views.list),`
        -   最后在 视图函数 添加 list 方法

## 数据库和模型

### Django 下使用 mysql 数据库

1.  安装 pymysql 包

    -   用作 python 和 MySQL 的接口
        -   `sudo pip3 install pymysql`
    -   安装 MySQL 客户端(非必须)
        -   `sudo pip3 install mysqlclient`

2.  创建和配置数据库

    1.  创建数据库

        -   创建 `create database 数据库名 default charset utf8 collate utf8_general_ci;`
            -   `collate utf8_general_ci` 英文不区分大小写

    2.  数据库的配置

        -   sqlite 数据库配置

            -   ENGINE

                -   指定数据库后端引擎
                    -   'django.db.backends.mysql'
                    -   'django.db.backends.sqlite3'
                    -   'django.db.backends.oracle'
                    -   'django.db.backends.postgresql'

        ```py
            # mysql -> 将 DATABASES 中的 default 做如下替换
            'default': {
                'ENGINE': 'django.db.backends.mysql',
                'NAME': 'mywebdb',  # 数据库名称
                'USER': 'root',
                'PASSWORD': '341309',
                'HOST': '127.0.0.1',
                'PORT': 3306,
            }
        ```

    3.  添加 mysql 支持

        -   修改项目主模块中 `__init__.py` 加入如下内容来提供 pymysql 引擎的支持

            ```py
            import pymysql

            pymysql.install_as_MySQLdb()
            ```

3.  数据库迁移

> 数据库的迁移是 Django 同步 你对模型所做更改（添加字段，删除模型等）到数据库的方式

1.  生成或更新迁移文件

    -   将每个应用下的 models.py 文件生成一个中间文件，并保存在 migrations 文件夹中
    -   `python manage.py makemigrations`

2.  执行迁移脚本程序

    -   执行迁移程序实现迁移。将每个应用下的 migrations 目录中的中间文件同步回数据库
    -   `python manage.py migrate`

    -   回滚
        -   `python manage.py migrate books 000x`
            -   books -> 应用app 名字

3.  查看迁移执行的SQL语句

    -   将 sqlmigrate, 显示迁移的sql 语句
    -   `python manage.py sqlmigrate`

### 模型

> 模型是提供数据信息的数据库接口模型是数据的唯一的、确定的信息源。它包含你所储存数据的必要字段和行为。

-   通常,毎个模型对应数据库中唯一的一张表。毎个模型的实例对应数据表中的一条记录

-   模型说明:
    -   每个模型都是一个 Python类,每个模型都是 django.db.models.Model 的子类。
    -   每一个模型属性都代表数据库中的一个表。
    -   通过所有这一切, Django 为你提供一个自动生成的数据库访问API

## Python数据库模型 ORM

1.  ORM框架

    -   ORM(Object Relationship Mapping) **对象关系映射**， 它允许你使用类和对象对数据库进行交互(使用类和对象和使用SQL一样且更方便各种操作)。

    -   三大特征
        1.  表到类的映射
        2.  数据类型的映射
        3.  关系映射

2.  模型示例

    -   添加 bookstore 的 app: `python manage.py startapp bookstore`
    -   添加模型类并注册app

        -   在 主模块的 settings.py 的 INSTALLED_APPS 中注册

            ```python
                INSTALLED_APPS = [
                    ...
                    'bookstore',
                ]
            ```

    -   添加数据表

        -   在 bookstore app 中的 models.py 创建模型类

            ```python
                from django.db import models

                class book(models.Model):
                    title = models.CharField("书名", max_length=50)
            ```

    -   重新生成迁移文件：`python manage.py makemigrations`
    -   更新数据库：`python manage.py migrate`

_ps. 报错处理步骤_

_1.  删除 migrations 文件夹中 所有的 `000?_XXXX.py`_
_2.  删除数据表，并重新创建_
_3.  重新生成 migrations 文件夹中 所有的 `000?_XXXX.py`，并更新数据库_

_mysql 8.+ 以上的报错_
_pip install cryptography_

3.  编写模型类

    -   模型类需继承自 `django.db.models.Model`

        1.  Models的语法规范

            ```python
                from django.db import models

                class ClassName(models.Model):
                    name = models.FIELD_TYPE(FIELD_OPTIONS)
            ```

        2.  CLASSNAME

            -   实例类名，表名组成的一部分，首字母大写
            -   默认表名组成规范：
                -   应用名称\_classname
            -   NAME
                -   属性名，映射回数据库就是字段名
            -   FIELD_TYPE
                -   字段类型，映射到表中的字段类型

        3.  FIELD_TYPE(字段类型)

            1.  BooleanField()

                -   数据库类型：tinyint(1)
                -   编程语言中：使用 True / False 来表示值
                -   在数据库中：使用 1 / 0 表示具体的值

            2.  CharField()

                -   数据库类型：varchar
                -   注意：
                    -   必须要指定 max_length 参数值

            3.  DateField()

                -   数据库类型：date
                -   作用：表示日期
                -   编程语言中：使用 字符串 来表示具体值
                -   参数：
                    -   DateField.auto_now: 每次保存对象时，自动设置该字段为当前时间（取值: True / False）
                    -   DateField.auto_now_add: 当对象第一次被创建时自动设置当前时间（取值: True / False）
                    -   DateField.default: 设置当前时间（取值： 字符串时间格式：'2019-6-1'）
                    -   以上三个参数只能多选一

            4.  DateTimeField()

                -   数据库类型：datetime(6)
                -   作用：表示日期和时间
                -   auto_now_add=True

            5.  DecimalField()

                -   数据库类型：decimal(x,y)
                -   编程语言中：使用 小数 表示该列的值
                -   在数据库中：使用 小数
                -   参数：
                    -   DecimalField.max_digits:总位数 / 长度，包括小数点后的位数。该值必须大于等于 decimal_places。
                    -   DecimalField.decimal_places: 小数点后的数字长度
                -   eg.
                    -   `price = models.DecimalField("定价", max_digits=7, decimal_places=2, null=True)`

            6.  FloatField()

                -   数据库类型：double
                -   编程语言中和数据库中都使用 小数 表示值

            7.  EmailField()

                -   数据库类型：varchar
                -   编程语言中和数据库中都使用 字符串 表示值
                -   包含邮箱的正则验证，正则匹配则存入

            8.  IntegerField()

                -   数据库类型：int
                -   编程语言中和数据库中都使用 整数 表示值

            9.  URLField()

                -   数据库类型：varchar(200)
                -   编程语言中和数据库中都使用 字符串 表示值

            10. ImageField()

                -   数据库类型：varchar(100)
                -   作用：在数据库中保存图片的路径
                -   编程语言中和数据库中都使用 字符串 表示值

                ```python
                    import PIL

                    image=models.ImageField(
                        upload_to="static/images"  # 指定 图片的上传路径在后台上传时会自动将文件保存在指定的目录下
                    )
                    # 从数据表中获取的 image 数据需要 序列化后（str(image_data)） 传入 json
                ```

            11. TextField():

                -   数据库类型：longtext
                -   作用：不定长的字符数据

        4.  FIELD_OPTIONS(字段选项)

            -   指定创建的列的额外信息
            -   允许出现多个字段选项，多个选项之间使用 ， 隔开

            1.  primary_key

                -   如果设置为True，表示该列为主键

            2.  null

                -   如果设置为True，表示该列值允许为空

            3.  default

                -   设置所在列的默认值

            4.  db_index

                -   如果设置为True，表示为该列 添加索引

            5.  unique

                -   如果设置为True，表示为该列 的值唯一

            6.  db_column

                -   指明列的名称，如果不指定的话则采用属性名作为列名。
                -   eg.`title = models.CharField("书名", max_length=50, db_column='bookname')`

_[模板字段 参考文档](https://yiyibooks.cn/xx/Django_1.11.6/ref/models/fields.html)_

### 数据库的操作 CRUD

-   CRUD 是指在做计算处理时的增加（Create）、读取查询（Read）、更新（Update）和删除（Delete）

**管理器对象**

-   每一个继承自 models.Model 的模型类，都会有一个 objects 对象被同样继承下来。这个对象即管理器对象。
-   数据库的增删改查可以通过模型的管理器实现
    ```python
    class Entry(models.Model):
        ...
    Entry.objects.create(...) 是管理器对象
    ```

**创建数据对象**

-   Django 使用一种直观的方式把数据库表中的数据表示出Python对象
-   创建数据中每一条记录就是创建一个数据对象

1.  Entry.objects.create(属性=值，属性=值)

    -   成功：返回创建好的实体对象（一条记录）
    -   失败：抛出异常

2.  创建Entry 实体对象，并调用save()进行保存

    -   obj = Entry(属性=值,属性=值)
    -   obj.属性=值
    -   obj.save()
    -   无返回值，保存成功后，obj会被重新赋值

    ```django
        from . import models
        # 1.
        try:
            models.Book.objects.create(title='C++'...)
            print(abook)
        except:
            print('error..)

        # 2.
        try:
            abook=models.Book(title='C++'...)
            abook.save()
            print(abook)
        except:
            print('error..)

        # 3.
        book = models.Book()
        book.title = 'C++'
        book.save()
    ```

**Django Shell 的使用**

-   在Django提供了一个交互式的操作项目叫 Django Shell它能够在交互模式用项目工程的代码执行相应的操作
-   利用 Django Shell可以代替编写 view 的代码来进行直接操作
-   在 Django Shell下只能进行简单的操作,不能运行远程调式

**数据查询及数据操作**

-   数据库的查询需要使用管理器对象进行
-   通过 Entry.objects 管理器方法调用查询接口

|   方法    |                说明                |
| :-------: | :--------------------------------: |
|   all()   | 查询全部记录，返回QuerySet查询对象 |
|   get()   |       查询符合条件的单一记录       |
| filter()  |       查询符合条件的多条记录       |
| exclude() |     查询符合条件之外的全部记录     |

1.  all()

    -   用法： Entry.objects.all()
    -   作用：查询 Entry 实体中所有的数据
        -   等同于 `select * from table_name`
    -   返回值：QuerySet容器对象，内部存放Entry 实例

    ```python
    from books import models

    books=models.Book.objects.all()
    for book in books:
        print('书名'，book.title,'出版社'， book.pub)
    ```

2.  在模型类中定义 `def __str__(self)` 方法可以将自定义默认的字符串

    ```python
        class Book(models.Model):
            title=...
            def __str__(self):
                return "书名：%s，出版社：%s，定价：%s"%(self.title,self.pub,self.price)
    ```

3.  查询返回指定列（字典表示）

    -   方法：values('列1','列2')
    -   用法：Entry.objects.values(...)
    -   作用：
        -   查询部分列的数据并返回
        -   `select 列1，列2 from xxx;`
    -   返回值：QuerySet
        -   返回查询结果容器，容器内存字典，每个字典代表一条数据
        -   格式：{'列1':值1，'列2',值2}

    ```python
    from books import models

    books=models.Book.objects.values('title','pub')
    for book in books:
        print('书名'，book['title'],'出版社'， book['pub'])
    ```

4.  查询返回指定列（元组表示）

    -   方法：values_list('列1','列2')
    -   用法：Entry.objects.values_list(...)
    -   作用：
        -   返回元组形式的查询结果
    -   返回值：QuerySet
        -   返回查询结果容器，容器内存元组，每个字典代表一条数据
        -   会将查询出来的数据封装到元组中再封装到查询集合 QuerySet中

5.  排序查询

    -   方法: order_by
    -   用法: Entry.objects.order_by(...)
    -   作用：
        -   与all()方法不同,它会用 SQL 语句的 ORDER BY 子句对查询结果进行根据某个字段选择性的进行排序
    -   返回值：QuerySet
        -   返回查询结果容器，容器内存元组，每个字典代表一条数据
        -   会将查询出来的数据封装到元组中再封装到查询集合 QuerySet中
    -   默认是按升序排序，降序排序需要在列前加 `-` 表示

    ```python
        books=models.Book.objects.order_by('-price')
    ```

6.  条件查询多条记录

    -   方法: filter(条件)
    -   用法: Entry.objects.filter(属性1=值1，属性2=值2)
    -   返回值: QuerySet 容器对象，内部存放 Entry 实例
    -   说明:

        -   当多个属性在一起时为 与关系(and)，即 `Books.objects.filter(price=20,pub="清华")`

    -   字段查找

        -   字段查询是指如何指定 SQL 语句中 WHERE 子句的内容。
        -   字段查询需要通过 Queryset 的 filter, exclude() and get()的关键字参数指定。
        -   非等值条件的构建,需要使用字段查询
        -   eg.:`books = models.Book.objects.filter(price__gt=50)`

    -   查询谓词

        -   每一个查询谓词是一个 独立的查询功能

        -   语法：xxx.filter(属性\_\_查询谓词=值)

        1.  `__exact`:等值匹配（默认）
            -   `Author.objects.filter(id__exact = 1)`
        2.  `__gt`:大于指定值
            -   `Author.objects.filter(id__gt = 1)`
            -   `__gte`:大于等于
        3.  `__lt`:小于指定值

            -   `Author.objects.filter(id__lt = 5)`
            -   `__lte`:小于等于

        4.  `__contians`:包含指定值
            -   `Author.objects.filter(name__contains = '老')`
        5.  `__startswith`:以指定内容开头
            -   `Author.objects.filter(name__startswith='周')`
        6.  `__endswith`:以指定内容结尾

            -   `Author.objects.filter(name__endswith='王')`

        7.  `__in：查找数据在指定范围内`
            -   `Author.objects.filter(age__in=[30,40,50,60,70])`
        8.  `__range`:查找数据在指定区间内的(包含起始和结尾)
            -   `Author.objects.filter(age__range=(30,70))`

        [查询谓词参考](https://yiyibooks.cn/xx/Django_1.11.6/ref/models/querysets.html)

    -   不等的条件筛选

        -   作用：返回不包含此 条件 的记录的数据集
        -   exclude(条件)
        -   eg.: `models.Book.objects.exclude(price__gt=50)`

7.  查询指定的一条数据

    -   作用:返回满足条件的唯一一条数据
    -   返回值：Entry**对象**
    -   说明
        -   该方法只能返回一条数据
        -   查询结果多余一条数据則抛出 Model.MultipleObjectsReturned 异常
        -   查询结果如果没有数据则抛出 Model.DoesNotExist 异常

8.  修改数据记录

    1.  修改单个实体的某些字段值

        1.  查
            -   通过 get() 得到要修收的实体对象
        2.  改
            -   通过 对象.属性 的方式修改数据
        3.  保存
            -   通过对象 save() 保存数据

        ```python
        book=models.Book.objects.get(id=5)
        book.price=120
        book.save()
        ```

    2.  通过 QuerySet 批量修改 对应的全部字段

        -   直接调用 Queryset的 update(属性=值)实现批量修改

        ```python
        books=models.Book.objects.filter(market_price=9999)
        books.update(market_price=58)
        ```

9.  删除记录

    -   删除记录是指删除数据库中的一条或多条记录
    -   删除单个 Entry 对象 或 删除一个查询结果集 (Query Set) 中的全部对象都是调用 delete()方法

    1.  删除单个对象

        -   步骤

            1.  查找查询结果对应的一个数据对象
            2.  调用这个数据对象的 delete() 方法实现删除

    2.  删除查询结果集

    -   步骤

        1.  查找查询结果集中满足条件的全部 Queryset查询集合对象
        2.  调用查询集合对象的 delete() 方法实现删除

        ```python
            book=models.Book.objects.get(id=9)
            book.delete()

            books=models.Book.objects.filter(id__gte=9)
            books.delete()
        ```

10. 聚合查询

    -   聚合查询是指对一个数据表中的一个字段的数据进行部分或全部进行统计查询,查 bookstore_book数据表中的全部书的平均价格,查询所有书的总个数等都要使用聚合查询

    1.  不带分组聚合

        -   不带分组的聚合查询是指导将全部数据进行集中统计查询
        -   聚合函数

            -   定义模块：`django.db.models`
            -   用法：`from django.db.models import *`
            -   聚合函数：
                -   `Sum \ Avg \ Count \ Max \ Min`
            -   语法：
                -   \`Entry.objects.aggregate(结果变量名=聚会函数('列'))
            -   返回结果
                -   由 结果变量名 和值组成的字典
                -   格式为：
                    -   `{"结果变量名"：值}`

            ```python
            from django.db.models import *

            models.Book.objects.aggregate(myAvg=Avg("price"))
            ```

    2.  分组聚合

        -   分组聚合是指通过计算查询结果中每一个对象所关联的对象集合,从而得出总计值(也可以是平均值或总和),即查询集的每一项生成聚合。
        -   语法 `QuerySet.annotate(结果变量名=聚合函数('列'))`
        -   用法步骤

        1.  通过先用查询结果 Entry.objects.values 查找查询要 分组聚合的列

            -   `pub_set=models.Book.objects.values('pub')`

        2.  通过返回结果的 QuerySet.annotate 方法分组聚合得到分组结果

            -   QuerySet.annotate(名=聚合函数('列'))
            -   返回 QuerySet 结果集，内部存储结果的字典

            -   `pub_set.annotate(myCount=Count('pub'))`

11. F对象

    -   一个F对象代表数据库中某个字段的信息
    -   F对象通常是对数据库中的字段值在不加载到内存中的情况下直接在数据库服务器端进行操作
    -   F对象在数据包 django.db.models 中。使用时需要通过如下语句进行加载
        -   `from django.db.models import F`

    1.  作用:

        -   在执行过程中获取某列的值并对其直接进行操作
        -   当同时对数据库中两个字段的值进行比较获取 Queryset 数据集时,可以便用F对象

    2.  说明:

        -   一个F()对象代表了一个model的字段的值

    3.  使用它就可以直接参考 model 的 field 和执行数据库操作而不用再把它们( model field)查询出来放到 python内存中。

        ```python
        from django.db.models import F
        # 每个字段统一 升，降 值
        models.Book.objects.all().update(market_price=F('market_price')+10)

        # 两个字段进行比较
        books = models.Book.objects.filter(market_price__gt=F('price'))
        ```

12. Q对象

    -   当在获取查询结果集 使用复杂的逻辑或丨、逻辑非~ 等操作时可以借助于Q对象进行操作

    1.  作用:

        -   在条件中用来实现除 and(&) 以外的 or(|) 或 not(-) 操作

    2.  运算符：

        -   `&` 与操作
        -   `|` 或操作
        -   `~` 非操作

    3.  语法

        -   `Q(条件1) | Q(条件2)`
        -   `Q(条件1) & Q(条件2)`
        -   `Q(条件1) &~ Q(条件2)`   条件1成立且条件2不成立


    ```python

        models.Book.objects.filter(Q(pub="清华")|Q(price__gt=50))

        models.Book.objects.filter(Q(pub='清华') | Q(price__gt=50), id__lt=3)  # , 表示与关系
    ```

### 原生的数据库操作方法

1.  使用 Entry.objects.raw() 进行教据库查询操作查询

    -   在 django 中,可以使用模型管理器的 raw 方法来执行 select 语句进行数据查

    -   语法：`Entry.objects.raw('sql语句')`
    -   返回值：QuerySet

2.  使用 django中 的游标 cursor对数据库进行增删改操作

    -   在 DJango 中可以使用如 UPDATE, DELETE 等 SQL 语句对数据库进行操作。
    -   在 DJango 中使用上述非查询语句必须使用游标进行操作

    -   使用步骤：

        1.  Django 中的 cursor 游标定义在django.db.connection 中，使用前需要导入。
            -   `from django.db impot connection`
        2.  用创建 cursor 类的构造函数创建 cursor 对象,再使用 cursor 对象为保证在出现异常时能释放 cursor 资源通常使用 with 语句进行创建操作

        ```python
        from django.db import connection

        with connection.cursor() as cur:
             cur.execute('update books_book set pub="xxx" where id=8;')
        ```

### admin后台数据库管理

-   django 提供了比较完善的后台管理数据库的接口,可供开发过程中调用和測试使用
-   django 会搜集所有已注册的模型类,为这些模型类提拱数据管理界面,供开发者使用

    -   使用步骤：

        1.  创建后台管理帐号
            -   `python manage.py createsuperuser`
        2.  据提示完成注册
        3.  登录：127.0.0.1/admin

#### 自定义后台管理数据表

-   若要自己定义的模型类也能在 /admin 后台管理界中显示和管理,需要将自己的类注册到后台管理界面
-   添加自己定义模型类的后台管理数据表的,需要用 admin.site.register(自定义模型类) 方法进行注册

-   配置步骤如下:

    1.  在应用app中的 admin.py 中导入注册要管理的模型 models 类
        -   `from . import models`
    2.  调用 admin.site.register 方法进行注册

        -   `from django.contrib import admin`
        -   \`admin.site.register(自定义模型类)

        ```python
            from django.contrib import admin
            from . import models

            admin.site.register(models.Book)
        ```

    3.  修改后台 Models的展现形式

        -   后台 Models 默认的展现形式 xxx object
        -   可以在 models.py 中重写 `__str__` 展现形式

        ```python
            class Book(models.Model):
                ...
                def __str__(self):
                    return "id：%d，书名：%s，出版社：%s，" % (self.id, self.title, self.pub)
        ```

-   模型管理器类

    -   作用:
        -   用后台管理界面添加便于操作的新功能
    -   说明:
        -   后台模型管理器类须继承自 `django.contrib.admin` 里的 ModelAdmin 类
    -   使用

        1.  在 应用app/admin.py 中定义 模型管理器类
        2.  注册管理器与模型类关联

    -   模型管理器类 ModelAdmin 中实现的高级管理功能

        1.  list_display 去控制哪些字段会显示在 Admin 的修改列表页面中。
        2.  list_display_links 可以控制 list_display 中的字段是否应该链接到对象的“更改"页面
        3.  list_filter 设置激活 Admin 修改列表页面右侧栏中的过滤器
        4.  search_fields 设置启用 Admin 更改列表页面上的搜索框。
        5.  list_editable 设置为模型上的字段名称列表,这将允许在更改列表页面上进行编辑。

        [参考](https://yiyibooks.cn/xx/Django_1.11.6/ref/contrib/admin/index.html)

        ```python
            class BookManager(admin.ModelAdmin):
                list_display = ['id', 'title', 'pub', 'price', 'market_price']
                list_display_links = ['id', 'title']
                list_filter = ['pub']
                search_fields = ['title', 'pub']
                list_editable = ['market_price']

            admin.site.register(models.Book, BookManager)
        ```

### 数据库表管理

1.  修改模型类字段的显示名字

    -   模型类各字段的第一个参数为 verbose_name,此字段显示的名字会在后台数据库管理页面显示

2.  通过 Meta 内嵌类, 定义模型类的属性及展现形式

    -   模型类可以通过定义内部类 class Meta 来重新定义当前模型类和数据表的一些属性信息

    ```python
        class Book(models.Model):
            ...
            class Meta:
                1.  db_table = '数据表名'
                    -   该模型所用的数据表的名称。(设置完成后需要立马更新同步数据库)
                2.  verbose_name = '单数名'
                    -   给模型对象的一个易于理解的名称(单数),用于显示在 /admin 管理界面中
                2.  verbose_name_plural = '复数名'
                    -   该对象复数形式的名称(复数),用于显示在 /admin 管理界面中
    ```

### 数据表关联关系映射 Relationship Map

-   在关系型数据库中,通常不会把所有数据都放在同一张表中,这样做会额外占用内存空间,
-   在关系列数据库中通常用表关联来解决数据库。
-   常用的表关联方式有三种

    1.  一对一映射: 一对一是表示现实事物间存在的一对一的对应关系。

        -   语法

            ```python
                class A(models.Model):
                    ...

                class B(models.Model):
                    属性=models.OneToOneField(A)
            ```

        -   示例

            ```python
            class Author(models.Model):
                ...
                name=models.CharField('姓名',max_length=50)

            class Wife(models.Model):
                name=models.CharField('姓名',max_length=50)
                author=models.OneToOneField(Author)
            ```

        -   查询

            -   在 Wife 对象中,通过 author 属性找到对应的 author 对象
            -   在 Author 对象中,通过 wife 属性找到对应的 wife 对象

        -   创建一对一数据记录

            ```python
                from . import models
                author1=models.Author.objects.create(name='teacherW',age=22)
                wife1=models.Wife.objects.create(name='wifeW',author=author1) # 一对一可以没有对应的数据，可略
            ```

        -   一对一数据的相互获取

            1.  正向查询
                -   直接通过关联属性（引用属性）查询即可
                    -   `wife = models.Wife.objects.get(name='fff')`
                    -   `wife.name / wife.author.name`
            2.  反向查询

                -   通过反向引用属性查询
                -   反向引用属性为 实例时象.引用类名(小写) 如作家的反向引用为 作家对象.wife
                -   当反向引用不存在时,则会触发异常

                ```python
                authorW=models.Author.objects.get(name='teacherW')
                try:
                    print(authorW.name,authorW.wife.name)
                except:
                    print(authorW.name)
                ```

        -   作用
            -   主要是解决常用数据不常用数据的存储问题，把经常加载的一个数据放在主表中,不常用数据放在另一个副表中,这样在访问主表数据时不需要加载副表中的数据以提高访问速度提高效率和节省内存空间，如经常把书的内容和书名建成两张表,因为在网站上经常访问书名等信息,但不需要得到书的内容。

    2.  一对多映射

        -   一对多是表示现实事物间存在的一对多的对应关系
        -   创建：

            -   当一个A类对象可以关联多个B类对象时

                ```python
                class A(models.Model):
                    ...

                class B(models.Model):
                    属性1 = models.ForeignKey(多对一中的'一'的模型类, ...)
                    属性2 = models.ForeignKey(多对一中的'一'的模型类, ...)
                ```

                -   外键类 ForeignKey

                    -   构造函数
                        -   `ForeignKey(to,on_delete,**options)`
                    -   常用参数
                        -   on_delete
                            1.  models.CASCADE 级联删除。Django模拟SQL约束 ON DELETE CASCADE 的行为，并删除包含 ForeignKey 的对象
                            2.  models.PROTECT 抛出 ProtectedError 以阻止被引用的对象删除
                            3.  SET_NULL 设置 ForeignKey null,只用 null 是 True 才有可能
                            4.  SET_DEFAULT 将 ForeignKey 设置为其默认值，必须设置 ForeignKey 的默认值
                            5.  ...
                        -   \*\*options 可以是常用的字段选项
                            -   eg.: null,unique ...

                    ```python
                    # 创建
                    class Publisher(models.Model):
                        name = models.CharField('名称', max_length=50, unique=True)

                    class Book(models.Model):
                        ...
                        publisher = models.ForeignKey(Publisher, null=True)

                    # 添加
                    pub1=models.Publisher.objects.get(name='清华')
                    models.Book.objects.create(title='React',publisher=pub1)
                    ```

        -   查询

            -   多对一 查询
                ```python
                abook = models.Book.objects.get(id=8)
                abook.publisher.name
                ```
            -   一对多 查询

                ```python
                pub1=models.Publisher.objects.get(name='清华')

                # 方法1
                books=pub1.book_set.all() # 关联类名_set：反向引用属性 -> 管理器对象 QuerySet

                # 方法2
                books = models.Book.objects.filter(publisher=pub1)

                for i in books:
                    print(i.name)
                ```

    3.  多对多映射：多对多表达对象之间多对多复杂关系

        -   创建

            ```python
            class Publisher(models.Model):
                ...

            class Book(models.Model):
                ...
                authors = models.ManyToManyField(Author)
            ```

        -   通过 Author 查询对应的所有的 Books

            ```python
                author.book_set.all()
                author.book_set.filter()
                author.book_set.create(...)  # 创建新书并关联作者 author
                author.book_set.add(book)  # 添加已有的书为当前作者 author
                author.book_set.clear()  # 删除 author 所有关联的书 （所有）
                author.book_set.remove()  # 删除 该author 所关联的书（一本）

                # 示例：
                lz = models.Author.objects.get(name='lz')
                wj = models.Author.objects.get(name='wj')
                angular = models.Book.objects.get(title='angular')
                lz.book_set.add(angular)
                wj.book_set.add(angular)

                wj.book_set.create(title='vue3')

                # 一对多 查询
                books = wj.book_set.all()
                books = models.Book.objects.filter(authors=wj)
                for book in books:
                    print(book.title)

                # 多对一 查询
                books = models.Book.objects.all()
                for book in books:
                    print(book.title,end=':')
                    for author in book.authors.all(): # book.authors -> 查询结果集
                        print(author.name,end='/')
                    print()
            ```

## Cookies / Session

### Cookies

>  cookies 是保存在客户端测览器上的存储空间,通常用来记录浏览器端自己的信息和当前连接的确认信息

-   cookies 在浏览器上是以 键-值对 的形式进行存储的,键和值都是以 ASCII 字符串的形存储(不能是中文字符串)
-   在 Django 服务器端来 设置 浏览器的 COOKIE 必须通过 HttpResponse 对象来完成

-   HttpResponse 关于 COOKIE 的方法
    -   添加、修改 COOKIE
        -   `HttpResponse.set_cookie(key,value='...',max_age=None,expires=None)`
            -   key: cookie 的名字
            -   value: cookie 的值
            -   max_age: cookie存活的时间，单位为秒
            -   expires: 具体过期时间
            -   eg. `resp.set_cookie('zf', '22', max_age=7*24*60*60)  # 七天`
    -   删除 COOKIE
        -   HttpResponse.delete_cookie(key)
        -   删除指定 key 的 Cookie。如果 key 不存在则什么也不发生
-   Django 中的 COOKIE

    -   使用 响应对象 HttpResponse 将 cookie 保存进客户端

        -   方法1
            ```python
                from django.http import HttpResponse
                resp = HttpResponse()
                resp.set_cookie('cookie_name', 'cookie.value', time)
            ```
        -   方法2
            ```python
                from django.shortcuts import render
                resp=render(request, 'xxx.html',locals())
                resp.set_cookie('cookie_name', 'cookie.value', time)
            ```
        -   方法3
            ```python
                from django.shortcuts import redirect
                resp=redirect('/')
                resp.set_cookie('cookie_name', 'cookie.value', time)
            ```

    -   获取 cookie
        -   通过 request.COOKIES 绑定的字典（dict）获取客户端的 COOKIES 数据
            -   `value = request.COOKIES.get('cookie_name', 'no values!')`

### Session

-   session是在服务器上开辟一段空间用于保留浏览器和服务器交互时的重要数据

-   每个客户端都可以在服务器端有一个独立的 Session

-   http协议是无状态的:每次请求都是一次新的请求,不会记得之前通信的状态

-   客户端与服务器端的一次通信,就是一次会话

-   实现状态保持的方式:在客户端或服务器端存储与会话有关的数据

-   推荐使用 session方式,所有数据存储在服务器端,在客户端 cookie中存储 session_id

-   注意:不同的请求者之间不会共享这个数据,与请求者一一对应

1.  定义：
    -   在服务器上开辟一段空间用于保留浏览器和服务器交互时的重要数据
2.  Django 下启用 Session

    -   配置：

        -   在 主模块 settings.py 中添加
            ```python
                INSTALLED_APPS=[
                    ...
                    'django.contrib.sessions',（默认已添加）
                ]
                MIDDLEWARE=[
                    ...
                    'django.contrib.sessions.middleware.SessionMiddleware',
                ]
            ```

    -   基本操作

        -   session对象是一个类似字典的 SessionStore 类型的对象,可以用类拟于字典的方式进行操作
        -   session 只能够存储能序列化的数据，如字典、列表等
        -   保存 session 的值到服务器
            -   request.session['KEY'] = VALUE
        -   获取 session 的值
            -   `VALUE=request.session['KEY']`
            -   或
            -   `VALUE=request.session.get('KEY','缺省值')`
        -   删除：`del request.session['KEY']`

    -   在 settings.py 中有关 session 的设置

        1.  SESSION_COOKIE_AGE：作用 sessionid 在 cookies 中的保存时长 SESSION_COOKIE_AGE=60\*30
        2.  SESSION_EXPIRE_AT_BROWSER_CLOSE=True 设置只要浏览器关闭时，session就失效

    -   注：
        -   当使用 session 时需要迁移数据库，否则会出现错误
        -   在模板中使用 session 是，由于模板中默认不能传递参数，故使用 `request.session.KEY` 获取值

## 中间件

> 中间件是 DJango 请求响应处理的钩子框架。它 是一个轻量级的、低级的“插件"系统,用于全局改变 DJango 的输入或输出

-   每个中间件组件负责做一些特定的功能。例如, Django包含一个中间件组件 AuthenticationMiddleware ,它使用会话将用户与请求关联起来
-   中间件被记录在 built-in middleware reference中

![Python-Django_Middleware](http://localhost:9710/blog/Python/Python-Django_Middleware.jpeg)

-   中间件类

    -   中间件类 须继承自`django.utils.deprecation.MiddlewareMixin`类
    -   中间件类须实现下列五个方法中的一个或多个

        1.  `def process_request(self,request):`
            -   执行视图之前被调用,在每个请求上调用,返回 None 或 Httpresponse 对象
        2.  `def process_view(self,request,callback,callback_args,callback_kwargs):`
            -   调用视图之前被调用,在每个请求上调用,返回None 或 Httpresponse 对象
        3.  `def process_response(self,request,response):`
            -   所有响应返回浏览器之前被调用，在每个请求上调用，返回 HttpResponse 对象
        4.  `def process_exception(self,request,exception):`
            -   当处理过程中抛出异常时调用，返回 HttpResponse 对象
        5.  `def process_template_response(self,request,response):`
            -   在视图刚好执行完毕之后被调用，在每个请求上调用，返回实现 render 方法的响应对象

        -   注:中间件中的大多数方法在返回 None 时表示忽略当前操作进入下一项事件,当返回 Httpresponese 对象时表示此请求结果,直接返回给客户端

        ```python
        # 注册中间件：主目录 settings.py 添加项
        # 中间件目录 /mymiddleware/check_login.py <class MyMiddleWare>
        MIDDLEWARE = [
            ...
            'mymiddleware.check_login.MyMiddleWare',
            'mymiddleware.check_login.VisitLimit',
        ]

        # check_login.py
        # 使用
        from django.http import HttpResponse
        from django.http import HttpResponseRedirect
        from django.utils.deprecation import MiddlewareMixin

        import re

        class MyMiddleWare(MiddlewareMixin):
            """
                访问权限
            """
            def process_request(self, request):
                print('process request  callback')
                # return None
                if re.match(r'^/bookstore', request.path_info) \
                        and ('userinfo' not in request.session):
                    return HttpResponseRedirect('/userinfo/login')
                return None

        class VisitLimit(MiddlewareMixin):
            """
                限制一个 IP 访问 /user/login 的次数不能超过 5次
            """

            visit_times = {}  # 客户端IP地址访问次数

            def process_request(self, request):
                ip_address = request.META['REMOTE_ADDR']
                if not re.match('^/text', request.path_info):
                    return
                times = self.visit_times.get(ip_address, 0)
                print("IP", ip_address, "___ visit ", times,
                    "times. for ", request.path_info)
                self.visit_times[ip_address] = times+1
                if times < 5:
                    return
                return HttpResponse('Forbidden...')
        ```

### 跨站请求伪造保护 CSRF

-   跨站请求伪造攻击

    -   某些恶意网站上包含链接、表单按钮或者 Javascript,它们会利用登录过的用户在浏览器中的认证信息试图在你的网站上完成某些操作,这就是跨站请求伪造

-   CSRF

    -   Cross-Site Request ForeignKey

-   说明：

    -   CSRF 中间件和模板标签提供对跨站请求伪造简单易用的防护。

-   作用：

    -   不让其他表单提交到此 Django 服务器

-   解决方案

    1.  取消 csrf （不推荐）
        -   删除 settings.py 中 MIDDLEWARE 中的`'django.middleware.csrf.CsrfViewMiddleware',`
    2.  开发验证
        ```python
            # 在视图处理函数增加：@csrf_protect z装饰器
            @csrf_protect
            def post_views(request):
                pass
        ```
    3.  通过验证

        -   在表单中增加一个标签
        -   `{% csrf_token %}`

            ```python
                <form action="/userinfo/login" method="POST">
                    {% csrf_token %}
                    ...
                </form>
            ```

## 分页

-   分页是指在 web页面 有大量数据需要显示时,当一页的内容太多不利于间读和不利于数据提取的情况下,可以分为多页进行显示
-   Django 提供了一些类来帮助你管理分页的数据一也就是说,数据被分在不 同页面中,并带有 上一页/下一页 链接。
-   这些类位于 `django/core/paginator.py` 中

### paginator

-   paginator 对象

    -   Paginator(object_list,per_page)
    -   参数
        -   object_list 可使用查询结果集
        -   per_page 每页数据个数
    -   返回值
        -   分页对象

-   Paginator 属性

    -   count: 对象总数，即查询出来的数据总条数
    -   num_pages: 总页数
    -   page_range: 从 1 开始的 range 对象，值即：range(1,num_pages+1)
    -   per_page: 每页数据个数，即将创建 Paginator 时传入的per_page，设置为该属性

-   Paginator 方法

    -   Paginator.page(number)
        -   参数 number 为页码信息（从1开始）
        -   返回当前 number 页面对应的页信息
        -   如果提供的页面不存在，抛出 InvalidPage 异常

-   Paginator 异常 exception

    -   InvalidPage：当向page() 传入一个无效的页码值时抛出
    -   PageNotAnInteger: 当向page() 传入一个不是整数的值时抛出
    -   EmptyPage: 当向page() 提供一个有效值，但是那个页面没有任何对象时抛出

### Page 对象

-   创建对象 Paginator 对象的 page()方法，返回Page对象，不需要手动构造
-   Page 对象属性
    -   object_list: 当前页上所有对象的列表
    -   number: 当前页的序号，从1开始
    -   paginator: 当前page对象相关的Paginator对象
-   Page 方法
    -   has_next(): 如果有下一页返回 True
    -   has_previous(): 如果有上一页返回 True
    -   has_other_pages(): 如果有上一页或下一页返回 True
    -   previous_page_number(): 返回下一页的页码，如果下一页不存在，抛出 InvalidPage 异常
    -   next_page_number(): 返回上一页的页码，如果上一页不存在，抛出 InvalidPage 异常
    -   len(): 返回当前页面对象的个数
-   说明
    -   Page对象是可选代对象,可以用 for 语句来访问当前页面中的每个对象
-   示例：

    ```python
    # views.py
    def bookstore_book(request):
        books = models.Book.objects.all()
        paginator = Paginator(books, 5)
        p_count = paginator.count
        p_num = paginator.num_pages
        p_range = paginator.page_range
        p_per = paginator.per_page

        cur_page = request.GET.get('page', 1)
        page = paginator.page(cur_page)  # 当前页所有信息
        return render(request, 'bookstore_book.html', locals())
    ```

    ```html
    <!-- 分页部分 html，数据列表部分使用 for abook in page: 遍历即可 -->
    <!-- 模板中调用函数不加括号 -->
    <nav aria-label="Page navigation">
        <ul class="pagination">
            <li
                class="page-item{% if page.has_previous %}{% else %} disabled{% endif %}"
            >
                <a
                    class="page-link"
                    href="{{ url 'book' }}?page={% if page.has_previous %}{{page.previous_page_number}}{% else %}{% endif %}"
                    aria-label="Previous"
                >
                    <span aria-hidden="true">&laquo;</span>
                </a>
            </li>
            {% for p in p_range %}
            <li
                class="page-item{% if p == page.number %} disabled{% else %}{% endif %}"
            >
                <a class="page-link" href="{{ url 'book' }}?page={{ p }}"
                    >{{ p }}</a
                >
            </li>
            {% endfor %}
            <li
                class="page-item{% if page.has_next %}{% else %} disabled{% endif %}"
            >
                <a
                    class="page-link"
                    href="{{ url 'book' }}?page={% if page.has_next %}{{page.next_page_number}}{% else %}{% endif %}"
                    aria-label="Next"
                >
                    <span aria-hidden="true">&raquo;</span>
                </a>
            </li>
        </ul>
    </nav>
    ```

## Django中的 forms 摸块

-   在 Django 中提供了 forms 模块用 forms 模块可以自动生成form内部的表单控件,同时在服务器端可以用对象的形式接收并操作客户端表单元素,并能对表单的数据进行服务器端验证

1.  forms 模块的作用

    -   通过 forms 模块,允许将表单与 class 相结合,允许通过 class 生成表单

2.  使用 forms 模块的步骤

    1.  在应用中创建 forms.py
    2.  导入 django 提供的 forms
        -   from django import forms
    3.  创建 class ,一个 class 会生成一个表单
        -   定义表单类：`class ClassName(forms.Form):`
    4.  在 class 中 创建类属性
        -   一个类属性对应到表单中是一个控件
    5.  利用 Form 类型的对象自动成表单内容
    6.  读取 form 表单并进行验证数据

3.  forms.Form 的语法

    -   属性=forms.Field类型(参数)

    1.  类型

        ```python
        class XXX(forms.Form):
            forms.CharField() : 文本框 <input type="text">
            forms.ChoiceField() : 下拉选项框 <select>
            forms.DateField() : 日期框 <input type="date">
        ```

    2.  参数

        1.  label  提示文本，即 label标签内的内容
        2.  widget  指定小部件
        3.  initial  空间初始值（针对文本框类型），表单中的 value值
        4.  required  是否为必填项，默认值为 True

        -   [参考](https://yiyibooks.cn/xx/Django_1.11.6/ref/forms/fields.html#module-django.forms.fields)

4.  在模板中解析 form 对象

    -   在 视图中创建 form 对象 并发送到模板中解析

        1.  手动解析

            ```django
            {% for f in myform %}
            {{f.label}} {{f}}
            {% endfor %}
            ```

        2.  自动解析
            1.  {{form.as_p}}
                -   将 form 中的每个属性（控件/文本）都使用 p标签 包裹起来再显示
            2.  {{form.as_ul}}
                -   将form中的每个属性(控件/文本)都使用 li标签 包裹起来再显示
                -   注意:必须手动提供 ol 或 ul 标签
            3.  {{form.as_table}}
                -   将form中的每个属性(控件/文本)都使用 tr标签 包裹起来再显示
                -   注意:必须手动提供 tablet标记

5.  通过 forms对象 获取表单数据

    1.  通过 forms.Form 子类的构造器来接收post数据
        -   `form=XXXForm(request.POST)`
    2.  必须是form通过验证后,才能取值
        -   form.is_valid()
            -   返回 True : 通过验证可以取值
            -   返回 False : 暂未通过验证则不能取值
    3.  通过 form.cleaned_data 字典的属性接收数据

6.  widget 内置小部件
    -   表示的是生成到网页上的控件以及一些其他的 html属性

|           widget值           |     对应的 type      |
| :--------------------------: | :------------------: |
|       forms.TextInput        |     type="text"      |
|     forms.PasswordInput      |   type="password"    |
|      forms.NumberInput       |    type="number"     |
|       forms.EmailInput       |     type="email"     |
|        forms.URLInput        |      type="url"      |
|      forms.HiddenInput       |    type="hidden"     |
|     forms.CheckboxInput      |   type="checkbox"    |
| forms.CheckboxSelectMultiple |   type="checkbox"    |
|      forms.RadioSelect       |     type="radio"     |
|        forms.Textarea        |    textarea 标签     |
|         forms.Select         |     select 标签      |
|     forms.SelectMultiple     | select multiple 标签 |

    -   使用：

            -   在控件类型的基础之上还能指定控件的一些 html属性值

            ```django
                class RegForm(forms.Form):
                    ...
                    password = forms.CharField(
                        label='pl. input password', widget=forms.PasswordInput(
                            attrs={
                                'myattr1': 'myvalue1',
                                'myattr2': 'myvalue2',
                            }
                        )
                    )
            ```

### Django form 表单验证

-   django form提供表单和字段验证
-   当在创建有不同的多个表单需要提交的网站时,用表单验证比较方便验证的封装
-   当调用 form.is.valid() 返回 True 表示当前表单合法,当返回 False 说明表单验证出现问题
-   验证步骤
    -   先对 form.XXXField() 参数值进行验证,比如 min_length, max_length, validators=[...] 如果不符合 form.is_valid() 返回False
    -   对各自 from.clean_xxx属性名(self) 方法对相应属性进行验证,如果验证失败 form.is_valid() 返回False
    -   调用 form.clean(self) 对表单的整体结构进行验证,如果验证失败 form.is_valid() 返回False
    -   以上验证都成功 form.is_valid() 返回 True
-   验证方法

    -   validators=[验证函数1,验证函数2]
        -   验证函数验证失败抛出 forms.ValidationError
        -   验证成功返回 None
    -   def clean_xxx属性(self)
        -   验证失败必须抛出 forms.ValidationError
        -   验证成功必须返回 xxx属性的值
    -   def clean(self)
        -   验证失败必须抛出 forms.ValidationError
        -   验证成功必须返回 self.cleaned_data

    ```python
        #  forms.py
        from django import forms
        import re

        mobile_re = re.compile(
            r'^(13[0-9]|15[0123456789]|17[678]|18[0-9]|14[57])[0-9]{8}$')

        def mobile_validate(value):
            if not mobile_re.match(value):
                raise forms.ValidationError('手机号码各式错误')

        class RegForm(forms.Form):
            username = forms.CharField(label='pl. input name')
            password = forms.CharField(
                label='pl. input password', widget=forms.PasswordInput)
            password2 = forms.CharField(
                label='pl. input password2', widget=forms.PasswordInput)
            mobile = forms.CharField(label='mobile phont', validators=[mobile_validate])

            def clean(self):
                pwd1 = self.cleaned_data['password']
                pwd2 = self.cleaned_data['password2']
                if pwd1 != pwd2:
                    raise forms.ValidationError('PASSWORD INCONSISTENCY!!!')
                return self.cleaned_data

            def clean_username(self):
                username = self.cleaned_data["username"]
                if len(username) < 6:
                    raise forms.ValidationError('username to short')

                return username

        # views.py
        from . import forms
        def test_form(request):
            if request.method == 'GET':
                myform = forms.RegForm
                return render(request, 'userinfo/test_form.html', locals())
            elif request.method == 'POST':
                myform = forms.RegForm(request.POST)
                if myform.is_valid():
                    print(myform.cleaned_data)
                    return HttpResponse('ok')
                else:
                    return HttpResponse('error..')

        # html
        <form action="/userinfo/test_form" method="POST">
            {% csrf_token %}
            {{ myform.as_p }}
            <input type="submit" value="submit" />
        </form>
    ```

    -   表单验证不是必须 使用类创建，手动创建的也能验证，属性名就是 name的值

### 文件上传

-   文件上传必须为 POST 提交方式
-   表单 <form> 中文件上传时必须有带有 `enctype=" multipart/form-data"`时才会包含文件内容数据
-   表单中 用 `<form type="file" name="xxx">` 标签上传文件

    -   名字 xxx 对应 request.FILES['xxx'] 对应的内存缓冲文件流对象，可通过 request.FILES['xxx'] 返回的对象获取上传文件数据
    -   file=request.FILES['xxx']
        -   file 绑定文件流对象，可以通过文件流对象的如下信息获取文件数据
            -   file.name 文件名
            -   file.file文件的字节流数据

-   如下上传文件为图片类型,可以用模块类属性定义成 models.ImageField 类
    -   `image_file=models.ImageField(upload_to='images/')`
    -   ps. 如果属性类型为 ImageField 需要安装 Pilow
    -   pip install Pilow==3.4.1
    -   图片存储路径

```python
    from django.conf import settings
    import os

    def on_upload(request):
        if request.method == 'GET':
            return render(request, 'upload.html', locals())
        elif request.method == 'POST':
            myfile = request.FILES['myfile']
            with open(os.path.join(settings.MEDIA_DIR, myfile.name), 'wb') as fd:
                file_content = myfile.file.read()
                fd.write(file_content)
            return HttpResponse('ok')

    ##### settings.py
    MEDIA_DIR = os.path.join(BASE_DIR, 'static/images')
```

## Django 中的用户认证(使用 Django 认证系统)

-   Django带有一个用户认证系统。它处理用户账号、组、权限以及基于 cookie 的用户会话。

-   作用

    1.  添加普通用户和超级用户
    2.  修改密码
    3.  登陆和退出管理
    4.  查看已登陆用户

-   [文档](https://yiyibooks.cn/xx/Django_1.11.6/topics/auth/default.html)

-   User 模型类

    -   位置：`from django.contrib.auth.models import User`

    -   默认 user 的基本属性

|    属性名    |                                     类型                                      | 是否必须存在 |
| :----------: | :---------------------------------------------------------------------------: | :----------: |
|   username   |                                    用户名                                     |      是      |
|   password   |                                     密码                                      |      是      |
|    email     |                                     邮箱                                      |     可选     |
|  first_name  |                                      名                                       |      /       |
|  last_name   |                                      姓                                       |      /       |
| is_superuser |                          是否是管理员账户（/admin）                           |      /       |
|   is_staff   |                           是否可访问 admin 管理界面                           |      /       |
|  is_active   | 是否是活跃用户 默认为 True，一般不删除用户，而是将用户 is_active 设置为 False |      /       |
|  last_login  |                                 上次登录时间                                  |      /       |
| date_joined  |                                 用户创建时间                                  |      /       |

-   auth 基本模型操作

    -   创建 普通用户/超级用户

        ```python
        from django.contrib.auth import models

        user=models.User.objects.create_user(username='...',password='...',email='...')
        user=models.User.objects.create_superuser(username='...',password='...',email='...')

        ...
        user.save()
        ```

    -   删除用户

        ```python
            try:
                user=models.User.objects.get(username='...')
                user.is_active=False
                user.save()
                print('delete user..')
            except:
                print('delete user error..')
            return HttpResponseRedirect('/user/info')
        ```

    -   修改密码

        ```python
            try:
                user = models.User.objects.get(username='...')
                user.set_password = '...'  # 不能直接使用 user.password 设置，set_password 有加盐加密设置
                user.save()
                return HttpResponse('update password user..')
            except:
                return HttpResponse('update password user error')
        ```

    -   验证密码

        ```python
            try:
                user = models.User.objects.get(username='待验证用户名')
                if user.check_password('待验证密码'):
                    return HttpResponse('yes')
                else:
                    return HttpResponse('no')
            except:
                return HttpResponse('user not exist')
        ```

### Web 请求认证

-   `from django.contrib.auth import authenticate,login,logout`

-   认证用户

    -   函数 `authenticate(username='用户名',password='密码')`
        -   验证用户名和密码是否存在于用户中，如果存在返回 User 对象，如果不合法返回 None

-   登录用户

    -   函数 login(request,user)
    -   参数 request 为 HttpRequest 对象
    -   参数 user 为 User对象
    -   login() 使用 Django 的 session 框架来将用户的ID保存在 session 中

-   登出用户

    -   函数 logout(request)

```python
    from django.shortcuts import render
    from django.http import HttpResponse
    from django.contrib.auth import models
    from django.contrib.auth import authenticate, login, logout

    # Create your views here.

    def mylogin2(request):
        if request.method == 'GET':
            return render(request, 'user/login.html', locals())
        elif request.method == 'POST':
            username = request.POST.get('username', '')
            password = request.POST.get('password', '')

            # try:
            #     user = models.User.objects.get(username=username)  # 缺少 is_actice=True
            #     if user.check_password(password):
            #         return HttpResponse('login success')
            #     else:
            #         return HttpResponse('password error..')

            # except:
            #     return HttpResponse('user not exist..')

            # 简化认证
            user = authenticate(username=username, password=password)
            if user is not None:
                # 登录
                login(request, user)
                return HttpResponse('login success')
            else:
                return HttpResponse('login fail')

    def mylogout2(request):
        logout(request)
        return HttpResponse('logout')

    def myregister2(request):
        if request.method == 'GET':
            return render(request, 'user/register.html', locals())
        elif request.method == 'POST':
            username = request.POST.get('username', '')
            password = request.POST.get('password', '')

            # ...

            try:
                user = models.User.objects.create_superuser(
                    username=username,
                    password=password,
                    email='657829956@qq.com'
                )
                user.save()
                return HttpResponse('reg success')
            except:
                return HttpResponse('reg fail')
```

### 自定义 user 模型

-   当 django自带的 django.contrib.auth.models.User 的属性不能满足我们现有的需求时,可以自定义User模型
-   如,向用户中加入姓别和家庭住址字段等
-   自定义 User 类需要继承自 `django.contrib.auth.models.AbstractUser`
-   自定义 User 类后需要再次做迁移操作
-   [文档](https://yiyibooks.cn/xx/Django_1.11.6/topics/auth/customizing.html)

```python
from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
    address = models.CharField("...", max_length=0)
```

## 项目部署

-   项目部置在软件开发完毕后,将开发机器上运行的开发板软件实际安装到服务器上进行长期运行

-   部署要分以下几个步骤进行

    1.  在安装机器上安装和配置同版本的数据库
    2.  django 项目迁移(在安裝机器上配置与开发环境相同的 python版本 及 依懒的包 )
    3.  用 uwsgi 替代 python3 manage.py runserver 方法启动服务器
    4.  配置 nginx 反射代理服务器
    5.  用 nginx 配置静态文件路径,解决静态路径问题

-   步骤

    1.  安装同版本的数据库

    2.  django 项目迁移

        1.  安装python

            -   `sudo apt install python3`

        2.  安裝相同版本的包

            -   导出当前模块数据包的信息

                -   `pip3 freeze > package_list.txt`

            -   导入到另一台新主机
                -   `pip3 install -r package_list.txt`

        3.  将当前项目源代码复制到远程主机上(scp命令)

            -   sudo scp -a 当前项目源代码远程主机地址和文件夹
                -   eg.`scp -a /home/zf/mywebsite root@12.23.43.53/root/mydjangoproject`

        4.  远程登录云主机（使用 ssh命令 远程登录）
            -   `ssh root@12.23.43.53`

### WSGI Django 工作环境部暑

-   WSGI （ Web Server Gateway Interface ） Web服务器网关接口,是 Python 应用程序或框架 和 Web服务器 之间的一种的接口。
-   他实现了 WSGI 协议、http 等 协议。Nginx 中 HttpUwsgiModule 的作用是与 uWSGI 服务器进行交换。WSGI 是一种 web服务器网关接口。

#### uWSGI 网关接口配置

-   使用 python3 manage.py runserver 通常只在开发和测试环境中使用
-   当开发结束后,完善的项目代码需要在一个高效稳定的环境中运行,这时可以使用 uWSGI
-   uWSGI 是 WSGI 的一种，它可以让 Django、Flask 等开发的web站点运行其中

#### 安装

-   `sudo pip3 install uwsgi`

#### 配置 uWSGI

-   添加配置文件：项目文件夹/uwsgi.ini

    -   eg. `mywebsite/uwsgi.ini`

        ```ini
            [uwsgi]
            # 套接字的方式 IP:端口号
            # socket=127.0.0.1:8000
            # Http通信方式 IP:端口号
            http=127.0.0.1:8000
            # 项目当前工作目录
            chdir=/home/zf/PycharmProjects/Django/website
            # 项目中 wsgi.py 文件的目录，相对于当前工作目录
            wsgi-file=website/wsgi.py
            # 进程个数
            process=4
            # 线程个数
            threads=2
            # 服务 pid记录文件
            pidfile=uwsgi.pid
            # 服务 日志文件位置
            daemonize=uwsgi.log
        ```

-   uWSGI 运行管理

    -   启动 uWSGI

        -   在项目根目录下运行
        -   `sudo uwsgi --ini website/uwsgi.ini`
        -   查看运行状态：`ps aux | grep uwsgi`

    -   停止 uWSGI

        -   在项目根目录下运行
        -   `sudo uwsgi --stop uwsgi.pid`(推荐)
        -   `sudo killall uwsgi`

    -   说明：
        -   当 uwsgi 启动后，当前 django项目 的程序已变成后台守护进程，在关闭当前终端时此进程也不会停止

### Nginx 反射代理配置

> Nginx 是轻量级的高性能web服努器,提供了诸如HTP代理和反向代理、负载均 存、缓存等一系列重要特性,在实践之中使用广泛

-   C 语言编写 执行效率高
-   Nginx 作用
    -   负载均衡，多台服务器轮流处理请求
    -   反射代理
-   原理
    -   客户端请求 Nginx， 再由 Nginx 请求 uwsgi ,运行 django 下的 python代码。
-   Ubuntu下安装：`sudo apt install nginx`

-   配置

    -   修改 `sudo vi /etc/nginx/sites-available/default`

        ```python
            # 在 server 节点下添加新的 location 项，指向 uwsgi 的 ip 与端口
            server{
                ...
                location / {
                    uwsgi_pass 127.0.0.1:8000; # 重定向到 127.0.0.1:8000
                    include /etc/nginx/uwsgi_params; # 将所有的参数转到 uwsgi下
                }
                ...
            }
        ```

-   启动 nginx

    -   `sudo /etc/init.d/nginx start`
    -   `sudo service nginx restart`

-   查看 nginx

    -   `ps aux | grep nginx`
    -   `sudo /etc/init.d/nginx status`
    -   `sudo service nginx status`

-   停止 nginx

    -   `sudo /etc/init.d/nginx stop`
    -   `sudo service nginx stop`

-   重启 nginx

    -   `sudo /etc/init.d/nginx restart`
    -   `sudo service nginx restart`

-   修改 uWSGI 配置
    -   修改 项目文件夹 /uwsgi.ini 下的 Http通信方式改为 socket 通信方式
        -   `socket=127.0.0.1:8000`
    -   uwsgi 关闭并重新启动
        -   `sudo uwsgi --stop uwsgi.pid`
        -   `sudo uwsgi --ini website/uwsgi.ini`

### nginx 配置静态文件路径

-   在 django 中 settings.py 中配置的 static 只在 `python manage.py runserver`时有效

    ```conf
        # file: /etc/nginx/sites-available/default
        # 新添 location /static 路由配置，重定向到指定的绝对路径
        server {
            ...
            location /static {
                # root static 文件夹所在的绝对路径
                root /home/zf/PycharmProjects/Django/website; # 必须以 ; 结尾
            }
            ...
        }

        # 重启 nginx 即可
    ```

### 404界面

-   在模板文件夹内添加 404.html 模版,当响应返回 HttpResponseNotFound 或 raise Http404 时将会被显示
-   404.html 仅在发布版中（即 settings.py 中 DEBUG=False）才起作用
-   当响应处理函数触发 Http404 异常时就会跳转到 404界面

```python
from django.http import Http404
def xxx():
    raise Http404
```

## 配置总结

```python
# settings.py

# 允许 post 提交，注释 中间件中的
# 'django.middleware.csrf.CsrfViewMiddleware',

# 设置 TEMPLATES 的 DIRS 值为
'DIRS': [os.path.join(BASE_DIR, 'templates')],

# 静态文件 配置
STATIC_URL='/static/'  # 网络路由
STATICFILES_DIRS = [os.path.join(BASE_DIR, 'static')]  # 文件路径

# 添加 应用
# 在 INSTALLED_APPS 添加应用名

####### 在其他模块中 导入当前项目的 settings 模块
from django.conf import settings
```

## AJAX

### jq 对 ajax 的支持

1.  $obj.load(url,data,callback);

    -   作用：载入远程的HTML 文件到指定的元素中
    -   参数
        -   $obj（显示响应内容的jq元素）:通过jq选择器获取到的页面元素
        -   url:请求地址
        -   data:发送到服务器端的参数数据 （可省）
            -   方式1：字符串传参
                -   `"key1=value1&key2=value2"`
                -   无参数 / 字符串传参 用 GET 发起请求
            -   方式2：js对象传参
                -   `{key1:'value1',key2:'value2'}`
                -   js对象传参 用 POST 发起请求
        -   callback:请求成功时的回调 （可省）

    ```html
        {% csrf_token %}
        <button id="btnLoad">加载百度</button>
        <div id="baidu"></div>
        <script>
            $(function () {
                $('#btnLoad').click(function () {

                    var params = {
                        uname: 'maria',
                        uage: 30,
                        csrfmiddlewaretoken: $("[name='csrfmiddlewaretoken']").val()
                    }

                    $('#baidu').load('/ajax/server/', params, function () {
                        window.setTimeout(() => {
                            $("#baidu").hide(3000);
                        }, 5000);
                    })
                })
            })
        </script>
    ```

2.  $.get(url,data,callback,type)

    -   作用: 通过get方式异步的向远程地址发送请求.(仅支持异步)
    -   type: 响应回来的数据的格式
        -   取值：json / html(默认) / text / script (js脚本执行处理)

        ```javascript
            $(function () {
                $('#btnGet').click(function () {
                    $.get('/ajax/get_server/', function (data) { // data 响应回来的数据
                        console.log(data)
                    }, 'json')
                })
            })
        ```

3.  $.post(url,data,callback,type)

4.  $.ajax()
    -   自定义所有 ajax 操作
    -   语法: $.ajax({ AJAX参数对象 })
    -   AJAX参数对象:
        1.  url : 字符串,表示异步请求的地址
        2.  type : 字符串,表示请求方式,'get'或'post'
        3.  data : 传递到服务器端的参数
            1.  字符串: "key=value&key=value"
            2.  JS对象: {key:"value",key:"value"}
        4.  async : 是否使用异步的方式发送请求
            -   true:使用异步（默认值）
            -   false:使用同步
        5.  dataType:字符串,表示响应回来的数据的格式
            1.  'html'
            2.  'text'
            3.  'xml'
            4.  'json'
            5.  'script'
            6.  'jsonp':跨域时使用
        6.  success:请求成功时的回调函数
            -   `success:function(data){ data表示的是请求成功后的响应数据 }`
        7.  error:请求失败时的回调函数
        8.  beforeSend:发送请求之前的回调函数
            -   返回true:则正常发送ajax请求
            -   返回false:则终止本次请求的发送

## 备忘

-   快速排序 并归排序
-   剑指 offer

-   产品、变现、后台架构

### 前后端分离

![Python_WebProject](http://localhost:9710/blog/Python/Python_WebProject.png)

-   前端:即客户端,负责渲染用户显示界面【如web的js动态渲染页面,安卓,ios,pc客户端等】
-   后端:即服务器端,负责接收http请求,处理数据
-   APl: Application Programming Interface是一些预先定义的函数,或指软件系统不同组成部分衔接的约定
-   前后端分离完整请求过程
    1.  前端通过http请求后端API
    2.  后端以json形式返回前端数据
    3.  前端生成用户显示界面【如html,ios, android】

-   优点
    1.  各司其职
        -   前端:视觉层面,兼容性,前端性能优化
        -   后端:并发,可用性,性能
    2.  解耦,前端和后端均易于扩展
    3.  后端灵活搭配各类前端·如安卓等
    4.  提高用户体验
    5.  前端+后端可完全并行开发,加快开发效率

-   分离弊端

    -   http 无状态问题 -> token
    -   跨域   -> CORS
    -   csrf -> token
    -   动静分离和前后端分离区别
        -   动静分离指的是css/js/img这类静态资源跟服务器拆开部署,典型方案-静态资源交由CDN厂商处理

-   dict / set 实现：（位置松散 -> 稀疏数组 空间占有率不太好）

    -   `dict_['A'] = 1` 根据 key A 进行一次hash计算，得到hash值
    -   计算数组中的位置（索引）
    -   当再存入一对 键值 时 `dict_['B'] = 2`,出现哈希碰撞（计算出的索引与之前的索引一样）,以当前位置进行二次hash（开放地址法），计算位置
    -   再存入一对 键值 时 `dict_['C'] = 3`,两次碰撞得到位置
    -   若直接删除 `dict_['A']` , 当需要查找 `dict_['C']`报错 KeyError
    -   python 采用伪删除的方案解决（删除值，位置仍然被占），实现了探测链，探测链 断一条，值找不到

    -   扩容：

        -   一旦空闲位置少于总量的 三分之一 进行扩容（重新排座）

### JWT (Json Web token)

#### 前提

1.  bate64:二进制转成可视字符的一种算法

    ```python
    import base64

    # b64 加密
    #     参数 bytes
    #     返回值 bytes
    bs_ = base64.b64encode(b'answerzf')
    # b64 解密
    #     特点：没安全性
    #     应用：可视字符表示二进制
    base64.b64decode(bs_) 
    ```

    -   原理
        
        1.  将字符串拆成每三个字符一组
        2.  计算每一个字符对应的 ASCII码 (二进制)
        3.  将8位的二进制码,按照毎6位一组重新分组,不足6位的在后面补0
        
        ![Python-Net_DeadLock](http://localhost:9710/blog/Python/Python_WebProject_Base64.png)

        ![Python-Net_DeadLock](http://localhost:9710/blog/Python/Python_WebProject_Base64min.png)

    -   注：
        -   弊端：编码后的字串一定比编码前多，传输量有些冗余
        -   编码后的长度被四整除
        -   `base64.urlsafe_b64decode()`
            -   标准的Base64并不适合直接放在URL里传输，因为URL编码器会把标准Base64中的“/”和“+”字符变为形如“%XX”的形式
            -   为解决此问题，可采用一种用于URL的改进Base64编码，它不在末尾填充'='号，并将标准Base64中的“+”和“/”分别改成了“-”和“_”，这样就免去了在URL编解码和数据库存储时所要作的转换()

2.  加密
    -   HS256 (HMAC-SHA256)

        -   SHA256 - 散列(hash)算法的一种
            -   哈希算法三大特点：不可逆、定长、雪崩（输入该一个字节输出就变）

            ```python
            # SHA256
            import hashlib

            s=hashlib.SHA256() # 创建 SHA256
            s.update(b'xxxx') # 添加 需要 hash 的内容，参数： bytes
            s.digest() # 获取最终结果 返回值：byte

            # HMAC-SHA256
            import hmac
            
            # 参数： 
            #     key: 加密的key bytes
            #     str: 需要加密的内容 bytes
            #     digestmod: hmac的算法，指定为SHA256
            h = hmac.new(key,str,digestmod='SHA256')
            h.hexdigest() # 获取最终结果 返回值：十六进制
            h.digest() # 获取最终结果 返回值：byte（二进制）
            ```

    -   RSA256 非对称加密
        -   两把钥匙：公钥、私钥
        -   加密：公钥加密，私钥解密
        -   签名：公钥验证，私钥签名

#### JWT 组成

-   header 元数据格式：`{'alg':'HS256','typ':'JWT'}`
    -   alg 算法 - 默认 HS256
    -   typ 表明该token的类别 - 默认 JWT
    -   传输前 序列化 json 字串，做 base64
-   payload `{'exp':xxx, 'iss':xxx..}`
    -   格式为字典，分为公共声明和私有声明
        1.  公共声明：JWT 提供了内置关键字用于描述常见的问题,此部分均为可选项,用户根据自己需求按需添加key,常见公共声明如下
            
            -   `'exp':xxx`
                -   Expiration Time token过期时间的时间戳
            -   `'aud': xxx`
                -   (Audience) Claim 指明token签发面向群体
            -   `'iss'：xxx`
                -   (Issuer) Claim 指明token的签发者
            -   `'iat': xxx`
                -   (Issued At) Claim 指明创建时间的时间戳
            -   `'nbf':xxx`
                -   Not Before Time 生效时间，如果当前时间在 nbf 时间之前，则 Token 不被接收
        
        2.  私有声明

            -   用户可根据业务需求，添加自定义的key

    -   公共声明和私有声明均在同一个字典中;转成json串并用base64加密

-   signature 签名
    -   根据 header 中的 agl 确定具体算法,以下用 HS256 为例
    -   将 bash64后的header 字符串 + '.' + base64 后的 payload字符串 和 自定义的key
    -   做 hmc256 签名，将签名在做base64

-   生成结果
    
    -   base64_header + '.' + base64_payload + '.' +base64_sign

**实现 JWT 协议**

```python
"""
    JWT
"""

import base64
import json
import hmac
import copy
import time

class JWT:

    @staticmethod
    def encode(payload, key, exp=300):
        """
            JWT 加密
        :param payload: 自定义 json
        :param key: 密钥  str / bytes
        :param exp: 超时失效时间 时间戳 int
        :return: token bytes
        """
        json_header = {
            'alg': 'HS256',
            'typ': 'JWT'
        }
        # separators 第一个参数：json串中 键值对以 xx 连接符相连
        #           第二个参数：键值 以 xx 连接符相连
        #          作用是去掉'，' '：'后面的空格，在传输数据的过程中，越精简越好，冗余的东西全部去掉。
        # sort_keys 作用：告诉编码器按照字典key排序(a到z)输出
        str_json_header = json.dumps(
            json_header, separators=(',', ':'), sort_keys=True)  # json 串 紧凑有序
        byte_base64_header = JWT.b64encode(str_json_header.encode())

        # payload
        json_payload = copy.deepcopy(payload)
        json_payload['exp'] = int(time.time() + exp)
        str_json_payload = json.dumps(
            json_payload, separators=(',', ':'), sort_keys=True)
        byte_base64_payload = JWT.b64encode(
            str_json_payload.encode())

        # sign
        b64_ = byte_base64_header + b'.' + byte_base64_payload

        if isinstance(key, str):
            key = key.encode()
        byte_hmac = hmac.new(key, b64_, digestmod='SHA256').digest()
        byte_base64_sign = JWT.b64encode(byte_hmac)

        return byte_base64_header + b'.' + byte_base64_payload + b'.' + byte_base64_sign

    @staticmethod
    def decode(token, key):
        """
            校验 token
        :param token: 加密后的 token bytes
        :param key: 密钥 str / bytes
        :return: 成功 payload json ,失败 抛出异常
        """

        header_bs, payload_bs, sign = token.split(b'.')

        if isinstance(key, str):
            key = key.encode()

        byte_hmac = hmac.new(key, header_bs + b'.' + payload_bs, digestmod='SHA256').digest()
        now_sign = JWT.b64encode(byte_hmac)

        if sign != now_sign:
            raise JWTError('Token is invalid')

        json_payload = json.loads(JWT.b64decode(payload_bs).decode())
        # 过期时间戳
        exp = json_payload['exp']
        now_time = time.time()
        if now_time > exp:
            raise JWTError('Token is expired')

        return json_payload

    @staticmethod
    def b64encode(byte_):
        # 替换原生 base64 '=' -> ''
        return base64.urlsafe_b64encode(byte_).replace(b'=', b'')

    @staticmethod
    def b64decode(byte_):
        # 还原 base64 '=',并解码
        rem = len(byte_) % 4
        byte_ += b'=' * (4 - rem)
        return base64.urlsafe_b64decode(byte_)

class JWTError(Exception):
    """
        自定义异常
    """
    def __init__(self, error_msg):
        self.error = error_msg

    def __str__(self):
        return '<JWTError error %s>' % self.error

if __name__ == '__main__':
    JWT_byte = JWT.encode({'abc': '124', 'def': 'eee'}, 'answer')
    print(JWT.decode(JWT_byte, 'answer'))
```

#### pyjwt

-   安装：`pip3 install pyjwt`

```python
import jwt

# payload: 公有声明+私有声明 json
# key: 自定义加密 key str
# algorithm: 需要使用的加密算法 str
# token: bytes
jwt.encode(payload,key,algorithm) # 返回值：token bytes
jwt.decode(token,key,algorithm) 
    # 如果 payload 中添加了 `iss`,可针对该字段校验, 添加参数 `'issuer'='发布者'` str   异常 jwt.InvalidissuerError# 
    # 如果 payload 中添加了 `aud`,可针对该字段校验, 添加参数 `'Audience'='xxx'` str   异常 jwt.InvalidAudienceError
```

_Ps:若 encode得时候 payload中添加了exp 字段则 exp字段得值需为 当前时间戳 + 此 token 得有效期时间,例如希望 token300秒后过期 {'exp':time.time()+300} 在执行 decode时,若检查到exp字段,且 token过期,则抛出 jwt.ExpiredSignatureError_

### CORS

> Cross-Origin Resource Sharing(跨域资源共享)

-   同源（协议 - 域名 - 端口）

1.  CORS 允许浏览器向跨源（协议 + 域名 + 端口）服务器，发出 XMLHttpRequest 请求,从而克服了AJAX只能同源使用的限制
2.  特点:
    1.  浏览器自动完成(在请求头中加入 特殊头 或 发送特殊请求)
    2.  服务器需要支持(响应头中需要有特殊头)
3.  简单请求(Simple requests) 和 预检请求(Preflighted requests)
    1.  **满足以下全部条件的请求为简单请求**
        1.  请求方法：GET or HEAD or POST
        2.  请求头仅包含：Accept  Accept-Language Content-Language Content-Type
        3.  Content-Type 仅支持三种
            1.  application/x-www-form-urlencoded
            2.  multipart/form-date
            3.  text/plain

    2.  **不满足以上任意一点的请求都是预检请求**

4.  简单请求发送流程

    1.  请求
        -   请求头中携带 Origin ,该字段表明自己来自哪个域
    2.  响应
        -   如果请求头中的 Origin 在服务器接受范围内,则返回如下头

            ```http
                Access-Control-Allow-Origin
                    # 服务器接受的域
                Access-Control-Allow-Credentials（可选）
                    # 是否接受Cooike
                Access-Control-Expose-Headers（可选）# 拓展 头
                    # 默认情况下，xhr只能拿到如下响应头：
                    # Cache-Control
                    # Content-Language
                    # Content-Type
                    # Expires
                    # Last-Modified 
                    # 如有需要获取其他投，需在此指定
            ```

        -   如果服务器不接受此域,则响应头中不包含 Access-Control-Allow-Origin

5.  预检请求发送流程

    1.  以 OPTION 方式 发起请求，携带如下请求头

        ```http
            Origin （必选）
                # 表明 此请求来自哪个域
            Access-Control-Request-Method （必选）
                # 此次请求使用的方法
            Access-Control-Request-Headers （必选）
                # 此次请求使用的头
        ```

    2.  OPTION 接受响应阶段,携带如下响应头

        ```http
            Access-Control-Allow-Origin （必选）
                # 当前服务器接受的域
            Access-Control-Allow-Methods （必选）
                # 告诉浏览器,服务器接受得跨域请求方法
            Access-Control-Expose-Headers （必选）
                # 返回所有支持的头部,当 request 有 Access-Control-Request-Headers ,该响应头必然回复
            Access-Control-Allow-Credentials（可选）
                # 是否接受Cooike
            Access-Control-Max-Age（可选）
                # OPTION请求缓存时间，单位s
        ```

    3.  主请求阶段

       ```http
            Origin
                # 表明 此请求来自哪个域
        ```

    4.  主请求响应阶段

       ```http
            Access-Control-Allow-Origin
                # 当前服务器接受的域
        ```

6.  Django 支持

    django-cors-headers

    -   配置流程

        ```python
        # settings.py

        # 1. ALLOWED_HOSTS = ['*']

        # 2. 禁用 # 'django.middleware.csrf.CsrfViewMiddleware',

        # 3. 数据库 
            # 创建数据库 `create database blog_server default charset utf8;`
            # 配置 数据库
            # 添加 pymysql 支持 （主模块 __init__ 下）

        # 4. 图片上传路径配置
        
            # 前端请求图片资源的路径
            # MEDIA_URL = "/media/"
            # 后端服务器 图片存储的 目录
            # MEDIA_ROOT = os.path.join(BASE_DIR, "media/") 
        
            # dir_projectname/urls.py

                # 添加图片的 路由映射  http://127.0.0.0:8000/media/xxx.jpg
                # from django.conf.urls import include, url
                # from django.conf.urls.static import static
                #  urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
            # django 的 PUT 请求不支持 multipart/form-data 即文件上传
        
        # 5. 其他
            # LANGUAGE_CODE = 'zh-hans'
            # TIME_ZONE = 'Asia/Shanghai'
        
        # 6. cors 配置
            # 1. INSTALLED_APPS 添加 corsheaders

            # 2. MIDDLEWARE 中 添加 `'corsheaders.middleware.CorsMiddleware',`
                # 尽量靠前 必须在 `'django.middleware.common.CommonMiddleware',` 前

            # 3. 访问地址权限配置
                # CORS_ORIGIN_ALLOW_ALL = True 白名单不启用
                # CORS_ORIGIN_WHITELIST = [
                #     "https://xxx.xxx"
                # ] # 白名单配置

            # 4. 允许的请求方式
                # CORS_ALLOW_METHODS = (
                #     'DELETE',
                #     'GET',
                #     'OPTIONS',
                #     'PATCH',  # http 语义 局部跟新
                #     'POST',  # http 语义 全量跟新
                #     'PUT',
                # )

            # 5. 允许的 头部信息
                # CORS_ALLOW_HEADERS = (
                #     'accept-encoding',
                #     'authorization',  # 传递 token
                #     'content-type',
                #     'dnt',  # don't catch me
                #     'origin',
                #     'user-agent',
                #     'x-csrftoken',
                #     'x-requested-with',  # 辨别是否时 ajax 请求
                # )

            # CORS_PREFLIGHT_MAX_AGE  # 默认 86400s 一天

            # CORS_EXPOSE_HEADERS  # 默认 []

            # CORS_ALLOW_CREDENTIALS  # bool 默认 False

        # 6. APPEND_SLASH = False 
            # 关掉 url 自动补 / 
        ```

### RESTful - Representational State Transfer

1.  资源（Resource）

    -   网络上的一个实体,或者说是网络上的一个具体信息,并且每个资源都有一个 独一无二的URI 与之对应
    -   获取资源直接访问 URI 即可

2.  表现层（Representation）

    -   如何去表现资源
        -   即资源得表现形式 如HTML,xml,jPG,json等

3.  状态转化(State Transfer)

    -   访问一个 URI 即发生了一次客户端和服务端得交互;此次交互将会涉及到数据和状态得变化
    -   客户端需要通过某些方式触发具体得变化 Http method 如 GET , POST , PUT , PATCH , DELETE ...

4.  RESTful 的特征
    1.  每一个URI代表一种资源
    2.  客户端和服务器端之前传递着资源的某种表现json
    3.  客户端通过HTTP的几个动作对资源进行操作-发生状态转化

#### 设计 符合 RESTful 特征的API

1.  协议 http/https

2.  域名

    -   域名中体现 api 字样: https://api.xxx.com or https://xx.xxx.xx/api/

3.  版本

    -   https://api.xxx.xx/v1/

4.  路径

    -   路径中避免使用动词,资源用名词表示,案例如下

5.  HTTP 动词语义

    -   GET   (SELECT):从服务器取出资源(一项或多项)
    -   POST  (CREATE):在服务器新建一个资源
    -   PUT   (UPDATE):在服务器更新资源(客户端提供改变后的完整资源)
    -   PATCH (UPDATE):在服务器更新资源(客户端提供改变的属性)
    -   DELETE(DELETE):从服务器删除资源

6.  灵活运用查询字符串

7.  状态码

    1.  用HTTP响应码表达此次请求结果,例如

        ```http
        1.  200 OK - [GET] : 服务器成功返回用户请求的数据
        2.  201 CREATED - [POST/PUT/PATCH] : 用户新建或修改数据成功
        3.  202 Accepted - [*] : 表示一个请求已经进入后台排队(异步任务)
        4.  204 NO CONTENT - [DELETE] : 用户删除数据成功。
        5.  400 INVALID REQUEST - [PosT/PUT/PATCH] : 用户发出的请求有错误,服务器没有进行新建或修改数据的操作,该操作是幂等的。
        6.  401 Unauthorized - [*] : 表示用户没有权限(令牌、用户名、密码错误)
        7.  403 Forbidden - [*] : 表示用户得到授权(与401错误相对), 但是访问是被禁止的
        8.  404 NOT FOUND - [*] : 用户发出的请求针对的是不存在的记录, 服务器没有进行操作,该操作是幂等的。
        9.  406 Not Acceptable - [GET] : 用户请求的格式不可得(比如用户请求json格式,但是只有XML格式)。
        10. 410 Gone - [GET] : 用户请求的资源被永久删除,且不会再得到的
        11. 422 Unprocesable entity - [POST/PUT/PATCH] 当创建一个对象时,发生一个验证错误
        12. 500 INTERNAL SERVER ERROR - [*] : 服务器发生错误
        ```

    2.  自定义内部 code 进行响应

        -   返回结构如下 `{'code':200,'data':{},'error':xxx}`
    
8.  根据HTTP动作的不同,返回结果的结构也有所不同

    ```http
    1.  GET /users : 返回资源对象的列表(数组)
    2.  GET /users/xxx : 返回单个资源对象
    3.  POST /users : 返回新生成的资源对象
    4.  PUT /users/xxx : 返回完整的资源对象
    5.  PATCH /users/xxx : 返回完整的资源对象
    6.  DELETE /users/xxx : 逡回一个空文档
    ```

### Flask

```python
from flask import Flask, send_file

# 初始化 Flask
app = Flask(__name__)

# 绑定路由 / 执行函数
@app.route('/index')
def index():
    # 返回具体页面
    # Flask 默认 静态文件目录【css ,js ,img】 在 static
    # Flask 默认 模板目录 在 templates
    return send_file('templates/index.html')

@app.route('/login')
def login():
    return send_file('templates/login.html')

@app.route('/register')
def register():
    return send_file('templates/register.html')

@app.route('/<username>/info')
def info(username):
    # 个人信息
    return send_file('templates/about.html')

@app.route('/<username>/change_info')
def change_info(username):
    # 修改个人信息
    return send_file('templates/change_info.html')

@app.route('/<username>/topic/release')
def topic_release(username):
    # 发表 blog
    return send_file('templates/release.html')

@app.route('/<username>/topics')
def topic_release(username):
    # 个人 blog 列表
    return send_file('templates/list.html')

@app.route('/<username>/topics/detail/<t_id>')
def topic_detail(username, t_id):
    # blog 内容详情
    return send_file('templates/detail.html')

if __name__ == '__main__':
    # 启动 Flask 服务
    # 默认监听 5000 端口
    # python flask_client.py 启动服务
    app.run(debug=True)

```