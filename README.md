
有关该项目中的代码部分简单解释，可以查看博客。

1、[Netty+WebSocket 获取火币交易所时时数据项目](https://www.cnblogs.com/qdhxhz/p/11278746.html)

### 简介

本项目使用SpringBoot+Netty来开发WebSocket服务器,与火币交易所Websocket建立连接,时时获取火币网交易所推送过来的交易对最新数据。

项目本身也是我在之前公司为了获取各大交易所数据所开发的项目,现在只是重新整理了下代码，现在它更像一个脚手架项目，可以在此基础上很方便的添加其它交易所。


## 一、项目概述

#### 1、技术架构

```
SpringBoot2.1.5 + Netty4.1.25 + Maven3.5.4 + lombok(插件)
```

#### 2、项目整体结构

```makefile
spring-boot-netty-websocket-huobi # 项目名称
|
---src
      |
      ---com.jincou.exchange.client
                                  |# 连接交易所websocket的公共类
                                  ---common
                                  |# 枚举类
                                  ---enums
                                  |# 获取火币网交易所数据具体实现类
                                  ---huobi
                                  |# 定时任务
                                  ---scheduler    
                                  |# 工具类
                                  ---util
      ---resources                
           | # 配置类
            ---application.yml
```

`说明`

从这个项目的架构来看，当你需要添加获取一个新的交易所数据的时候，也很方便，因为这里已经把公共部分抽离出来了。

#### 3、项目测试

直接启动Springboot启动类Application.java,就可以获取火币网推送过来交易对的数据了。

如图

![](https://img2018.cnblogs.com/blog/1090617/201907/1090617-20190731184311639-1781494089.gif)

![](https://img2018.cnblogs.com/blog/1090617/201907/1090617-20190731185211763-899166739.png)


## 二、需要注意的点

#### 1、服务器问题

一般交易所的服务器都在国外，所以我们本地是无法建立Websocket连接的，除非本地翻墙。

同样项目也不能部署到阿里云等国内服务器，你只能选择香港或者国外服务器部署项目。

这里是火币网专门为我们提供的国内测试地址，所以本地可以获取数据。

#### 2、获取交易所最新交易对数据问题

我们在向交易所Websocket订阅交易对的时候，首先就是要知道该交易所有哪些交易对，这份数据是需要我们单独去获取的，而且不是一次获取就好了。

因为该交易所可能新增或者删除交易对。所以需要我们通过定时任务去获取更新最新的交易对数据。

我这边只是模拟了一个交易对`btcusdt`,并没有提供获取最新交易对数据的服务。

#### 3、数据存储问题

这也是最值得思考的一个问题，数据我们是获取了，但如果保存！

正常合理的开发应该获取数据是一个微服务，处理获取的数据是一个微服务。那么只需要获取数据后去调处理数据微服务就可以保存数据了。

但在这里，如果只是这样是行不通的。

因为**火币网向我们推送的消息的速度会比我们调其它服务保存的数据要快，这就会存在数据丢失的情况发生**。

这里仅仅是输出一个`btcusdt`交易对,并且只是订阅一个`k线`主题,而实际上交易所会有上百个交易对和几种订阅主题,

这样的消息推送速度是上面的几百倍。所以你会发现如果你不做任何改动，对于一些大的交易所而言，你的数据是来不及存储的。

<br>

`补充`

这边之前也写过有关 Netty 和 Websocket 相关的博客文章，可以做个参考

[1、Netty专题（共9篇）](https://www.cnblogs.com/qdhxhz/category/1343708.html)

[2、Websocket专题（共5篇）](https://www.cnblogs.com/qdhxhz/category/1166311.html)

