# flask_learning
Flask本身相当于一个内核，其他几乎所有的功能都要用到扩展。

WSGI工具箱采用Werkzeug(路由模块)、模板引擎使用Jinja2，这两个是Flask的核心

## 常用扩展包

扩展列表：http://flask.pocoo.org/extensions/

+ Flask-SQLalchemy： 操作数据库；
+ Flask-script：插入脚本；
+ Flask-migrate：管理迁移数据库；
+ Flask-Session：Session存储方式指定；
+ Flask-WTF：表单；
+ Flask-Bable：提供国际化和本地化支持，翻译；
+ Flask-Login：认证用户状态；
+ Flask-OpenID：认证
+ Flask-RESTful：开发REST API的工具；
+ Flask-Bootstrap：继承前端Twitter Bootstrap框架；
+ Flask-Moment：本地化日期和时间；
+ Flask-Admin：简单而可扩展的管理接口的框架


## 创建hello_world.py文件
```python
from flask import Flask
app = Flask(__name__)

@app.route('/')  # 装饰器的作用是将路由映射到视图函数index
def index():
    return 'Hello World'

# Flask应用程序实例的run方法启动WEB服务器
if __name__ == '__main__':
    app.run()
```
## Flask对象初始化参数

+ import_name

。Flask程序所在的包（模块），传__name__就可以，意思就是将程序所在的目录作为项目目录，可以决定Flask在访问静态文件时查找的路径
+ static_url_path

。静态文件访问路径，可以不传，默认为：/+static_folder
+ static_folder

。静态文件存储的文件夹，可以不传，默认为static
+ template_folder

。模板文件存储的文件夹，可以不传，默认为template

## **配置信息集中管理**

Flask将配置信息保存到了app.config属性中，该属性可以按照字典类型进行操作。

1、读取

+ app.config.get(name)
+ app.config[name]

2、设置

+ 从配置对象中加载

app.config.from_object(配置对象)

```python
from flask import Flask
class DefaultConfig(object):
    """默认配置"""
    SECRET_KEY = 'sss'

app = Flask(__name__)
app.config.from_object(DefaultConfig)

@app.route('/')
def index():
    print(app.config['SECRET_KEY'])
    return "hello world"
```
+ 从配置文件中加载

app.config.from_pyfile(配置文件)

新建一个配置文件setting.py
```python
SECRET_KEY = 'sss'
```
在Flask程序文件中

```python
from flask import Flask
app = Flask(__name__)
app.config.from_pyfile('setting.py')

@app.route('/')
def index():
    print(app.config['SECRET_KEY'])
    return "hello world"
```
+ 从环境变量中加载

Flask使用环境变量加载配置的本质是通过环境变量值找到配置文件（环境变量的值为配置文件的绝对路径），在读取配置文件的信息，其使用方式为

app.config.from_envvar(环境变量名)

现在终端中执行如下命令；
```
export PROJECT_SETTING='~/setting.py'
```
再运行如下代码：
```python
from flask import Flask
app = Flask(__name__)
app.config.from_envvar('PROJECT_SETTING', silent=True)
# silent参数说明：默认为False，表示没找到变量会直接报错
# 如果为True，没找到则会自动返回None
@app.route('/')
def index():
    print(app.config['SECRET_KEY'])
    return 'Hello world'
```
