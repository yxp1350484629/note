# day052(IO)

1.非阻塞IO通常使用设置属性setblocking settimeout来操作，一般在流式套接字中用的多。

由于这个给socket套接字设置了属性，所以服务端的套接字在accept操作的 时候，如果没有连接，就会抛出错误，终究非阻塞IO是为效率服务，所以一般与while 等循环组合判断是否连接，如果还没有链接，就执行不与客户端套接字关联的其他操作，直到连接，才执行与客户端套接字关联的操作。用try except else来操作这种情况，事半功倍  setblocking与settimeout区别是，settimeout会等待后面参数的秒数后再判断是否抛出异常

```python3
from socket import *
from time import sleep,ctime
a=socket()
a.settimeout(5)
a.bind(('0.0.0.0',8888))
a.listen(3)
while True:
    try:
        b,addr=a.accept()
    except Exception as err:        没有链接就执行其他操作
        print(err)
        sleep(2)
        print(ctime())
    else:
        while True:
            data=b.recv(1024)
            print(data)
```

2.不管哪一门语言的IO多路复用，都是三步走

1.将想要监测的IO设置为关注IO

2.用相关方法将这些关注IO对象提交给操作系统内核

3.操作系统将已经准备就绪的IO对象返回给应用端，然后对IO对象进行操作

2，3部分一般是一个循环的模式

3.IO操作是能与内存，磁盘进行数据交互的操作，而IO对象是能进行IO操作的对象

select模块里的select函数里面的三个列表参数rlist,wlist,xlist储存的都是IO对象，rlist里面储存的是要进行阻塞IO操作的IO对象，比如能进行accept,recv的对象，socket套接字和客户端套接字。wlist列表里储存的是要进行主动操作的IO对象，比如send的客户端套接字。xlist的里面存储的是可能会发生错误的IO对象。

把对象储存在这三个列表中起到的都是监控作用，而select函数起到了提交的作用，返回的也是三个列表，一般用列表名rs,ws,xs来表示，是返回的**各自列表参数中准备就绪的IO对象**。这一个函数把上面三步走操作都完成了。值得一提的是同一个IO对象可以进行主动，被动，可能发生错误的操作，所以可以一个IO同时存储在在这三个列表参数中

4.个人觉得，比如accept,recv这些阻塞IO，有两个功能，一是监控功能，监控IO事件请求。当客户端的请求发送完毕，也就是IO操作准备就绪，就实现第二个功能，accept建立连接，recv接收数据

而select也是一个阻塞IO，相当于给所有给定的IO对象中的这些IO事件监控，而不是像上面那样的单点监控，select将IO事件准备就绪的IO对象返回，然后需要自己再去根据IO对象进行连接。

单点的accept,recv需要先执行，客户端才能发送，然后服务端识别执行，多点的是select先执行，实现对所有IO对象的事件监控，然后返回准备就绪IO事件的IO对象。阻塞函数先执行，然后进行监控，满足条件，执行相应功能。如同accept功能是建立连接，input是输入到应用层。

select和accept等的同时执行，表面上看有两层监控，实际上应该底层实现只有select一层，然后传到accept，就不再阻塞，只需要直接执行功能了。

5.个人觉得，select多路复用的比较关键的点可能在于在循环中改变rlist这个列表

```python3
from select import select
from socket import *
s=socket()
s.setsockopt(SOL_SOCKET,SO_REUSEADDR,1)
s.bind(('0.0.0.0',8888))
s.listen(3)
rlist=[s]
wlist=[]
xlist=[]
while True:
    rs,ws,xs=select(rlist,wlist,xlist)
    for r in rs:
        if r is s:
            print('有IO事件发生')
            c,addr=rs[0].accept()
            rlist.append(c)
            print('connect from',addr)
        else:
            data=r.recv(1024)
            if not data:
                i=r
                r.close()
            else:
                print('接收到消息',data.decode())
                r.send('谢谢发送'.encode())
    rlist.remove(r)

```

很多程序员都只用rlist,因为wlist,xlist意义不大，wlist如果有了IO对象，也不会怎么样，不过切记不要一直把对象留在wlist中。xlist也就是监控IO如果发生什么错误，到哪一步的时候就会执行什么，其实不用xlist和wlist也好，少了两个for循环，它们两个需要执行的操作，可以在rlist下面去执行，一个主动发就行了，另一个用错误捕获就行了

6.按位操作是把整数按照二进制位进行操作，相当于把整数转化为二进制数对每一位都进行操作，然后再转化为整数 比如 11&14  相当于1011&1110  对每位进行或操作  得到1010 然后转化为整数返回为10

异或^ 相同为0，不同为1

