[参考文章浅谈cookie，session和localStorage，sessionStorage的区别](https://segmentfault.com/a/1190000017155151)

## 前言

天啊，我一直以为session和sessionStorage是一个玩意，原来不是的。有必要重新认识一下他们几个了。

## 啥是cookie和session？

> cookie和session都是用来跟踪浏览器用户身份的会话方式。

浏览器的缓存机制提供了可将用户数据存储在客户端上的方式，可以利用cookie和session跟服务器进行数据交互。

### （1）cookie

**cookie机制**：如果不在浏览器中设置过期时间，cookie被保存在内存中，生命周期随着浏览器的关闭而结束，这种cookie简称为会话cookie。如果在浏览器中设置了cookie的过期时间，cookie会被保存在硬盘中，关闭浏览器后，cookie数据仍然存在，直到过期事件结束才消失。

cookie是服务端发送给客户端的特殊信息，cookie是以文本的方式保存在客户端，每次请求时都带上它。

![cookie原理](https://s2.loli.net/2022/04/05/wDFKCTBIf7VWrqJ.png)

### （2）session

**session机制**：当服务器收到请求需要创建session对象时，首先会检查客户端请求中是否包含sessionId。如果有sessionId，服务器将根据id返回对应session对象。如果客户端请求中没有sessionId，服务器会创建新的session对象，并把sessionID在本次响应中返回给客户端。通常使用cookie方式存储sessionID到客户端，在交互中浏览器按照规则将sessionId发送给服务器。

## session 和 sessionStorage

！！这两者没有任何关系！！

- sessionStorage存储在客户端，session在服务器端
- session主要用于维护会话状态
- sessionStorage则是在会话期间存储相关数据

但是session 与 sessionStorage会话周期是不同的：

1. 关闭浏览器或者服务端session过期，会话结束
2. 关闭当前选项卡或者浏览器窗口，sessionStorage数据被删除，也就算会话结束。

**注：在新标签或窗口打开一个页面会初始化一个新的会话，即便链接相同也是如此**。

## cookie和session的区别

1. **存储位置不同**：cookie的数据信息存储在客户端浏览器上，session存储在服务器上。
2. **存储容量不同**：单个cookie保存的数据一般为4kb，一个站点最多能白村20多个cookie。session没有限制，但是存储过多的session会导致服务器性能下降。
3. **存储的方式不同**：cookie中只能保存ASCLL字符串，session中能存储任何类型的数据。
4. **cookie不安全，可以用来诈骗。session比较安全，不存在敏感信息泄露的风险**。
5. **有效时间不同**：cookie可以通过设置有效期达到长期有效。session当关闭窗口或浏览器就会失效。
6. cookie支持跨域名访问，session不支持跨域名访问。

