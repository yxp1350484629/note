# day084(tornado)

1.cookies里的内容永远由服务器决定，浏览器只是存储和发送，tornado中的set_cookie方法本质上是设置响应头中的Set_cookie这个内容，设置这个浏览器才能知道要设置的cookie的名字，值，u                                                                                                                                                                                                                                                                                                                                                                                           rl等                   

tornado中的get_cookie是获取浏览器发送请求携带过来的cookie，一般有两个参数，第一个是cookie名，第二个是                       默认值，这个默认值可以在未取到cookie得到这个值

2.tornado可以在服务端调用方法来进行cookie的删除，注意所有在客户端进行的cookie删除，都不是真的删除了浏览器中的cookie，浏览器中的cookie只能是自己删除，我们只是设置了cookie的值为空和有效期失效，这样就不会携带cookie过来 clear_cookie删除某个特定url的某个名字的cookie，clear_all_cookie删除某个url下所有cookie

3.tornado提供了内置的安全cookie的设置方法，必须先在设置文件setting中添加cookie_secret，这个一般自己用base64等算法生成一个key，然后用tornado的set_secure_cookie，后面添加正常参数值，便可以设置安全cookie了，得到浏览器传过来的cookie也需要用get_secure_cookie来获得值      

4. tornado异步原理（回调函数)

   ```python
   import threading
   def reqA():
       longIo(finish)
   def reqB():
       print('其他操作')
   def finish(response):
       print('当longIO处理完延迟操作得到一个response就要执行这个')
       print('这里写得到响应后，用响应组织的程序')
   def longIO(fuc):
       def run():
           print('处理所有的延迟操作,并得到response')
           response=???
           fuc(response)
       t=threading.Thread(target=run,).start()
   def main():
       reqA()
       reqB()
   
   ```

5.tornado异步原理(协程)

```python
@gengoruntine
def reqA():
    res=yield longIo()
    print('响应就是res，根据响应值进行返回')
def reqB():
    print('其他操作')
def longIo():
    print('进行所有的延迟操作，并得到一个响应response')
    response=???
    yield response
def gengoruntine(func):
    def wrapper(*args,**kwargs):
        gen1=func()
        gen2=next(gen1)
        def run():
            res=next(gen2)
            print('在这里也可以对res进行处理，和回调函数中的处理类似')
            try:
                gen1.send(res)
            except Exception:
                pass
        threading.Thread(target=run,).start()
    return wrapper
def main():
    reqA() 
    直接执行的是wrapper
    reqB()
```

