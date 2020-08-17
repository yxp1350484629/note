# day083(tornado)

 

1.ioloop是tornado核心io循环模块，封装了linux下的epoll，io多路复用，是tornado处理io高效的基础。

2.Application是tornado的核心应用类，是与服务器对应的接口

3.app.listen和创建一个HttpServer，然后listen的区别在于，app.listen就已经做好了Httpserver两步做的事情，创建了服务器并且监听了端口，app.listen是能是单进程的服务器，而HttpServer后面可以接start(n)开启n个进程

4.最终开启监听，都是IOLoop.current().start()，同时它开启了IO循环，开启了IO多路复用的epoll进行监控服务器的socket，有连接进来就创建单独客户端的socket，并且仍然由epoll监控，这种IO多路复用的方式是后面实行异步编程的基础。

5.为啥不使用tornado提供的多进程，而是自己一个一个开启服务呢

 所有进程都复制使用父进程的ioloop,如果在创建子进程之前父进程ioloop修改，子进程都会收到影响

所有进程都由一个命令在执行，如果我们要修改代码，那么所有进程都将关闭

我们用多进程的话，肯定需要变listen为bind，这样我们多个进程都监听同一个端口，操作系统epoll不好分别监控，影响效率，可以一个进程一个端口

6.由于tornado自带的配置文件方式，不能导入参数为字典，所以我们直接使用自定义的config,py，想要什么直接写在里面，然后Import

7.如果在url映射的时候，就要进行参数的传递，传递多个就是通过字典的方式进行传递，传递到对应类中的initialize，这个方法会在进行映射的第一步就执行

8.tornado在组织响应内容要为json的时候，其实只需要组织一个字典，然后write就会自动序列化为json串，并且响应头中的content-type为application/json，自己手动序列化用dumps组织json，反而content-type为text/html，还要加头，不太方便

9.如果要设置响应的响应头，我们一般可以在映射类的set_default_headers方法里重写，一般就是用self.set_header来设置响应头参数和内容，这个方法也在get,post等前面就会调用生成默认的响应头，我们也可以在get,post方法里set_header来覆盖

10. set_status一般用于在get,post等里面设置响应的状态码，如果状态码为正常的，就是像404等，tornado会自动加上默认注释，如果设置888等不平常的状态码，就需要自己在后面添加解释，set_status有两个参数

11. Application得到所有的url映射关系生成app，在映射的时候可以通过url函数来为这个url添加名字，和正常的url映射格式一样，但是可以在后面加name=' '，这样在任何一个映射的类中，都可以通过reverse_url(名字)来找到某个app中的url，这就是反向解析url

12. get方法中若想获取url后面的传参，可以使用get_query_argument ，三个参数，name为要取的参数名，default为默认值，没有的话就得到这个，strip设置为True或者False来是否去掉name两边的空格

    也可以使用get_query_arguments这个无论参数是否有，有多少个，都可以获取[]结构的参数值，没有就是空列表，健壮性好一些

    post取参是get_body_argument和get_body_arguments 同上

    get_argument和get_arguments是get post都能获取,一般不用这个，难以区分

13. self.request对象和djnago中一样，属性也是请求的所有相关数据

14. request.files是专门针对上传文件的，是字典格式，里面的值为列表中的字典，每个字典都是一个文件对象

15. self.write是先写入到发送缓冲区里面 self.finish() 强制刷新缓冲区并且结束请求通道

16. self.send_error会抛出给的异常码，然后tornado会自动调用write_error，接收错误码，并写代码返回错误页面，write_error结束整个请求流程结束

 17.prepare和on_finish是在请求处理函数之前和之后调用的方法，prepate相当于DJango中的中间件，但这是在请求过来进行url映射后，处理函数之前进行调用的，而on_finish是在处理函数结束后，进行调用的，一般用来释放资源或者处理日志，主要on_finishi里面就不要有self.send_error了，因为这是后台处理自己的事情，和客户端没有任何关系

18.正常请求进来接口的调用顺序，set_defalut_header肯定是第一的，因为只要请求过来必须要给回应，无论什么情况都会有响应，第一步就先设置正常的响应头，然后就是请求参数初始化方法，其次是中间件方法，然后才是后面的

19.在有错误抛出，也就是prepare或者处理方法中有send_error的时候，会立即再执行set_defalut_header这是设置错误时的响应头更换正确时候的响应头，然后再是write_error直接返回响应，然后是on_finish

20.static_url函数，一般用在模板css等链接里面，作用可以将配置里面的静态路径和括号里面参数拼接，还有一个更重要的作用是为要访问的静态文件生成一个hash值，作为查询字符串接在url的后面，只要这个静态文件不改变，那么访问过来的url里面hash值就不改变，改变hash值就会改变请求，也改变了对应的缓存。这个适用于开发和线上都行，只要不改变css内容,hash值不变，缓存不变

21.tornado自动开启了模板的转义。怕的是传过来的数据会是恶意数据，我们可以在模板中设置一行关闭转义，或是整个模板都关闭转义，也可以在设置文件中setting字典中设置'autoescape':None,关闭整个项目自动转义，但是一般不用用整体关闭 ，本身就是为了安全性

也可以在关闭整个模板转移后，在模板中使用escape函数，后面接传过来变量，来开启转义

22.很多时候用户想要直接访问静态文件，肯定是比如前面的网址+/index.html，不会知道静态文件在服务器具体哪个文件上，我们就可以使用StaticFileHandler，这个一般放在所有url映射的最下面，因为是顺序匹配，所有都匹配不到就走静态文件匹配看看，所有这个url匹配就可以这样写(r'/(.*)$',tornado.web.StaticFileHandler，{'path':os.path.join(config.BASE_DIRS，具体路径)),这个tornado.web.StaticFileHandler是系统自带，可以接受路径已经匹配到的url参数，看是否一致，注意 (r'/(.*)$',tornado.web.StaticFileHandler这个还可以匹配/，一般这个就是返回的静态文件，就算是百度的www.baidu.com/也是返回的静态



