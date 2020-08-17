# day053(POLL,EPOLL)

1.poll多IO复用，原理和select相同，不同的是，需要先用poll方法建立poll对象，（EPOLL方案是调用epoll方法)然后通过poll对象调用register方法为IO对象添加监控，select模块里的有关于POLL，EPOLL方案的监控属性。

最后用poll对象里的poll()方法，添加监控，一般这里是循环，poll，epoll的关键是通过循环不断添加register处理，注意监控返回的是一个列表元组，一般用for 来循环访问，其中第一个是IO对象的文件符，第二个是具体的IO事件

```python3
from select import *
from socket import *
s=socket()
s.setsockopt(SOL_SOCKET,SO_REUSEADDR,1)
s.bind(('0.0.0.0',8888))
s.listen(3)
a=poll() EPOLL方案这里调用epoll()
a.register(s,POLLIN)  EPOLL方案这里是EPOLLIN
c={s.fileno():s}  用字典来存储IO对象文件符和IO对象，便于后面根据文件符查找
while True:
    events=a.poll()    注意这里EPOLL和POLL方案都是poll方法，提交监控
    for i,r in events:
        if i==s.fileno():
            b,addr=c[i].accept()
            print(r)
            a.register(b,POLLIN)EPOLL方案这里是EPOLLIN
            c[b.fileno()]=b
        elif r&POLLIN:    判断r是不是除s对象外的客户端套接字阻塞事件发生
            print(r,POLLIN） EPOLL方案这里是EPOLLIN
            data=c[i].recv(1024)
            if not data:
                a.unregister(i)
                c[i].close()
                del c[i]
            else:
                print('接收到',data.decode())
                c[i].send(b'thanks')
```

EPOLL方案出现的比较晚，性能比select和poll方案都高

EPOLL那些IO事件发生的监控选项比较多，也就是select方案里的EPO。。。。比较多

2.本地套接字（本地应用之间的数据传输)

和网络套接字不同的是在创建流式套接字等需要设置不同的属性，本地套接字通过绑定本地文件(套接字文件)进行操作（关键在于通信两个软件必须连接同一个文件)。相当于软件用函数等接口通过操作系统用一个内存文件实现数据收发，和网络连接不同，接收端要绑定那个文件，而且发送端connect那个文件，相当于通过一个媒介在传输。之后和网络套接字原理差不多。（但是两端发送都用send,因为不需要地址)

注意套接字文件是内存中的一个虚拟文件,ls -l 查看，可以看见第一个为s，显示该文件为套接字文件，而且它的大小永远为0，表示不占用磁盘文件。本地套接字在本地两个程序间数据互动效率还是很高的。

3.并发与并行

并发:一个cpu内核处理多个任务，当然这只是表面上的，同一时间点，一个内核只能处理一个任务，但是它可以频繁切换处理不同的任务，优先与则后，达到并发

并行：多个cpu内核处理多个任务，不同内核任务之间关系叫并行，也就是同一时间点通过多个内核执行多个任务

操作系统内核是操作系统的核心，也就是操作系统，来处理一些数据，进程。cpu内核属于硬件，但是操作系统内核需要依赖于cpu内核等

就相当于进程要想占用底层cpu时间片，必须依赖操作系统内核来分配。