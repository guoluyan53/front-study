> 我觉得这些文章可以帮助理解：
>
> [Vue响应式原理](https://www.jianshu.com/p/cdd7dde12786)

# 前言

#### 数据响应式原理 == 数据双向绑定原理？？？

那肯定是不一样的。

- **数据响应式原理**：通过数据的改变去驱动DOM视图的变化
- **双向数据绑定原理**：双向数据绑定除了数据驱动DOM之外，DOM的变化反过来会影响数据，这是一个双关的过程。

所以把这vue的双向数据绑定理解为响应式是不准确的。

# 一、Vue响应式原理/双向数据绑定

## 过程说明

<u>vue.js是采用 **数据劫持**结合 **发布者-订阅者模式**的方式</u>

通过 `Object.defineProperty()`来劫持各个属性的setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。主要分为以下几个步骤：

1. 需要`observe`的数据对象进行递归遍历，包括子属性对象的属性，都加上setter和getter这样的话，给这个对象的某个值赋值，就会触发setter，那么就能监听到了数据变化。

2. `compile`解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数update()，添加监听数据的订阅者，一旦数据有变动，收到通知，更新视图。

3. `Watcher`订阅者是 `Observe` 和 `Compile`之间通信的桥梁，主要做的事情是：

   ① 在自身实例化时往属性订阅器（订阅器管理员）（dep）里面添加自己

   ② 自身必须有一个update()方法

   ③ 待属性变动 dep.notice()（订阅器管理员通知变动了）通知时，能调用自身的update()方法，并触发Compile中绑定的回调，则功成身退

4. MVVM作为数据绑定的入口，整个Observer、Compile和 Watcher三者，通过 Observer来监听组件的model数据变化，通过Compile来解析编译模板指令，最终利用Watcher搭起Observer和Compile之间的通信桥梁，达到数据变化 -》视图更新；视图交互变化（input）-》数据model变更的双向绑定效果。

![img](https://s2.loli.net/2022/03/26/jQHGD4ZIMtgXeqU.png)



## 面试回答

**谈谈你对vue的 MVVM响应式原理的理解**。

- Vue是采用数据劫持结合发布者-订阅者模式的方式，通过Object.defineProperty()来劫持各个属性的getter、setter，在数据变动时发布消息给订阅者，然后触发相应的监听回调函数来更新视图。
- 首先需要Observer 对数据进行递归遍历，包括子属性对象的属性，都添加上getter、setter，当读取值或者修改数据时，就会触发getter或setter，就能监听到数据变化。
- compile 解析指令，初始化页面将模板中的变量替换成数据，并将每个指令对应的结点 绑定更新的回调函数，添加订阅者，一旦数据变动，订阅者收到通知，触发回调更新视图。
- watcher订阅者是compile 和 observer之间的桥梁，首先在自身实例化时往dep中添加自己，其次，要有一个update方法更新，最后，数据变动时触发 dep.notice()，调用自身的update方法，触发compile 中绑定的回调函数。
- MVVM 作为数据绑定的入口，整合Observer、compile、watcher三者，通过observer来监听自己的model数据变化，通过compile来解析指令，最终利用watcher搭起Observer和compile之间的通信桥梁，达到 <u>数据变化----视图更新；视图交互变化（input）-----数据model更新的双向绑定效果。</u>

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a512c19873a64397926fac0365ea6544~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp?)

# 三、Vue2.x里的 object.defineProperty()

**循环递归整个对象进行监听**

> 在正常操作下，这个方法不可修改、不可枚举、不可删除。除非设置了true。
>
> defineProperty ---->  定义属性，接收三个参数（要增加的对象，要增加的属性，描述对象），即
>
> `Object.defineproperty(obj, prop , descriptor)`，默认返回的是obj。
>
> 劫持数据---》给对象进行扩展---》属性进行设置。

```javascript
function defineProperty (){
    var _obj = {};
    Object.defineProperty(_obj, 'a' ,{
        //一下都是纯的数据描述，是否可以三改
        value:1,
        writable:true,  //可修改
        enumerable:true,  //可枚举
        configurable:true  //可删除
    })
}
```

### 1. 关于getter 和 setter机制

每定义一个属性的时候 getter setter 机制

```javascript
function defineProperty(){
    var _obj = {};
    var a = 1;
    //同时定义多个属性用es
    Object.defineProperties(_obj,{
        a:{
            get(){
                return '"这是" + a';
            },
            set(newVal){
                a = newVal;
                console.log('the value "a" 被定义为了'+ a);  //这里的a其实就是被newVal赋值了
            }
        },
        b:{}
    })
}

var obj = defineProperty();
obj.a = 2; //调用set方法，将2的值传递给newVal，同时改变变量a的值，输出：the value "a" 被定义为了2
console.log(obj.a);  //调用get方法，获取到a的值，输出：2。
```

> 在描述 descriptor里**不能**同时存在 **writable，enumerable，configurable 和set()、get()共存**

在上述代码中，打印`obj.a`的值输出了2，这是因为我们将 obj.a赋值为了2，调用了set方法，同时也会修改原有定义的a。故当下一次再调用get方法获取数据的时候，会去看看原有的数据有没有被修改，返回的是最新的数据。

### 2. 关于数据劫持

**定义**：数据劫持，指的是访问或者修改对象的某个属性时，通过一段代码拦截这个行为，**进行额外的操作**（比如加上console语句，或者输出一些长句子，向上面这样就是进行了额外的操作）或者修改返回结果。

### 3. 怎样使用Object.defineProperty操作数组？

[Object.defineProperty可以劫持数组吗？](https://juejin.cn/post/7026910654237769735)

> 这里说明一下，Object.defineProperty本身是可以对数组进行监听的，只是在vue中Object.defineProperty不能监听数组变化而已，这主要是因为如果监听数组的变化，那么每次改动开销会很大。**对于数组其实是一个特殊的元素，如果一个个绑定从0 =》 arr.lrngth那就很耗费资源，性能低**
>
> 因此vue对数组的7个变异方法进行了重写，从而来监听数组的变化。除了这八种，其他数组属性都检测不到，比如通过下标方式修改数组数据或者给对象新增属性，这都不能触发组件的重新渲染。

因为Object.defineProperty在vue中不能监听数组的变化，数组的push、pop、shift、unshift、splice、sort、reverse方法不会被触发。

```javascript
function DataArr(){
    var _val = null; //object
    var _arr = [];
    Object.defineProperty(this,'val',{
        get:function(){
            return _var;
        }
        set:function(newVal){
        	_val = newVal;
        	_arr.push({val:_val});
        	console.log('A new value"' + _val + '" has been pushed to _arr')
        }
    });
	this.getArr = function(){
        return _arr;
    }
}

//使用new创建一个对象，这时this就指向创建的这个变量
var dataArr = new DataArr();
dataArr.val = 123;
dataArr.val = 234;
console.log(dataArr.getArr());
```

![image-20220303095147324](https://gitee.com/guoluyan53/image-bed/raw/master/img/image-20220303095147324.png)

这里使用创建对象的形式来改变数组。

#  四、Vue3.0 里的Proxy

> Proxy是代理的意思，其实就是代理一个对象，替这个对象完成想要的功能。
>
> Proxy可以直接处理对象、数组、函数等引用类型的值。

**形式**：`Proxy(target,handler)`

- target：目标对象，你要进行处理的对象
- handler 容器   无数可以处理的对象方法，自定义对象属性的获取，复制，枚举，函数调用等功能。

### 1. 可以直接修改对象

```javascript
var target = {
    a:1,
    b:2
}
let proxy = new Proxy(target,{
    //get接收两个参数，第一个是要改变的对象，第二个是对象属性
    get(target,prop){ 
        return 'this is property value' + target[prop];
    }
    //set接收三个参数，第一个是要改变的对象，第二个是对象属性，第三个是要修改的值
    set(target,prop,value){
    target[prop] = value;
    console.log(target[prop]);
}
});

console.log(proxy.a);  //this is property value 1
console.log(target.a);  //1
proxy.b = 3;       //修改b的值为3
console.log(target);   //{a:1,b:3}
```

### 2. 可以直接修改数组

**所以为什么proxy可以在vue3.0里劫持整个数组呢？**



```javascript
let arr = [
    {name:'小明',age:19},
    {name:'小王',age:29},
    {name:'小化',age:13},
    {name:'小李',age:45},
];
let persons = new Proxy(arr,{
    get(arr,prop){
        return arr[prop];
    },
    set(arr,prop,value){
        arr[prop] = value;
    }
});
console.log(persons[2]);  //{name:'小化',age:13}
persons[1] = {name:'小转换',age:13};
console.log(persons,arr); //是修改后的结果，是一样的
```

### 3. 直接修改函数

```javascript
let fn = function(){
    console.log('i am a function');
}
fn.a = 123;
let newFn = new Proxy(fn,{
    get(fn,prop){
        return fn[prop] + 'this is a Proxy return';
    }
})
console.log(newFn.a)
```

