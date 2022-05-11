> [参考文章](https://cloud.tencent.com/developer/article/1887095)

# WebSocket

## 1. 对WebSocket的理解

WebSocked是HTML5提供的一种浏览器与服务器进行**全双工通讯**的网络技术，属于应用层协议。它基于TCP传输协议，并复用HTTP的握手通道。浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输。

WebSocket的出现就解决了半双工通信的弊端。它最大的特点是：**服务器可以向客户端主动推动消息，客户端也可以主动向服务器推送消息**。

![img](https://ask.qcloudimg.com/http-save/1037212/ccc22f4eea2be82335a9c646f72fe61e.png?imageView2/2/w/1620)

> 上图可知WebSocket在进行通信之前都会进行一次握手，之后就可以创建持久性连接了。

 **WebSocket原理**：客户端向WebSocket服务器通知（notify）一个带有所有接收者ID（recipients IDs）的事件（event），服务器接收后立即通知所有活跃的（active）客户端，只有ID在接收者ID序列中的客户端才会处理这个事件。

## 2. WebSocket的API

### （1）WebSocket的构造函数

要使用WebSocket，就必须创建一个WebSocket对象。

```javascript
const my = new WebSocket(url,protocols);
```

### （2）WebSocket对象的属性

![img](https://ask.qcloudimg.com/http-save/1037212/1a5b28ece499ec52b4d6de02742b9084.png?imageView2/2/w/1620)

1. **onclose**：用于指定连接关闭后的回调函数
2. **onerror**：用于指定连接失败后的回调函数
3. **onmessage**：用于指定当从服务器受到信息时的回调函数
4. **onopen**：用于指定连接成功后的回调函数
5. **readyState**（只读）：返回当前WebSocket的连接状态，共有四种
   1. `CONNECTING`：正在连接中，对应的值为0
   2. `OPEN`：已经连接并且可以通讯，对应的值为1
   3. `CLOSEING`：连接正在关闭，对应的值为2
   4. `CLOSED`：连接已关闭或者没有连接成功，对应的值为3

### （3）WebSocket的方法

**WebSocket的方法主要有两个**：

1. `close([code[,reason]])`：该方法用于关闭WebSocket连接，如果连接已经关闭，则此方法不执行任何操作
2. `send(data)`：该方法将需要通过WebSocket链接传输至服务器的数据排入队列，并根据所需要传输的数据的大小来增加bufferedAmount的值。若数据无法传输（比如数据需要缓存而缓冲区已满）时，套接字会自行关闭

### （4）Websocket的事件

使用 addEventListener() 或将一个事件监听器赋值给 WebSocket 对象的 oneventname 属性，来监听下面的事件。

**以下是几个事件：**

- *1）*`close`：当一个 WebSocket 连接被关闭时触发，也可以通过 onclose 属性来设置；
- *2）*`error`：当一个 WebSocket 连接因错误而关闭时触发，也可以通过 onerror 属性来设置；
- *3）*`message`：当通过 WebSocket 收到数据时触发，也可以通过 onmessage 属性来设置；
- *4）*`open`：当一个 WebSocket 连接成功时触发，也可以通过 onopen 属性来设置。

## 3. 特点

**WebSocket特点如下**：

*1）*较少的控制开销：在连接创建后，服务器和客户端之间交换数据时，用于协议控制的数据包头部相对较小；

*2）*更强的实时性：由于协议是全双工的，所以服务器可以随时主动给客户端下发数据。相对于 HTTP 请求需要等待客户端发起请求服务端才能响应，延迟明显更少；

*3）*保持连接状态：与 HTTP 不同的是，WebSocket 需要先创建连接，这就使得其成为一种有状态的协议，之后通信时可以省略部分状态信息；

*4）*更好的二进制支持：WebSocket 定义了二进制帧，相对 HTTP，可以更轻松地处理二进制内容；

*5）*可以支持扩展：WebSocket 定义了扩展，用户可以扩展协议、实现部分自定义的子协议。

- 支持双向通信，实时性更强
- 可以发送文本，也可以发送二进制数据
- 建立在TCP协议之上，服务端的实现比较容易
- 数据格式比较轻量，性能开销小，通信高效
- 没有同源限制，客户端可以与任意服务器通信
- 协议标识符是ws（如果加密，则为wss），服务器网址就是URL
- 与http协议有良好的兼容性。默认端口也是80和443，并且握手阶段采用http协议，因此握手时不容易屏蔽，能通过各种http代理服务器。

## 4. 使用

**WebSocket的使用方法**：

在客户端中：

```javascript
//在index.html中直接写WebSocket，设置服务端的端口为9999
let ws = new WebSocket('ws://localhost:9999');
//在客户端与服务端建立连接后触发
ws.onopen = function(){
    console.log("Connection open.");
    ws.send('hello');
};
//在服务端给客户端发来消息的时候触发
ws.onmessage = function(res) {
    console.log(res);  //打印的是MessageEvent对象
    console.log(res.data);  //打印的是收到的消息
};
//在客户端与服务端建立关闭后触发
ws.onclose = function(evt){
    console.log("Connection closed");
};
```

## 5. 应用

即时通讯、实时音视频、在线教育和游戏等领域。

## 6. WebSocket与HTTP有什么关系？

WebSocket是一种与HTTP不同的协议。两者都位于OSI模型的应用层，并且依赖与传输层的TCP协议。

RFC 6455中规定：WebSocket被设计为在HTTP80 和443 端口上工作，并支持HTTP代理和中介，从而使其与HTTP协议兼容。为了实现兼容性，WebSocket握手使用HTTP **Upgrade**头，从HTTP协议更改为WebSocket协议。