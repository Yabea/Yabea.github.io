# 关于Flask开发
title: 关于Flask开发
tags: Python
date: 2018-01-13
toc: true
---
> 工作以来，做的最多的工作就是Flask开发，终于可以“理直气壮”的写篇关于Flask开发的博客了。本篇博客将以开发一个简单的在线图书馆系统为例，讲述Flask项目开发，当然也会穿插讲述一些JS用法。因此，本文可能相对篇幅较长。

<!--more-->
[第一节 开发初探](#first)
[第二节 创建项目目录](#second)
[第三节 使用数据库](#third)

开发环境： ubuntu16.04 + Python2.7 + Flask + Mysql
<span id = "first">
## 开发初探
或许你是一个Pythoner,或许你擅长别的语言。而我们的目标是搞懂Flask开发。开发讲究的是进阶，由简到繁，那么如何使用Flask开发完成一个最简单的应用呢？
创建app.py，编写：
```
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello Flask!'

if __name__ == '__main__':
    app.run()
```
使用命令python app.py，运行该脚本，便可实现了第一个小应用的编写。
</span>

<span id = "second">
## 创建项目目录
对于Web开发框架，很重要的一点就是创建项目目录，接下来我们以创建一个在线图书馆为例进行介绍。创建的目录为:

 -  library 
    - library
        - templates （MVC中的V，存放html文件）
        - static （存放静态文件，包括css和js文件等）
        - __init__.py （模块初始化文件）
        - models.py(MVC中的M，映射数据库中表）
        - views.py （MVC中的C，存放视图函数）
        - config.py （配置文件）
    - manage.py  （数据库迁移文件）
    - serve.py （项目启动文件）
    
创建了项目目录之后，我们可以很清晰的看到整体的项目结构，可以看到，Flask也是按照MVC结构的，那么本节就将讲述如何使用MVC结构。

### 路由
> 首先，需要介绍一下在Flask中很重要的一个概念：**路由**。所谓路由，就是处理URL和函数之间关系的程序，Flask中也是对URL规则进行统一管理的，使用@app.route修饰器将一个函数注册为路由。

### 蓝图
> 编程讲究的是功能模块化，从而使代码看起来更加的优雅和顺畅。在开发在线图书馆功能时，有两种角色，一个是普通用户user，另一个是管理员admin，那么他们所拥有的权限和功能有很大差异，若将其放在同一个文件下，代码量相对较大，若进行版本控制时，也很容易出现冲突。
在Flask中，**蓝图**可以将各个应用组织成不同的组件，实现代码的模块化，这个具体怎么实现呢？

首先，在views.py中创建蓝图：
```
# coding:utf-8

views = Blueprint('views', __name__)

```

然后，在__init__.py初始化应用，并添加views蓝图：
```
#coding:utf-8

from flask import Flask
from views import views

def create_app(config='dataanalysis.config.Config'):
    app = Flask(__name__)
    app.register_blueprint(views)

    return app
```

这样views蓝图被定义，使用views蓝图，打开views.py，编写视图函数定义路由：

```
@views.route('/')
def index():
    return "Hello Flask"
```

编写serve.py：

```
from library import create_app

app = create_app()

if __name__=='__main__':
    app.run(debug=True, threaded=True, host="0.0.0.0", port=9008)
```

使用"python serve.py"命令运行该项目，便可在网页上显示"Hello Flask"。

那么如何使用模板呢？
在views中，重新编写：
```
# coding:utf-8

from flask import render_template, Blueprint

views = Blueprint('views', __name__)

@views.route('/')
def index():
    return render_template('home.html')
```

可以看到这里，使用render_template()引入了前端文件'home.html'，运行"python serve.py"便可在网页中显示home.html中的内容。
</span>

<span id = "third">
## 使用数据库
在Python中，一个最有名的的ORM框架就是SQLAlchemy，在Flask中，可以使用Flask-SQLAlchemy管理数据库，使用命令安装：
```
pip install flask-sqlalchemy
```


接下来使用MySQL数据库的使用为例。
### 配置
首先，需要在config.py中配置数据库：

```
#coding:utf-8

import os
basedir = os.path.abspath(os.path.dirname(__file__))

DEBUG = True
SQLALCHEMY_TRACK_MODIFICATIONS = False

#mysql数据库连接信息
SQLALCHEMY_DATABASE_URI = os.environ.get('DATABASE_URL')
SQLALCHEMY_TRACK_MODIFICATIONS = True

BOOTSTRAP_SERVE_LOCAL = True
```

接下来，使用命令配置环境，这里以临时环境变量为例，创建library数据库:

```
export DATABASE_URL="mysql://username:password@localhost/CTFD"
```

接下来，在models.py中创建数据库映射表，上面提到过，本篇文章以实现一个在线图书馆为例，那就首先创建一个User表吧，假定包括用户名，密码，邮箱三个字段：

```
class Users(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(128), unique=True)
    email = db.Column(db.String(128))
```

这样，我们就完成了Users表的映射，那么接下来，我们就可以使用数据库，完成在线图书馆的第一个功能-注册。
### 注册功能的实现
注册功能简单说就是将用户的信息填写的数据进行验证，并存入数据库。
#### 创建视图函数
这里我们通过编写对应的路由函数来实现，首先，这是用户相关的功能，为了体现模块化思想，我们创建user.py文件，用来编写用户相关的所有功能，并创建user蓝图，然后创建视图函数：
```

```


</span>
