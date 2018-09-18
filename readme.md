*************************************************************************************
工作中怎么用多任务：
  1个cpu1个进程
  1个进程里面1个线程(不用手动创建)
  1个线程里面开协程去处理提高cpu使用效率

*************************************************************************************
协程的底层是生成器！
*************************************************************************************

*************************************************************************************



"""简单协程抓取网页中图片

```python


from urllib.request import *  # url库

import re, time

import gevent

from  gevent import monkey

monkey.patch_all()

def down_image(url, file_name):

    print("开始", file_name)

    # 打开网页

    request_url = urlopen(url)

    # 读取数据

    content = request_url.read()

    # 写入

    with open("./images/%s.jpg" % file_name, 'wb') as f:

        f.write(content)

    print("结束:", file_name)

def open_html():

    url = "https://www.douyu.com/directory/all"

    request_url = urlopen(url)

    content = request_url.read()

    return content.decode("utf-8")

def main():

    # with open("./source.data", "rb") as f:

    #     content = f.read().decode("utf-8")

    # images_list = re.findall(r"<img .*original=\"(.+.jpg)\"", content)

    # 打开网页，抓取整个网页内容

    content = open_html()

    images_list = re.findall(r"<img .*original=\"(.+.jpg)\"", content)
    spwan_list = list()

    i = 0
    # 循环去遍历我们的地址去写入图片
    for url in images_list:
        i += 1
        time.sleep(0.5)
        spawn = gevent.spawn(down_image, url, str(i))
        spwan_list.append(spawn)

# 让我们的主进程等一下
gevent.joinall(spwan_list)

if name == 'main':

    main()


```

*************************************************************************************

*************************************************************************************
super()与__mro__的关系：super()调用的是mro顺序表中的对象

@property，可以用这个技术实现一个常量
*************************************************************************************

面向切面编程
AOP
入口函数必须简洁
一个函数一个功能
3个if判断以上可以用字典

------

栈：	先进后出
队列：	先进先出

------




0.什么是第三方?
第1方: python所有者,吉多.范罗苏姆
第2方: 我们开发者自己
第3方: 框架提供者,比如flask的提供者是,Armin Ronacher


1.第三方web框架?
是有一些个人或者组织提供的项目半成品,加上了自己公司的业务之后才是一个完成的项目


2.有哪些知名的第三方框架呢?
Django: 是一个web框架,并且是重量级的,因为提供了很全面的工具和扩展.
flask: 是一个轻量级的框架, 只提供了基本的路由路径处理工具(Werkzeug), 和页面渲染功能(jinja2),如果要实现其他额外的功能需要安装扩展包.
Dpark: 分布式框架
Scrapy: 轻量级爬虫框架
Scikit-learn: 机器学习框架...
TensorFlow: 人工智能相关框架.
Apache: 是美国的一个软件基金会组织.里面提供了350多个开源框架.比如:Tomcat,Hadoop,Spark...


3.Flask框架的特点?
诞生时间是2010年,作者是Armin Ronacher,
里面提供了两个核心内容: Werkzeug + jinja2
Werkzeug: 负责处理请求相关内容
jinja2: 负责渲染页面的.
额外扩展: 比如邮件发送Flask_Mail,Flask_Sqlalchemy,等等.

4.什么是虚拟环境?
就是一个特殊的文件夹.

5.为什么要安装虚拟环境?
答:是为了更加方便的调试各种版本的flask,扩展包,python解释器等.

6.如何安装虚拟环境,以及flask版本?
	见<<课件>>, 或者<<安装虚拟环境过程.txt>>


7.如何使用pytharm关联,安装好的虚拟环境?
  a.从终端找到虚拟环境,并进入,which python 获取到虚拟环境路径
  b.打开pycharm关联,虚拟环境路径.


8.最简单的helloworld程序?
7行代码

9.helloworld中每句话的含义?


10.url_map,是app身上的一个属性,可以查看访问哪些视图函数和路径之间的关系
格式: app.url_map
返回: 是一个map集合,里面装着地址(路径)和视图函数之间的映射关系


11.如何给存在的视图函数指定访问方式? 可以使用methods
格式: @app.route('/路径',methods=['访问方式1','访问方式2'])
常见的访问方式: GET,POST,PUT,DELETE,等8种


12.url_for(反解析),是flask提供的一个方法,用来通过视图函数名称找到视图函数地址的方法
格式: url_for('视图函数名称',key=value), 返回的是字符串地址


13.redirect,重定向,通过路由地址,找到对应的视图函数名称
格式: redirect('路由地址'), 返回的就是一个响应体对象



14.前后端在交互的时候,格式一般指定成为json格式,因为这样的交互效率最高.
有两种方式可以指定:
1.设置响应体对象的headers属性
	resp = make_response('helloworld')
	resp.headers['Content-Type'] = 'application/json'
	return resp

2.使用flask中提供的方法,jsonify()
	dict = {
		"name":"zhangsan",
		"age":"13"
	}
	return jsonify(dict)
	简化写法:
	return jsonify(name='zhangsan',age=13);


15.在访问视图函数的时候,如何指定参数类型,比如:整数,小数,字符串等内容
格式: @app.route('/地址/<参数类型:变量名>')
常见参数类型:
	int		整数
	float	小数
	path	字符串类型


16.如何需要在浏览器的地址中只能接收三个整数,如何解决?
需要自定义类型!
之所以,int,float,path等类型可以接收整数,小数,等是因为系统已经定义好了指定的类型
指定的类型没有办法满足我们的需要,所以需要自定义.

自定义类型(转换器)格式:
1.自定义类,继承自BaseConverter
2.编写初始化方法,init方法,接收两个参数,url_map,regex,并初始化父类空间和子类空间
3.将自定义转换器类,添加到默认的转换列表中
	app.url_map.converters[key] = 转换器类


17.自定义转换器方法之to_python方法
执行时间: 匹配到规则之后, 执行视图函数之前
作用: 用来过滤数据,还可以做编码转换


18.abort(代号),主动抛出异常代号
场景: 当访问服务器资源的时候,如果不存在/权限不够,可以直接使用abort抛出异常

errorhandler(代号): 可以捕捉异常代号,返回统一的界面.装饰方法执行


19.在使用app.run()的时候,可以传递参数
参数有: host: IP地址默认是127.0.0.1
参数有: port: Port端口默认是5000
参数有: debug: 默认是False
	如果设置成True好处:
		1.改变代码之后不需要重新运行,只需要保存即可
		2.报错之后有友好提示

20.app运行的时候有些配置信息需要加载,有三种方式可以加载内容
方式一: 可以从类中加载
app.config.from_object(类名称)
方式二: 可以从配置文件
app.config.from_pyfile(文件名称)
方式三: 可以从环境变量加载,依赖文件,不常用
app.config.from_envvar('环境名称key')

作用: 以后可以加载数据库配置信息,redis配置信息,session配置信息

21.request,是Werkzeug提供好的请求对象,里面封装了请求相关的所有信息,比如:请求地址,请求参数,请求方式,等等
request.url: 请求地址
request.method: 请求方式
request.args: 请求参数,并且是问好后面拼接的内容,www.baidu.com?name=zhangsan&age=13
....



ctrl+p	pycharm查看函数参数提示信息




test













