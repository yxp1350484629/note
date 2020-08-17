# day069(html)

1.<head>部分的内容都不会在页面中显示，但是会执行，<title>里是写的浏览器页面选项卡上的内容

2.可以把不用的选中，然后ctrl+/ 注释下，要用的时候再反注释

3.想到文本就要想到段落p，在p中的任何操作都是对文字的一些操作

4.段落标签是块元素，注意整个html都是都元素组成，包括这整个也是个html元素，块元素就是一行，一行的，以p段落开头结尾的无论写多少都是一行

5.标签的属性是放在<内容>里面的，而且是放在前一个<>

6.在html中也有一些是特殊字符，比如< 空格等等，那我们要显示也要通过某些特殊的方法显示，可用字符实体来用，注意在html元素中，比如在段落中你无论手动打入多少个空格，html解析都只有一个空格，要想有多个空格，就必须再用字符实体

7.想到文本就要想到段落p，想到整体结构就要想到容器div（块元素)，当然还有结构标签

8.设置属性的时候最好是一行一行排列，设置图片的大小属性时，只设置宽度大小，高度会默认按照以前宽度与设置宽度的比列缩放

9.注意属性一定是放在<>内的，而夹在两个<>之间的是内容，好比超文本协议，夹在两个之间的可以是文字也可以是图片

10.注意一般有结构的才能使用>一层一层创建，比如div*3, table>tr*2>td2创建两行两列的表

11.列表结构也是可以那样创建，列表可以有序或者无需，里面的li就相当于python中的一个序列位一样，序列位里面还可以放列表，也就是嵌套

12.表格可以设值表的属性，也就是在table那设置表属性，比如宽度高度，也可以在单元格中设置，比如居中什么，合并单元格（可以合并行，合并列)，表格里也可以放图片等

13.表单用于向客户端提交数据，其中form属性就有提交的相关属性，而里面的内容也是用<>括号包裹，内容在尖括号里面，注意，因为里面的内容是表单控件，是用来收集数据的，主要也是一些相关的属性，就要用<>包围，而不是用两个<>夹住，注意表单空间的所有属性的更改都是更改的type。

14.label用于设置表单空间的提示文本信息

```html
<div>  有容器就是一行，容器是块元素
    <label for="">用户名:</label> 用于设置这一行的控件的提示信息,内容放在最前面
    <input type="text"
    placeholder="请输入用户名">  这个是占位符，只要输入就会消失，如果忘记了需要输入什么这个时候就需要                                  label的提示了
</div>
学到现在认为两个<>中夹住的就是内容，而内容也可以是<>，因为内容有时候也需要一些属性和结构，不单单是一些文字什么。像这个表单form，属性设置为与提交相关，而内容也可能需要一定的结构，就需要div，而div中的表单控件也需要一定的样式和属性，这个时候就要提供专门的标签，在<>中设置了
```

15.get和post的区别在于，get传数据在地址上传输，没什么安全性,post采用隐藏的表单数据传输，更加安全，加密也更加安全。

16.在一个表单中的任何一个位置，可以设置

<input type="hidden" name="type" value="我们想给的值"> 这个在表单中不会显示，但是有name，就会把数据提交到数据库，我们可以根据value是不是我们给的值来判断是不是按照我们给的表单提供的数据，怕有人会模仿数据。

17.内联元素不能设置宽度，也就是系统给它适当的显示宽度。块元素可以包裹任何元素，内联元素只能包裹文本或其他内联元素，不能包裹内联块

18.做项目原则：先分析，再先做完所有功能，再调优

19.基本上所有的元素都可以进行样式调试

```html
<body>
    <div>
        <h3>用户注册</h3>
    <form style="width: 350px;" action=""> div是块元素，所以继承父标签的宽度
        <div>
            <label style="display: inline-block;width: 70px;text-align: right" for="">用户名</label>   这里必须设置位内块标签，既可以设置宽度对齐，也不把下面的挤到下一行
            <input type="text" name="username">
        </div>
        <div>
            <label style="display: inline-block;width: 70px;text-align: right" for="">密码</label>
            <input type="password" name="password">
        </div>
        <div>
            <label style="display: inline-block;width: 70px;text-align: right" for="">婚否</label>
            <input type="checkbox" name="marry"> 
        </div>
        <div>
            <label style="display: inline-block;width: 70px;text-align: right" for="">性别</label>
            <input type="radio" value="男" name="sex">男 
            <input type="radio" value="女" name="sex">女
        </div>  这个在块元素中可以随意添加文本，块元素可包括所有元素
        <div>
            <label style="display: inline-block;width: 70px;text-align: right" for="">学历</label>
            <select name="edc" id="">
                <option value="">请选择</option>
                <option value="1" >大专</option>
                <option value="2" >本科</option>
                <option value="3" >研究生</option>
            </select>
        </div>
        <div>
            <label style="display: inline-block;width: 70px;text-align: right" for="">简介</label>
            <textarea name="deo" id="" cols="30" rows="10">

            </textarea>
        </div>
        <div style="text-align: center">
            <input type="submit" value="提交">
            <input type="reset" value="重置">
        </div>
    </form>
    </div>
</body>
```

