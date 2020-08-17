# day075(javascript)

1.

js是区分大小写的，有些特定的[函数](http://www.so.com/s?q=函数&ie=utf-8&src=internal_wenda_recommend_textn)是需要首[字母](http://www.so.com/s?q=字母&ie=utf-8&src=internal_wenda_recommend_textn)[大写](http://www.so.com/s?q=大写&ie=utf-8&src=internal_wenda_recommend_textn)的(比如Number,Boolean)，这些函数名是不可以随意篡改的。

2.

js内置函数prompt

prompt()方法用于显示一个带有提示信息，并且用户可以输入的对话框。

prompt(text,defaultText);

| 参数        | 描述                                       |
| ----------- | ------------------------------------------ |
| text        | 可选。要在对话框中显示的提示信息（纯文本） |
| defaultText | 可选。默认的输入文本。                     |

比如var name=prompt("请输入您的名字","Bill Gates"); name就是得到的用户输入名字

3.注意在js中，只要是非0非空等，布尔类型判断的时候都是true，和python中一样

把true和false转换为number类型的时候就是1和0，这也和python中一样，js中用内置函数(一般会首字母大写)Number转化，python用Int转化

**也就是说js和python一样，非0非空等对象转化为布尔值都是true，但是true和false转化为数值，就是1和0**

4.js中没有elif，只能else后面再接一个if(){}

5.js中的switch专用于值判断

6.js和python中很多用法一样,字符串连接也是+

7.在浏览器的检查中的调试台sources中可以进行断点的设置，断点也就是为调试。我们可以选中多少行的那个数字，就是设置断点了，然后执行到那会停一下（注意选中的这一行未执行)，我们可以鼠标移动到已经执行完的代码变量上，查看相关的值

当然旁边会有继续执行，执行一行等按钮

8.js非强制函数调用必须要在函数创建之后

9.注意js中创建匿名函数必须要用变量绑定，因为在js中函数没有名字是不规范的，就会报错，所以用变量绑定，就相当于给了函数名

10.最好不要在函数中声明全局变量，而且推荐声明所有变量一定要用var