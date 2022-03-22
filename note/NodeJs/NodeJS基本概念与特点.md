> [参考文章](https://www.cnblogs.com/linuxtop/p/12422383.html#:~:text=Node.js%20is%20a%20platform%20built%20on%20Chrome%E2%80%99s%20JavaScript,data-intensive%20real-time%20applications%20that%20run%20across%20distributed%20devices.)

# 一、NodeJS的基本概念

**Node.js是一个JavaScript运行环境，他让JavaScript可以开发后端程序**，实现几乎其他后端语言实现的所有功能，可以与PHP、java、Python、.NET、Ruby等后端语言平起平坐。

Nodejs是基于V8引擎，V8引擎是Google 发布的开源的JavaScript引擎，本身就是用于Chrome浏览器的js解释部分，但是Ryan Dahl将V8搬到了服务器上，用于做服务器的软件。

## node.js的优势

**1、Node.js完全是js语法，只要懂js基础就可以学会Node.js后端开发**

Node打破了过去JavaScript只能在浏览器中运行的局面。前后端编程环境统一，可以大大降低开发成本。

**2、、Node.js具有超高的并发能力**

使用node开发后端比使用其他语言开发更加高效，可以同时连接用户的数量远比常规的多得多。

**3、实现高性能服务器**

严格地说，Node.js是一个用于开发各种web服务器的开发工具。在Node.js服务器中，运行的是高性能V8 JavaScript脚本语言，该语言是一种可以运行在服务器端的脚本语言。

**4、开发周期短、开发成本低、学习成本低**

 Node.js自身哲学，是花最小的硬件成本，追求更高的并发，更高的处理性能。

## nodejs的结构

![这里写图片描述](https://gitee.com/guoluyan53/image-bed/raw/master/img/20160621205639325)

**Nodejs结构大体分为三个部分**：

（1）`Node.js标准库`：这部分由JavaScript编写。也就是平时我们经常require的各个模块，如：http，fs，express，Request......这部分在源码的lib目录下可以看到

（2）`Node bindings`：node.js程序的main函数入口，还有提供给lib模块的c++类接口，这一层是JavaScript与底层C/C++沟通的桥梁，由C++编写，这部分在源码的src目录下可以可以看到。

（3）最底层，支持NodeJS运行的关键：

- V8引擎：用来解析、执行JavaScript代码的运行环境。
- libuv：提供最底层的IO操作接口，包括 **文件异步IO的线程池管理和网络的IO操作，是整个异步IO实现的核心**，这部分由c/c++编写，在源码的deps目录下可以看到。





# 二、NodeJS的特点

1. 它是一个JavaScript的运行环境
2. 依赖于Chrome V8引擎进行代码解释
3. 异步事件驱动
4. 非阻塞I/O
5. 轻量、可伸缩，适于实时数据交互应用
6. 单进程，单线程（这里指主线程）
7. 性能出众

