> vue-router有两种模式：hash模式和history模式。默认是hash

## hash模式

我们都知道在hash模式下，会多出一个（#）

- URL的hash也就是锚点（#），本质上是改变**window.location**的**href**属性
- 我们可以直接通过赋值location.hash来改变href，但是页面**不发生刷新**、

**特点**：

- hash值会出现在url里，但是不会出现在http请求中，对后端完全没有影响
- 故改变hash值，不会重新刷新加载页面
- 这种模式的支持度很高，低版本的IE浏览器也支持

**原理**：`onhashchange()`事件

```javascript
window.onhashchange = function(event){
    console.log(event.oldURL,event.newURL);
    let hash = location.hash.slice(1);
}
```

**使用onhashchange()事件的好处就是**：

（1）在页面的hash值发生变化时，无需向后端发起请求，window就可以监听事件的变化，并按照规则加载相应的代码。

（2）除此之外，hash值变化对应的url都会被浏览器记录下来，这样浏览器就能实现页面的前进和后退。

（3）虽然没有请求后端服务器，但是页面和hash值对应的url关联起来了。

## history模式

> **简介**：history模式中的url没有 # ，它使用的是传统的路由分发模式，即用户在输入一个url时，服务器会接收这个请求，并解析这个url，然后做出相应的逻辑处理。

这是HTML5里面提出来的。这种模式相当于一个栈结构

**特点**：history模式需要后台配置支持。如果后台没有正确配置，访问时就会返回404.

**API**：

- **修改历史状态**：这两个方法应用与浏览器的历史记录栈，提供了对历史记录进行修改的功能。只是当他们修改时，虽然修改了url，但浏览器不会立即向后端发送请求。如果要做到改变url但又不刷新页面，就需要前端用上这两个API。
  - `history.pushState()`：相当于把当前的路径加入栈中
  - `history.replaceState()`：替代，不能前进也不能后退
- **切换历史状态**：
  - `history.back()`：出栈，相当于后退一步
  - `history.forward()`：向前进一步
  - `history.go()`：跳转到任意的一个路径
    - history.go(-1)等价于history.back()
    - history.go(1)等价于history.forward()

<u>在vue-router里如果要切换到history模式，就要进行以下配置（后端也要进行配置）</u>：

```javascript
const router = new VueRouter({
    mode:'history',
    routes:[...]
})
```

## 两种模式的对比

调用 `history.pushState()`相比于直接修改hash，存在以下优势：

- pushState()设置的新的url可以是与当前url **同源的**任意的url
  - 而hash只可修改#后面的部分，因此只能设置与当前url同文档的url
- pushState()设置新的url可以与当前url **一模一样**，这样也会把记录添加到栈中；
  - 而hash设置的新值必须和原来的不一样才会触发动作将记录添加到栈中。
- pushState()通过 **stateObject**参数可以 **添加任意类型的数据**到记录中；
  - 而hash只可添加短字符串
- pushState()可额外设置title属性供后续使用
- hash模式下，仅hash符号之前的url会被包含在请求中，后端如果没有做到对路由的全覆盖，也不会返回404错误
  - history模式下，前端的url和实际向后端发起请求的url一致，如果 没有对应路由处理，将返回404错误。

> 无论使用哪种模式，本质都是使用的 `history.pushState()`，每次pushState后，会在浏览器记录中添加一个新的记录，但是 不会触发 页面刷新，也不会请求新的数据。



