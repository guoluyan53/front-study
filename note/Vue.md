![Vue面试题.png](https://gitee.com/guoluyan53/image-bed/raw/master/img/1621612367141-93b24efc-8b06-4c10-8259-586cd8c6c5d5.png)

# 一、Vue基础

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

## 2. v-if、v-show、v-html的原理

- `v-if`会调用addCondition方法，生成vnode的时候会忽略对应节点，render的时候就不会渲染；
- `v-show`会生成VNode，render的时候也会渲染成真实节点，只是在render过程中会在节点的属性中修改show属性值，也就是常说的display。
- `v-html`会先移除节点下的所有节点，调用html方法，通过addProp添加innerHTML属性，归根结底还是设置innerHTML为v-html的值。

## 3. v-if 和 v-show的区别

- **手段**：v-if是动态的向DOM树添加或者删除DOM元素；v-show是通过设置元素的display样式属性控制显隐；
- **编译过程**：v-if切换有一个局部编译/卸载的过程，切换过程中合适地销毁和重建内部的事件监听和子组件；v-show只是简单的基于css切换。
- **编译条件**：v-if是惰性的，如果初始条件为假，则什么也不做；只有在条件第一次变为真时才开始局部编译；v-show是在任何条件下，无论条件是否为真，都被编译，然后被缓存，而且DOM元素保留。
- **性能消耗**：v-if有更高的切换消耗；v-show有更高的初始渲染消耗
- **使用场景**：v-if适合运营条件不大可能改变；v-show适合频繁切换。

## 4. data为什么是一个函数而不是对象？

Vue组件可能存在多个实例，如果使用对象形式定义data，则会导致它们共用一个data对象，那么状态改变将会影响所有组件实例，这是不合理的；

采用函数形式定义，在initData时会将其作为工厂函数返回全新data对象，有效规避多实例之间状态污染问题。而在Vue根实例创建过程中则不存在该限制，也是因为根实例只能有一个，不需要担心这种情况。

