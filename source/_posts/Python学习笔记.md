---
title: Python学习笔记
date: 2017-12-11 20:43:53
tags:
- Python
categories:
- 编程
toc: true
description: 基本语法、常用模块、FAQ
---

# 常用模块

## numpy

- **将数组保存到文件、读取**
```python
np.savetxt(filename,X)
X=np.loadtxt(filename)
```

## logging

转自[Python logging模块详解](http://blog.csdn.net/zyz511919766/article/details/25136485/)

简单将日志打印到屏幕：
```python
import logging  
logging.debug('debug message')  
logging.info('info message')  
logging.warning('warning message')  
logging.error('error message')  
logging.critical('critical message')  

输出：

WARNING:root:warning message
ERROR:root:error message
CRITICAL:root:critical message

```

可见，默认情况下python的logging模块将日志打印到了标准输出中，且只显示了大于等于WARNING级别的日志，这说明默认的日志级别设置为WARNING（日志级别等级CRITICAL > ERROR > WARNING > INFO > DEBUG > NOTSET），默认的日志格式为日志级别：Logger名称：用户输出消息。

灵活配置日志级别，日志格式，输出位置
```Python
import logging  
logging.basicConfig(level=logging.DEBUG,  
                    format='%(asctime)s %(filename)s[line:%(lineno)d] %(levelname)s %(message)s',  
                    datefmt='%a, %d %b %Y %H:%M:%S',  
                    filename='/tmp/test.log',  
                    filemode='w')  

logging.debug('debug message')  
logging.info('info message')  
logging.warning('warning message')  
logging.error('error message')  
logging.critical('critical message')  

查看输出：
cat /tmp/test.log
Mon, 05 May 2014 16:29:53 test_logging.py[line:9] DEBUG debug message
Mon, 05 May 2014 16:29:53 test_logging.py[line:10] INFO info message
Mon, 05 May 2014 16:29:53 test_logging.py[line:11] WARNING warning message
Mon, 05 May 2014 16:29:53 test_logging.py[line:12] ERROR error message
Mon, 05 May 2014 16:29:53 test_logging.py[line:13] CRITICAL critical message
```
可见在logging.basicConfig()函数中可通过具体参数来更改logging模块默认行为，可用参数有
filename：用指定的文件名创建FiledHandler（后边会具体讲解handler的概念），这样日志会被存储在指定的文件中。
filemode：文件打开方式，在指定了filename时使用这个参数，默认值为“a”还可指定为“w”。
format：指定handler使用的日志显示格式。
datefmt：指定日期时间格式。
level：设置rootlogger（后边会讲解具体概念）的日志级别
stream：用指定的stream创建StreamHandler。可以指定输出到sys.stderr,sys.stdout或者文件，默认为sys.stderr。若同时列出了filename和stream两个参数，则stream参数会被忽略。

format参数中可能用到的格式化串：
%(name)s Logger的名字
%(levelno)s 数字形式的日志级别
%(levelname)s 文本形式的日志级别
%(pathname)s 调用日志输出函数的模块的完整路径名，可能没有
%(filename)s 调用日志输出函数的模块的文件名
%(module)s 调用日志输出函数的模块名
%(funcName)s 调用日志输出函数的函数名
%(lineno)d 调用日志输出函数的语句所在的代码行
%(created)f 当前时间，用UNIX标准的表示时间的浮 点数表示
%(relativeCreated)d 输出日志信息时的，自Logger创建以 来的毫秒数
%(asctime)s 字符串形式的当前时间。默认格式是 “2003-07-08 16:49:45,896”。逗号后面的是毫秒
%(thread)d 线程ID。可能没有
%(threadName)s 线程名。可能没有
%(process)d 进程ID。可能没有
%(message)s用户输出的消息

### 输出到屏幕同时存入文件

```Python
import logging

ISOTIMEFORMAT='%m-%d_%H-%M'
logger=logging.getLogger()
log_file=gl.LOG_PATH+'SCNN-'+time.strftime(ISOTIMEFORMAT,time.localtime())+'.log'
log_fh=logging.FileHandler(log_file)
log_ch=logging.StreamHandler()
log_formatter=logging.Formatter('%(asctime)s %(filename)s[line:%(lineno)d] %(levelname)s %(message)s')
log_fh.setFormatter(log_formatter)
log_ch.setFormatter(log_formatter)
logger.addHandler(log_fh)
logger.addHandler(log_ch)
logger.setLevel(logging.DEBUG)

```


# 《Python核心编程》笔记

## 安装配置
1. 启动Eclipse, 点击Help->Install New Software...   在弹出的对话框中，点Add 按钮。  Name中填:Pydev,  Location中填http://pydev.org/updates
2. 在Eclipse菜单栏中，点击Windows ->Preferences.   
在对话框中，点击pyDev->Interpreter - Python.  点击New按钮， 选择python.exe的路径, 打开后显示出一个包含很多复选框的窗口. 点OK

## 快速入门
大小写敏感
变量使用前不需要声明
代码不使用大括号,通过缩进表示代码逻辑

### 输出
`print`
直接使用变量名也可以直接以输出变量的字符串形式


### 注释
单行注释使用符号`#`

### 操作符
`/`为普通除法
`//`为浮点除法
`**`为乘方
逻辑操作符为`and or not`
可使用`3<4<5`这样的表达式
不支持`--`和`++`

### 数据类型
#### 数字
int long bool float complex decimal
**long的长度比java大.整型可以自动转换为long,不会有数字溢出**
decimal为十进制浮点型
```python
>>>1.1
1.100000000000001
>>>print(decimal.Decimal('1.1'))
1.1
```
#### 字符串
单引号`'`双引号`"`都行
**使用三个单引号`'''`或双引号`"""`** 可用来包含特殊字符
```python
>>>aStr='''this
...is a string'''
>>>pint(aStr)
this
is a string
>>>aSre
this\nis a string
```
第一个字符索引为`[0]`,最后一个为`[-1]`
`[:]`可得到子串
```python
>>>aStr='python'
>>>aStr[2:5]
tho
#包含左端不包含右端.可使用[:3]{2:]格式
```
`+`用于字符串链接
`*`用于字符串重复
```python
>>>'py'+'thon'
python
>>>'py'*3
pypypy
```

### 列表和元组
列表元素用`[]`,元组元素用`()`
都可使用切片运算`[:]`

### 字典
键值对

### if
```python
if expression:
	if_suite
elif expression:
	elif_suite
else
	else_suite
```

### while循环
```python
while expression:
	while_suite
```

### for循环
更像foreach
```python
for item in set:
	print(item)
```
`range()`函数,指定一个范围,控制循环
```python
for i in range(3)
	print(i)
```

### 列表解析
```python
squared=[x**2 for x in range(4)]
```

### 文件读写
`file=open(name,mode)`
r:读取 w:写入 a:添加 +:读写 b:二进制访问 默认为r

### 函数
函数通过引用调用,若改变参数值会影响到原始值
```python
def function_name(args):
	'optiinal documentation string'
	function_suite
```
参数可以有默认值`def functionName(arg1=value):`

### 类
```python
#声明类
class ClassName(base_class[es]):
	static_meber
	__init__(self):#构造函数,自动执行
		...
#创建类
aInstance=ClassName()

```
没有基类的话用`(object)`

### 一些实用函数
|函数|描述
|-|-
|dir([obj])|显示对象的属性,若无参数,则显示全局变量的名字
|help([obj])|显示对象的文档,若无参数进入帮助模式
|int(obj)|转换为整型
|len(obj)|对象的长度
|range([start],stop,[step])|返回整型列表,[srart,stop),start默认0,step默认1
|str(obj)|转换为字符串
|type(obj)|返回对象的类型

### 语句语法
- 换行为行分隔符
- `\`继续,即下一行与本行为同一行代码
	`() [] {} 和 ''' '''- `间的代码可多行书写,不需要\
-	;`分隔一行中的两个语句
- `:`分隔代码块的头和体
- 代码组由不同的缩进分隔

### 参数赋值
**对象都是通过引用传递的**
可进行链式赋值`x=y=z=1`
多元赋值`x,y,z=1,2,3`,此时建议使用括号
交换两个变量的值(x,y)=(y,x)
