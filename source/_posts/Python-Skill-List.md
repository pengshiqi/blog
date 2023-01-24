---
title: Python Skill List
date: 2018-01-25 11:01:05
tags: Python
categories: 
- 学习
---

偶然间在知乎上看到了一篇文章: [我眼中一个好的Pythoneer应该具备的品质](https://zhuanlan.zhihu.com/p/33266239)，于是想思考一下这些问题的答案。

<!-- more -->

* 知道python最常见的解释器有哪些。
* 知道python的语法，缩进和符号对应的含义。
* 知道python所有关键字的含义和使用。
* 知道python中大部分常用的类型（布尔值，字符串类型，数字类型，序列，集合，字典，生成器...）。
* 知道如何编写pythonic的代码（上下文管理器，推导表达式，装饰器，切片…）。
* 知道如何避免python中的一些坑，如可变的默认参数，闭包的迟绑定。
* 知道python中的函数式编程以及map、filter的使用。
* 知道list，tuple，dict，set等的时间复杂度和空间复杂度。
* 知道GIL的限制以及与多线程的关系。
* 知道python的命名空间查找规则（LEGB）。
* 知道python多继承的查找规则（MRO）。
* 知道python 2.x和3.x的主要差异。
* 知道property的含义以及其描述器实现。
* 知道python中dict的底层实现，以及与OrderDict的关系。
* 知道`__slots__`的含义以及使用场景。
* 知道如何定义和使用元类，了解其使用场景。
* 知道python中的多进程和多线程模型，知道多进程和多线程下间的通信实现。
* 知道进程池和线程池在python中对应的库和使用方式。
* 知道python中多线程间常用的同步原语的使用方式。
* 知道python大多数常用的标准库以及其用途。
* 知道python中type和object之间的关系。
* 知道深拷贝和浅拷贝在python中的实现方式。
* 知道python的调试工具（`logging`，`pdb`），知道`unittest`和`doctest`的使用。
* 知道python中的打包方式（`setup.py`）。
* 知道PEP8常见的范式以及代码格式化方法。
* 知道str，bytes和字符串编码的关系以及其相互转换的方法。
* 知道python中的几种字符串拼接方式与效率对比。
* 知道鸭子类型（duck typing）的含义与其在python中的表现形式。
* 知道函数和方法的区别，知道绑定方法（bound-method）与未绑定方法（unbound-method）的关系。
* 知道`a = list()`和`a = []`的区别。
* 知道dict和set的常见操作，知道set之间集合运算的语法糖。
* 知道asyncio的使用方式和使用场景。
* 知道如何用python实现最常用的设计模式。
* 知道如何用python做web编程，以及WSGI协议是什么。
* 知道如何利用`collections`，`itertools`，`operator`等模块来高效地操作容器对象。
* 知道使用python中正则表达式的匹配、查找、切分和替换。
* 知道如何使用`virtualenv`，清楚其用途。
* 知道如何使用`pip`，以及其与`requirements`文件的关系。
* 知道python中序列化的常用库和接口（`json`，`pickle`）。
* 知道`os`和`sys`库常用的方法，和操作文件和目录的方式。
* 知道python中`datetime`库的常用操作。
* 知道普通文件/二进制文件读写的方式，知道`StringIO`和`BytesIO`的用途。
* 知道以单下划线开头、双下划线开头和双下划线包围的变量分别代表着什么含义。
* 知道`__init__`和`__new__`方法在class和type中分别的作用是什么。
* 知道类变量和实例变量的区别。
* 知道`__dict__`在类中的含义，以及类属性和方法与`__dict__`的关系。
* 知道Mixin模式以及在python中的用途。
* 知道python中生成器的实现以及其使用场景。
* 知道python中抽象类的实现方式，以及其抽象基类模块，知道如何用python类实现一个抽象容器类型。
* 知道`dict`和`UserDict`的关系，以及为什么有`UserDict`的存在。
* 知道普通方法，`classmethod`和`staticmethod`的区别。
* 知道装饰器中添加`functools.wraps`的含义与作用。
* 知道`__getattr__`和`__getattribute__`的作用以及其顺序关系。
* 知道python中性能测量的方式，如`cProfile`，`tracemalloc`。
* 知道python中自省的使用方式，知道`inspect`库的常见用法。
* 知道python中的模块定义，以及导入模块的各种姿势。
* 知道python中弱引用的使用方式，知道python中gc的回收算法方式以及回收规则。
* 知道`sys.settrace`和`sys.setprofile`在python中的用途和使用方式。
* 知道`global`和`local`关键字在python中的含义和其使用场景。
* 知道python中`==`与`is`的区别。
* 知道`for-else`，`try-else`的含义和用途。
* 知道`.pyc`文件的含义，清楚python代码大概的执行过程。
* 知道python代码的中文编码处理，以及如何处理乱码。
* 知道`_`在python解释器中的含义。
* 知道python中格式化字符串的方式以及其常见格式语法。
* 知道python中常见的魔术方法和其使用方式。





-----



## 1. python常见的解释器

* **CPython**

>当我们从[Python官方网站](https://www.python.org/)下载并安装好Python 2.7后，我们就直接获得了一个官方版本的解释器：CPython。这个解释器是用C语言开发的，所以叫CPython。在命令行下运行`python`就是启动CPython解释器。
>
>CPython是使用最广的Python解释器。

* **IPython**

>IPython是基于CPython之上的一个交互式解释器，也就是说，IPython只是在交互方式上有所增强，但是执行Python代码的功能和CPython是完全一样的。好比很多国产浏览器虽然外观不同，但内核其实都是调用了IE。
>
>CPython用`>>>`作为提示符，而IPython用`In [序号]:`作为提示符。

* **PyPy**

> PyPy是另一个Python解释器，它的目标是执行速度。PyPy采用[JIT技术](http://en.wikipedia.org/wiki/Just-in-time_compilation)，对Python代码进行动态编译（注意不是解释），所以可以显著提高Python代码的执行速度。
>
> 绝大部分Python代码都可以在PyPy下运行，但是PyPy和CPython有一些是不同的，这就导致相同的Python代码在两种解释器下执行可能会有不同的结果。如果你的代码要放到PyPy下执行，就需要了解[PyPy和CPython的不同点](http://pypy.readthedocs.org/en/latest/cpython_differences.html)。

* **Jython**

> Jython是运行在Java平台上的Python解释器，可以直接把Python代码编译成Java字节码执行。

* **IronPython**

> IronPython和Jython类似，只不过IronPython是运行在微软.Net平台上的Python解释器，可以直接把Python代码编译成.Net的字节码。

 

**Reference:**

[廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/001407375700558864523211a5049c4983176de304549c8000)



## 2.GIL的限制以及与多线程的关系 

首先需要明确的一点是`GIL`并不是Python的特性，它是在实现Python解析器(CPython)时所引入的一个概念。而JPython就没有GIL。

GIL全称`Global Interpreter Lock`，为了避免误导，我们还是来看一下官方给出的解释：

> In CPython, the global interpreter lock, or GIL, is a mutex that prevents multiple native threads from executing Python bytecodes at once. This lock is necessary mainly because CPython’s memory management is not thread-safe. (However, since the GIL exists, other features have grown to depend on the guarantees that it enforces.)

具体见[Python的GIL是什么鬼，多线程性能究竟如何](http://cenalulu.github.io/python/gil-in-python/)。



## 3. python的命名空间查找规则（LEGB）

LEGB含义解释： 
**L-Local(function)**：			函数内的名字空间 
**E-Enclosing function locals**：  外部嵌套函数的名字空间(例如closure) 
**G-Global(module)**：			函数定义所在模块（文件）的名字空间 
**B-Builtin(Python)**：			Python内置模块的名字空间



## 4. python 2.x和3.x的主要差异



4.1 `__future__` 模块

Python 3.x 介绍的 一些Python 2 不兼容的关键字和特性可以通过在 Python 2 的内置 `__future__` 模块导入。

例如： `nested_scopes`,  `generators`,  `division`,  `absolute_import`,  `with_statement`,  `print_function`, `unicode_literals`。



4.2 `print`

print从语法调整为了函数。



4.3 整除

python2.7中，`/`为整除，而python3中，`/`的结果为float。



4.4 unicode

Python 2 有 ASCII str() 类型，`unicode()` 是单独的，不是 `byte` 类型。
在 Python 3，我们最终有了 `Unicode (utf-8)` 字符串，以及一个字节类：`byte` 和 `bytearrays`。



4.5 `xrange`

python3 中的`range`被`xrange`替代。



4.6 Raising Exception

Python2 中有两种写法：

```python
# 1.
raise IOError, "file error"

# 2.
raise IOError("file error")
```

而Python3中只保留了第二种写法。



4.7 Handling Exception

```python
# Python2
try:
    let_us_cause_a_NameError
except NameError, err:
    print err, '--> our error message'

# Python3 
try:
    let_us_cause_a_NameError
except NameError as err:
    print(err, '--> our error message')
```



4.8 `next()`函数和`.next()`方法

在Python2.7中，两种方式都可以。

在Python3 中，只能使用`next()`函数。



4.9 For循环变量和全局命名空间泄漏

在 Python 3.x 中 `for` 循环变量不会再导致命名空间泄漏。



4.10 比较不可排序类型

Python3中，不可排序的类型做比较的时候会抛出错误。

