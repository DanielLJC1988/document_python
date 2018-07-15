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