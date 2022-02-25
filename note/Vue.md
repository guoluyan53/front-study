![Vue面试题.png](https://gitee.com/guoluyan53/image-bed/raw/master/img/1621612367141-93b24efc-8b06-4c10-8259-586cd8c6c5d5.png)

# 一、Vue基础

## 4. data为什么是一个函数而不是对象？

> 简单来说就是，因为组件是可以复用的，JS里对象是引用关系，如果组件data是一个对象，那么子组件中的data属性值会互相污染，产生副作用。
>
> 所以一个组件的data选项必须是一个函数，因此每个实例可以维护一份返回对象的独立拷贝。new Vue的实例时不会被复用的，因此不存在以上问题。



Vue组件可能存在多个实例，如果使用对象形式定义data，则会导致它们共用一个data对象，那么状态改变将会影响所有组件实例，这是不合理的；

采用函数形式定义，在initData时会将其作为工厂函数返回全新data对象，有效**规避多实例之间状态污染问题**。而在Vue根实例创建过程中则不存在该限制，也是因为根实例只能有一个，不需要担心这种情况。

## 7. 为什么vue3.0使用proxy，抛弃了object.defineProperty?

`Object.defineProperty`只能劫持对象的属性，因此我们需要对每个对象的每个属性进行遍历。vue2.X里，是通过 递归+遍历 data对象来实现对数据的监控的，如果属性值也是对象那么需要深度遍历，显然如果能劫持一个完整的对象才是更好的选择。

Proxy可以劫持整个对象，并返回一个新的对象。Proxy不仅可以代理对象，还可以代理数组。还可以代理动态增加的属性。

# 二、Vue原理篇

## 1. v-if、v-show、v-html的原理

- `v-if`会调用addCondition方法，生成vnode的时候会忽略对应节点，render的时候就不会渲染；
- `v-show`会生成VNode，render的时候也会渲染成真实节点，只是在render过程中会在节点的属性中修改show属性值，也就是常说的display。
- `v-html`会先移除节点下的所有节点，调用html方法，通过addProp添加innerHTML属性，归根结底还是设置innerHTML为v-html的值。



## 2. vue的基本原理

当一个vue实例创建时，vue会遍历data中的属性，用 `Object.defineProperty`(vue3.0使用proxy)将他们转化为 getter/setter，并且在内部追踪相关依赖，在属性被访问和修改时通知变化。每个组件实例都有相应的watcher程序实例，它会在组件渲染的过程中把属性记录为依赖，之后当依赖项的setter被调用时，会通知watcher重新计算，从而致使它关联的组件得以更新。

![0_tB3MJCzh_cB6i3mS-1.png](https://gitee.com/guoluyan53/image-bed/raw/master/img/1620128979608-f7465ffc-9411-43e3-a6bc-96ab44dd77df.png)

## 3. 双向数据绑定原理

vue.js是采用 **数据劫持**结合 **发布者-订阅者模式**的方式，通过 `Object.defineProperty()`来劫持各个属性的setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。主要分为以下一个步骤：

1. 需要`observe`的数据对象进行递归遍历，包括子属性对象的属性，都加上setter和getter这样的话，给这个对象的某个值赋值，就会触发setter，那么就能监听到了数据变化。

2. `compile`解析模板指令，将模板中的变量替换换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，更新视图。

3. `Watcher`订阅者是 `Observe` 和 `Compile`之间通信的桥梁，主要做的事情是：

   ① 在自身实例化时往属性订阅器（dep）里面添加自己

   ② 自身必须有一个update()方法

   ③ 待属性变动 dep.notice()通知时，能调用自身的update()方法，并触发Compile中绑定的回调，则功成身退

4. MVVM作为数据绑定的入口，整个Observer、Compile和 Watcher三者，通过 Observer来监听组件的model数据变化，通过Compile来解析编译模板指令，最终利用Watcher搭起Observer和Compile之间的通信桥梁，达到数据变化 -》视图更新；视图交互变化（input）-》数据model变更的双向绑定效果。

![img](https://gitee.com/guoluyan53/image-bed/raw/master/img/1618656573096-ebdc520c-5d60-4d12-ad04-5df4ebbb5fe7.png)

# 三、Vue区别篇

## 1. MVVM、MVC、MVP的区别

这三种都是常见的软件架构设计模式，主要通过分离关注点的方式来组织代码结构、优化开发效率。

在开发单页面应用时，往往一个路由页面对应一个脚本文件，所有的页面逻辑都在一个脚本文件里。页面的渲染、数据的获取，对用户事件的响应所有的应用逻辑都混合在一起，这样在开发简单项目时，可能看不出什么问题，如果项目变得复杂，那么整个文件就会变得冗长、混乱，这样对项目开发和后期的项目维护是非常不利的。

### MVC

`MVC`通过分离 `Model`、`View`、`Controller`的方式来组织代码结构。

<u>其中`View`负责页面的显示，`Model`负责存储页面的业务数据以及对应的数据操作</u>。并且 `View`和 `Model`应用了观察者模式，当 `Model`层发生改变的时候它会通知有关 `View`层更新页面。

<u>`Controller`层是 `View`和 `Model`层的纽带</u>，它**主要负责用户与应用的响应操作**，当用户与页面产生交互的时候，`Controller`中的事件触发器就开始工作了，通过调用 `Model`层，来完成对`Model`的修改，然后 `Model`层再去通知 `View`层更新。

![image.png](https://gitee.com/guoluyan53/image-bed/raw/master/img/1603814137582-5a9aa62f-0045-4272-bef0-447dedb25596.png)

### MVVM（Vue使用）

> MVVM分为 Model、View、ViewModel。

- `Model`代表数据模型，数据和业务逻辑都在 `Model`层中定义
- `View`代表UI视图，负责数据的展示；
- `ViewModel`负责监听 `Model`中数据的改变并控制视图的更新，处理用户交互操作（业务逻辑层）

Model和View并无直接关联，而是通过ViewModel来进行联系。**Model和ViewModel之间有着双向数据绑定的联系。因此当Model中的数据改变时会触发View层的刷新，View中由于用户交互操作而改变的数据也会在Model中同步**。

这种模式实现了Model和View的数据的同步更新，因此开发者只需要专注于数据的维护操作即可，而不需要自己操作DOM。

![image.png](https://gitee.com/guoluyan53/image-bed/raw/master/img/1603814104939-8c8ac923-735d-4476-937a-cb1f795ffe84.png)

### MVP

> 即 Model、View、Presenter。

MVP和MVC唯一不同的在于 Presenter和Controller。在MVC中，View层和Model层会耦合在一起，当项目逻辑变得复杂时，可能会造成代码的混乱，并且可能会对代码的复用性造成一些问题。

MVP通过使用Presenter来实现对View层和Model层的解耦。MVC中的Controller只知道Model的接口，因此它没有办法控制View层的更新，MVP中，VIew层的接口暴露给了Presenter，因此可以在Presenter中将Model的变化和View的变化绑定在一起以此来实现View和Model的同步更新。这样就实现了View和Model层的解耦，Presenter还包括了其他的响应逻辑。

## 2. v-if 和 v-show的区别

- **手段**：v-if是动态的向DOM树添加或者删除DOM元素；v-show是通过设置元素的display样式属性控制显隐；
- **编译过程**：v-if切换有一个局部编译/卸载的过程，切换过程中合适地销毁和重建内部的事件监听和子组件；v-show只是简单的基于css切换。
- **编译条件**：v-if是惰性的，如果初始条件为假，则什么也不做；只有在条件第一次变为真时才开始局部编译；v-show是在任何条件下，无论条件是否为真，都被编译，然后被缓存，而且DOM元素保留。
- **性能消耗**：v-if有更高的切换消耗；v-show有更高的初始渲染消耗
- **使用场景**：v-if适合运营条件不大可能改变；v-show适合频繁切换。

## 3. Computed和Watch的区别

**区别**：

- `computed`计算属性：依赖其他属性值，并且computed的值有缓存，只有它依赖的属性值发生改变，下一次获取computed的值才会重新计算computed的值。不支持异步。
- `watch监听器`：更多的是 **观察**的作用，**无缓存性**，类似于某些数据的监听回调，每当监听的数据发生变化时都会执行回调进行后续操作。

**使用场景**：

- 当需要进行数值计算，并且依赖与其他数据时，应该使用 `computed`，因为可以利用computed的缓存特性，避免每次获取值时都要重新计算。
-  当需要在数据变化时执行异步或开销较大的操作时，应该使用watch，使用watch选项允许执行异步操作（访问一个API），限制执行该操作的频率，并在得到最终结果前，设置中间状态。这都是计算属性无法做到的。

# 四、Vue生命周期

# 五、组件通信

# 六、路由

# 七、Vuex

# 八、Vue3.0

# 九、虚拟DOM
