---
title: python中的time模块
date: 2016-08-15 17:40:06
tags: Python
categories: Python
---

因为最近总是会用到time这个模块，需要经常时间戳和时间字符串转换，而我又不太记得，故整理了一下python中的time模块，记录下来。

### 时间表示方式
<!-- more -->

1. 在python中，通常有这几种方式来表示时间：
    * 时间戳
    * 格式化的时间字符串
    * 元组（九个元素）
2. UTC（Coordinate Universal Time），世界标准时间。在中国为UTC+8 。
3. 时间戳(timestamp)。时间戳表示的是从 *1970年1月1日 00：00：00* 开始按毫秒计算的偏移量。返回时间戳的方式有time(),clock()等。
4. 元组(struct_time)。共有9个元素，返回struct_time的函数主要有gmtime(),localtime(),strptime()等。
  例如：
  ![icon](http://i2.buimg.com/4851/f3eb5255fd74f548.png)
  各字段的含义如下：

| 索引（index） | 属性（attribute）    | 值（values）    |
| --------- | ---------------- | ------------ |
| 0         | tm_year          | eg.2016      |
| 1         | tm_mon           | 1-12         |
| 2         | tm_mday          | 1-31         |
| 3         | tm_hour          | 0-23         |
| 4         | tm_min           | 0-59         |
| 5         | tm_sec           | 0-59         |
| 6         | tm_wday          | 0-6          |
| 7         | tm_yday(一年中的第几天) | 1-366        |
| 8         | tm_isdst(是否是夏令时) | -1 (default) |

### 常用函数

1. time.localtime([secs]): 将一个时间戳转换成当前时区的struct_time。
```
>>> time.localtime()
time.struct_time(tm_year=2016, tm_mon=8, tm_mday=16, tm_hour=15, tm_min=28, tm_sec=22, tm_wday=1, tm_yday=229, tm_isdst=0)
>>> time.localtime(1471334822)
time.struct_time(tm_year=2016, tm_mon=8, tm_mday=16, tm_hour=16, tm_min=7, tm_sec=2, tm_wday=1, tm_yday=229, tm_isdst=0)
```

2. time.time(): 返回当前时间的时间戳。
```
>>> time.time()
1471334993.112127
```

3. time.mktime(t): 将一个struct_time转换成时间戳。
```
>>> time.mktime(time.localtime())
1471335138.0
```

4. time.sleep(secs): 线程推迟指定的时间运行，单位为秒。

5. time.clock(): 在不同系统上含义不同。在UNIX系统上，返回进程时间，是以秒为单位的浮点数。而在windows系统上，第一次调用返回进程运行时间，而之后的调用则是返回第一次调用到现在的运行时间。下面是在macOS上的运行示例：
```
>>> time.clock()
0.077041
>>> time.clock()
0.077398
>>> time.clock()
0.077726
```

6. time.asctime([t]): 把一个表示时间的元组或者struct_time转换形式表示出来。default参数为time.localtime()。
```
>>> time.asctime()
'Tue Aug 16 16:26:22 2016'
```

7. time.ctime([secs]): 把一个时间戳转换成time.asctime()的形式。default参数为time.time()。
```
>>> time.ctime()
'Tue Aug 16 16:30:32 2016'
>>> time.ctime(1471334822)
'Tue Aug 16 16:07:02 2016'
```

8. time.strftime(format[,t]): 把一个代表时间的元组或者struct_time转化为格式化的时间字符串。default参数为time.localtime()。如果元组中任何一个参数越界，则会抛出一个ValueError的错误。
```
>>> time.strftime('%Y-%m-%d %X', time.localtime())
'2016-08-16 16:43:05'
```

9. time.strptime(string[,format]): 把一个格式化时间字符串转换成struct_time。它和strftime()是逆操作。
```
>>> time.strptime('2016-08-16 16:43:05', '%Y-%m-%d %X')
time.struct_time(tm_year=2016, tm_mon=8, tm_mday=16, tm_hour=16, tm_min=43, tm_sec=5, tm_wday=1, tm_yday=229, tm_isdst=-1)
```

| 格式   | 含义                         |
| ---- | -------------------------- |
| %a   | 本地简化星期名称                   |
| %A   | 本地完整星期名称                   |
| %b   | 本地简化月份名称                   |
| %B   | 本地完整月份名称                   |
| %c   | 本地相应的日期和时间表示               |
| %d   | 一个月第几天（01-31）              |
| %H   | 一天第几个小时（24小时制）             |
| %I   | 第几个小时（12小时制）               |
| %j   | 一年中第几天（001-366）            |
| %m   | 月份（01-12）                  |
| %M   | 分钟数（00-59）                 |
| %p   | 本地am或pm                    |
| %S   | 秒（01-61，闰年秒占两秒）            |
| %U   | 一年中的星期数（00-53）             |
| %w   | 一个星期中的第几天（0-6）             |
| %W   | 和%U基本相同，不同的是%W以星期一为一个星期的开始 |
| %x   | 本地相应日期                     |
| %X   | 本地相应时间                     |
| %y   | 去掉世纪的年份（00-99）             |
| %Y   | 完整的年份                      |
| %Z   | 时区的名字                      |
| %%   | '%'字符                      |
