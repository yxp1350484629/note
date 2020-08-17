# day079(django)

1.路由系统

127.0.0.1:8000/path  这个path就是路由，一个百度的path有n多种

而django做好了如何解析处理这些路由

2.视图

视图是专门处理这些路由的，根据后面的path不同，有不同的处理方式

3.一定要关注django的版本号，问问题最好去django的官网，是知识的源头，必须要在你用的版本号下问问题

而且在网上找的代码，如果不能跑，很大原因可能是版本号问题

4.

```python
urlpatterns = [
    url(r'^admin/', admin.site.urls),
]
这就是url文件默认的变量，我们要做的就是接在后面写
url文件主要就是处理url，所有的动态路径必须先走该文件进行匹配
这个url方法，就是通过正则表达式来匹配，匹配到了，就执行后面的方法
```

5.

```python
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
这个是setting.py中的协调文件路径的变量，最里面的os.path.abspath(__file__)是返回这个setting.py的绝对路径，然后上面一层.dirname是返回当然路径所在文件夹，然后又一层文件夹，也就是返回到了整个项目那个文件夹路径
如果我们以后要放一些图片什么到一些文件夹下，比如在项目文件夹下放imgs文件夹，就可以BASE_DIR+'/imgs'到达那个路径
```

6.DEBUG在开发测试的时候一定要设置为True,因为它会在浏览器中给我们提示错误信息，并且告诉我们应该怎么做，但是在项目做完上线后一定要把它关闭

而且设置为True，只要代码一改变，就会立即重启项目，加上pycharm是自动保存，只要一动，就会重启项目

7.

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
这是配置数据库环境，这个DATABASES可以同时配置多个数据库，因为是字典形式
我们刚创建这个项目，是没有db.sqlite3这个数据库文件的，开始执行这个项目才会出现，因为执行项目会先把setting执行一遍，而且在这里用到了BASE_DIR
```

8.LANGUAGE_CODE = 'en-us'，我们可以设置为中文，LANGUAGE_CODE = 'zh-Hans'，这个虽然不能改变全部，但是可以让浏览器中显示的错误和改错的方案变为中文

9.TIME_ZONE = 'UTC' 这个是把时间设置为了格林日志时间，我们可以+8，或者调整为中国时间TIME_ZONE = 'Asia/Shanghai'

10.setting.py可以添加任何的变量，只要自己配置好，可以在任何py文件通过 from django.conf import settings` 导入和使用

11.在django中的所有print都在终端显示，因为执行是在终端