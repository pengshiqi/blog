---
title: Python Decorator
date: 2017-09-13 13:51:40
tags: Python
categories: 
- 学习
---

Python 的 Decorator 是一个非常有用的工具，比如Flask的路由(router)，Decorator的目的是对已有的模块做一些修饰工作，使用方法就是在方法名前加上 '@XXX' 注解来为这个方法装饰一些东西。

<!-- more -->

了解Decorator之前，首先要了解，Python中函数可以作为参数传递给另一个函数。例如：

```python
def foo():
    print("foo")
  
def bar(func):
    func()
    
bar(foo)
```

Python Decorator 本质上是一个Python函数或类，它可以让其他函数或类在不需要做任何代码修改的前提下增加额外功能，Decorator的返回值也是一个函数/类对象。它经常用于有切面需求的场景，比如：插入日志、性能测试、事务处理、缓存、权限校验等场景，装饰器是解决这类问题的绝佳设计。有了装饰器，我们就可以抽离出大量与函数功能本身无关的雷同代码到装饰器中并继续重用。概括的讲，装饰器的作用就是为已经存在的对象添加额外的功能。

### 简单装饰器

```python
def hello(fn):
    def wrapper():
        print "hello, %s" % fn.__name__
        fn()
        print "goodby, %s" % fn.__name__
    return wrapper
 
@hello
def foo():
    print "i am foo"
 
foo()
```

运行以上代码，会看到如下输出：

```python
hello, foo
i am foo
goodby, foo
```

### @语法糖

对于Python的这个@注解语法糖- Syntactic Sugar 来说，当你在用某个@decorator来修饰某个函数func时，如下所示:

```python
@decorator
def func():
    pass
```

其解释器会翻译成下面的语句：

```python
func = decorator(func)
```

这个语句把decorator这个函数的返回值赋值回了原来的func。 

根据《[函数式编程](https://coolshell.cn/articles/10822.html)》中的**first class functions**中的定义的，你可以把函数当成变量来使用，所以，decorator必需得返回了一个函数出来给func，这就是所谓的**higher order function **高阶函数，不然，后面当func()调用的时候就会出错。 就我们上面那个hello.py里的例子来说，

```python
@hello
def foo():
    print "i am foo"
```

被解释成了：

```python
foo = hello(foo)
```

在我们的例子中，我们可以看到，**hello(foo)返回了wrapper()函数，所以，foo其实变成了wrapper的一个变量，而后面的foo()执行其实变成了wrapper()**。

知道这点本质，当你看到有多个decorator或是带参数的decorator，你也就不会害怕了。

比如：多个decorator：

```python
@decorator_one
@decorator_two
def func():
    pass
```

相当于：

```python
func = decorator_one(decorator_two(func))
```

比如：带参数的decorator：

```python
@decorator(arg1, arg2)
def func():
    pass
```

相当于：

```python
func = decorator(arg1,arg2)(func)
```

### *args、**kwargs

如果业务逻辑函数需要参数，可以把 wrapper 函数指定关键字函数：

```python
def wrapper(*args, **kwargs):
        # args是一个数组，kwargs一个字典
        logging.warn("%s is running" % func.__name__)
        return func(*args, **kwargs)
    return wrapper
```

### 类装饰器

装饰器不仅可以是函数，还可以是类，相比函数装饰器，类装饰器具有灵活度大、高内聚、封装性等优点。使用类装饰器主要依靠类的`__call__`方法，当使用 @ 形式将装饰器附加到函数上时，就会调用此方法。

```python
class myDecorator(object):
 
    def __init__(self, fn):
        print "inside myDecorator.__init__()"
        self.fn = fn
 
    def __call__(self):
        self.fn()
        print "inside myDecorator.__call__()"
 
@myDecorator
def aFunction():
    print "inside aFunction()"
 
print "Finished decorating aFunction()"
 
aFunction()
 
# 输出：
# inside myDecorator.__init__()
# Finished decorating aFunction()
# inside aFunction()
# inside myDecorator.__call__()
```

上面这个示例展示了，用类的方式声明一个decorator。我们可以看到这个类中有两个成员：
1）一个是 \__init__()，这个方法是在我们给某个函数decorator时被调用，所以，需要有一个fn的参数，也就是被decorator的函数。
2）一个是\__call__()，这个方法是在我们调用被decorator函数时被调用的。
上面输出可以看到整个程序的执行顺序。

### 装饰器顺序

一个函数还可以同时定义多个装饰器，比如：

```python
@a
@b
@c
def f ():
    pass
```

它的执行顺序是从里到外，最先调用最里层的装饰器，最后调用最外层的装饰器，它等效于

```python
f = a(b(c(f)))
```

### Reference

[理解 Python 装饰器看这一篇就够了](https://foofish.net/python-decorator.html)

[PYTHON修饰器的函数式编程](https://coolshell.cn/articles/11265.html)