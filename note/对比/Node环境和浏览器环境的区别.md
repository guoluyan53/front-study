> [node环境与浏览器环境有哪些区别](https://www.yisu.com/zixun/691554.html#:~:text=%E5%8C%BA%E5%88%AB%EF%BC%9A1%E3%80%81node%E4%B8%ADthis%E6%8C%87%E5%90%91global%EF%BC%8C%E8%80%8C%E6%B5%8F%E8%A7%88%E5%99%A8%E4%B8%AD%E6%8C%87%E5%90%91window%EF%BC%9B2%E3%80%81Node%E7%94%A8CommonJS%E6%A0%87%E5%87%86%EF%BC%8C%E8%80%8C%E6%B5%8F%E8%A7%88%E5%99%A8%E7%94%A8ES%20Modules%E6%A0%87%E5%87%86%EF%BC%9B3%E3%80%81%E6%B5%8F%E8%A7%88%E5%99%A8%E4%B8%AD%E7%9A%84js%E5%8F%AF%E4%BB%A5%E6%93%8D%E4%BD%9CDOM%EF%BC%8C%E8%80%8Cnode%E4%B8%AD%E4%B8%8D%E4%BC%9A%EF%BC%9B4%E3%80%81I%2FO%E8%AF%BB%E5%86%99%E6%93%8D%E4%BD%9C%E4%B8%8D%E5%90%8C%EF%BC%9B5%E3%80%81%E6%A8%A1%E5%9D%97%E5%8A%A0%E8%BD%BD%E4%B8%8D%E5%90%8C%E3%80%82,%E6%9C%AC%E6%95%99%E7%A8%8B%E6%93%8D%E4%BD%9C%E7%8E%AF%E5%A2%83%EF%BC%9Awindows7%E7%B3%BB%E7%BB%9F%E3%80%81nodejs%2012.19.0%E7%89%88%EF%BC%8CDELL%20G3%E7%94%B5%E8%84%91%E3%80%82)
>
> [Node.js 和浏览器的区别](http://nodejs.cn/learn/differences-between-nodejs-and-the-browser)
>
> 

# node环境与浏览器环境有哪些区别？

1. node中的this指向global，而浏览器中指向window
2. node用CommonJS标准，而浏览器用ES Modules标准
3. 浏览器中的js可以操作DOM，而node中不会
4. IO读写操作不同
5. 模块加载不同

## 1. 全局环境下this的指向不同

在node中this指向global，而浏览器中this指向window，这就是为什么underscore中一上来就定义了 一root

而且在浏览器中的window下封装了不少的API比如alert、document、location、history等。

## 2. 模块标准

nodejs使用CommonJS模块系统，浏览器中使用的是ES Modules标准。

也就是说在node中使用require()，在浏览器中使用import

## 3. DOM操作

浏览器中的js大多数情况下是在直接或间接（一些虚拟DOM库和框架）的操作DOM。

- 因为浏览器中的代码主要是在表现层工作
- 但是node是一门服务端技术。没有一个前台页面，所以我们不会在node中操作DOM。

## 4. IO读写不同

与浏览器不同，我们需要向其他服务端技术一样读写文件，nodejs提供了比较方便的组件，而浏览器想在页面中打开一个本地图片就麻烦了好多。

## 5. 模块加载

JavaScript有一个特点，就是原生没提供包引用的API一次性把要加载的东西执行一遍，所有东西都在一起，没有分而治之，搞得特别没有逻辑性和复用性。

在node中提供了CMD的模块加载的API

node还提供了npm这种包管理工具，能更有效方便的管理我们引用的库。

# 总结

1. 浏览器和node.js都可以看作是JS的运行平台，浏览器是JS在客户端的运行时环境，而node.js是JS在服务端的运行环境。
2. JS运行在浏览器端，用于用户的交互效果。JS运行在node.js，用于服务器的操作。
3. JS需要浏览器的JS引擎进行解析执行，但是不同浏览器的JS引擎不同，存在兼容性问题。而node.js是基于 chrome v8引擎的运行时环境，无兼容性问题。
4. 对于ECMAScript语法来说，在node.js和浏览器中都能运行。node.js无法使用DOM和BOM的操作，浏览器无法执行node.js中的文件操作等功能。