> 如果问cookie是什么？可能只能答上来是一种缓存机制。所以还是来重新学习一下cookie吧！

接下来就来认识：

1. 什么是cookie，cookie的作用
2. cookie的工作机制
3. cookie的基本属性以及如何使用cookie

## 一、什么是cookie？

http协议本身是**无状态**的。什么是无状态呢？即服务器无法判断用户身份。cookie实际上是一小段的文本信息（key-value格式）。客户端向服务器发起请求，如果服务器需要记录该用户状态，就使用response向客户端浏览器颁发一个cookie。客户端浏览器就会把cookie存储起来。当浏览器再次请求该网站时，浏览器把请求的网址连同该cookie一同提交给服务器。服务器检查该cookie，以此来辨别用户状态。

## 二、cookie机制

当用户第一次访问并登录一个网站时，cookie的设置以及发送会经历一下4个步骤：

1. 客户端发送一个请求到服务器
2. 服务器发送一个HttpResponse响应到客户端，其中包含set-Cookie头部
3. 客户端保存cookie，之后向服务器发送请求时，httpRequest请求中会包含一个cookie的头部
4. 服务端返回响应数据

![img](https://upload-images.jianshu.io/upload_images/13949989-dcf024be2733e725.png?imageMogr2/auto-orient/strip|imageView2/2/w/400/format/webp)

## 三、cookie属性项

- **Name**：名称
- **Value**：值，对于认证cookie，value值包括web服务器所提供的访问令牌
- **Size**：大小
- **Path**：可以访问cookie的页面路径
- **Secure**：指定是否使用HTTPs安全协议发送Cookie。
- **Domain**:可以访问该cookie的域名，cookie机制并未遵循严格的同源策略，允许一个子域可以设置或获取其父域的Cookie。
- **HTTP**：该字段包含`HTTPOnly` 属性 ，该属性用来设置cookie能否通过脚本来访问，默认为空，即可以通过脚本访问。
- **Expires/Max-Age**：此cookie的超时时间。

## 四、Cookie的域名

Cookie是不可以跨域名的，隐私安全机制禁止网站非法获取其他网站的Cookie。

正常情况下，同一个一级域名下的两个二级域名也不能交互使用Cookie，比如test1.mcrwayfun.com和test2.mcrwayfun.com，因为二者的域名不完全相同。如果想要mcrwayfun.com名下的二级域名都可以使用该Cookie，需要设置Cookie的domain参数为**.mcrwayfun.com**，这样使用test1.mcrwayfun.com和test2.mcrwayfun.com就能访问同一个cookie

