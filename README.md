# Youdao_translation
利用有道词典实现一个简单翻译程序

#1.爬虫前的分析
因为要实现有道翻译的翻译功能，就需要找到它的接口，打开**审查元素**，来到**网络监听窗口(Network)**，查看API接口。


![我们可以找到有道翻译的API接口，同时是以Post方式提交](http://upload-images.jianshu.io/upload_images/6078268-c014ecec8e8a67b1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



![Data的内容](http://upload-images.jianshu.io/upload_images/6078268-4714d3cefcb6aeb1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>```
>Form Data
>i:你好     
>from:AUTO
>to:AUTO
>smartresult:dict
>client:fanyideskweb
>salt:1503049820576
>sign:f21c50e08db736608d3ec3899678a725
>doctype:json
>version:2.1
>keyfrom:fanyi.web
>action:FY_BY_CLICKBUTTION
>typoResult:true
>```
>![通过翻译'你好'和'hellow'的对比查看不同的地方](http://upload-images.jianshu.io/upload_images/6078268-3df4057486b2ca57.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>- **i：**需要进行翻译的字符串，这个地方我们输入的是‘’你好‘’。
>- **salt：**当前的时间戳
>- **action：** 判断你是按回车提交或者点击按钮提交的方式


通过查看网页源代码的方式查看有道翻译的js文件，来查看salt和sign是怎么生成的。
- **查看网页源代码找到js文件**
>![找到js文件，然后点击这个文件，跳转到这个源文件中，然后全选所有的代码，复制下来](http://upload-images.jianshu.io/upload_images/6078268-5834f0b6da1b125f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- **打开[站长工具](http://tool.chinaz.com/tools/jsformat.aspx)，把代码格式化**
>![](http://upload-images.jianshu.io/upload_images/6078268-16c020abd7834bb8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- **把格式化后的代码，复制下来，用`sublime`或者`pycharm`打开都可以，然后搜索salt，找到相关的代码**
>![](http://upload-images.jianshu.io/upload_images/6078268-13344118d6b96f81.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#2.Python使用MD5加密字符串
**1. 介于有道翻译使用了MD5加密，就对MD5加密进行了学习**
```
#导入md5加密所需模块
import hashlib

#创建md5对象
m = hashlib.md5()

#生成加密串，其中 password 是要加密的字符串
m.update('password')

#获取加密串
pw = m.hexdigest()
print(pw)
#打印结果
5f4dcc3b5aa765d61d8327deb882cf99
```
**2 . 我们可以写成函数，直接传入要加密的字符串调用即可，由于传入的参数不是字符串会报错，所以应先对参数进行判断**

```
import hashlib
import types

def md5(str):
    if type(str) is types.StringType:
        m = hashlib.md5()   
        m.update(str)
        return m.hexdigest()
    else:
        print(‘您传入的参数不是字符串’)
```

运行结果：

![实现翻译功能](http://upload-images.jianshu.io/upload_images/6078268-7adc33e6b9ebe73c.gif?imageMogr2/auto-orient/strip)
