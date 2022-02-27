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

## 4. $nextTick原理及作用

> Vue的nextTick其本质是对JavaScript执行原理EventLoop（事件循环）的一种应用。

​		nextTick的核心是利用了如promise、MutationObserver、setImmediate、setTimeout的原生JavaScript方法来模拟应对的微/宏任务的实现，本质是为了利用JavaScript的这些异步回调任务队列来实现Vue框架中自己的异步回调队列。

​		**nextTick不仅是vue内部的异步队列的调用方法**，同时也允许开发者在实际项目中使用这个方法来满足实际应用中**对DOM更新数据时机的后续处理**。

### 原理

1. vue用异步队列的方式来控制DOM更新和nextTick回调后执行。
2. 微任务因为以高优先级特性，能确保队列中微任务在一次事件循环前被执行完毕。
3. 考虑兼容问题，vue做了微任务向宏任务的降级方案。



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

## 4. created 和 mounted 的区别

- **created**：在模板渲染成html前调用，即通常初始化某些属性值，然后再渲染成视图。
- **mounted**：在模板渲染成html后调用，通常是初始化页面完成后，再对html的DOM节点进行一些需要的操作。



# 四、Vue生命周期

![无标题](https://gitee.com/guoluyan53/image-bed/raw/master/img/无标题.png)

## 1. 说一下Vue的生命周期

Vue实例有一个完整的生命周期，也就是从开始创建、初始化数据、编译模板、挂载DOM -> 渲染、更新 -> 渲染、卸载等以系列过程，称这为vue的生命周期。

> 注：在这个过程中也会运行一些叫做**生命周期钩子**的函数，这给用户在不同阶段添加自己代码的机会。以下这些就是钩子函数：

1. **beforeCreate（创建前）**：数据观测和初始化事件还没开始，此时data的响应式追踪、event/watcher都还没有被设置，也就是说不能访问到data、computed、watch、methods上的方法和数据。
2. **created（创建后）**：实例创建完成，实例上配置的options包括 data、methods、computed、watch等都配置完成，但是此时渲染节点还未挂载到DOM，所以不能访问到 `$el`属性。
3. **beforeMounted（挂载前）**：在挂载开始之前被调用，相关的render函数首次被调用。实例已完成以下的配置：编译模板、把data里面的数据和模板生成HTML。此时还没有挂载到页面上。
4. **mounted（挂载后）**：在el 被新创建的 vm.$el替换，并挂载到实例上去之后调用。实例已经完成以下的配置：用上面编译好的HTML内容替换el属性指向的DOM对象。完成模板中的html渲染到html页面中。此过程中进行ajax交互。
5. **beforeUpdate（更新前）**：响应式数据更新时调用，此时虽然响应式数据更新了，但是对应的真实DOM还没有被渲染。
6. **updated（更新后）**：由于数据更改导致的虚拟DOM重新渲染和打补丁之后调用。此时DOM已经根据响应式数据的变化更新了。调用时，组件DOM已经更新，所以可以执行依赖于DOM的操作。然而在大多数情况下，应该避免在此期间更改状态，因为这可能会导致更新无限循环。该钩子函数在服务器端渲染期间不被调用。
7. **beforeDestroy（销毁前）**：实例销毁之前调用。这一步，实例仍然完全可以使用， `this`仍能获取到实例。
8. **destroy（销毁后）**：实例销毁后调用，调用后，Vue实例指示的所有东西都会解绑定，所有的事件监听都会被移除，所有的子实例也会被销毁。该钩子函数在服务端渲染期间不被调用。

## 2. Vue子组件和父组件执行顺序

**加载渲染过程**：

1. 父组件 beforeCreate
2. 父组件 created
3. 父组件 beforeMount
4. 子组件 beforeCreate
5. 子组件 created
6. 子组件 beforeMount
7. 子组件 mounted
8. 父组件 mounted

**更新过程**：

1. 父组件 beforeUpdate
2. 子组件 beforeUpdate
3. 子组件 updated
4. 父组件 updated

**销毁过程**：

- 父组件 beforeDestroy
- 子组件 beforeDestroy
- 子组件 destroyed
- 父组件 destroyed

## 3. 一般在哪个生命周期请求异步数据

我们可以在钩子函数 created、beforeMounted、Mounted中进行调用，因为在这三个钩子函数中，data已经创建，可以将服务端返回的数据进行赋值。

推荐在created钩子函数中调用异步请求，因为在created钩子函数中调用异步请求有以下优点：

- 能更快获取到服务端的数据，减少页面加载时间，用户体验更好。
- SSR不支持 beforeMounted、mounted钩子函数，放在created中有助于一致性。

## 4. keep-alive中的生命周期有哪些

> 如果需要在组件切换的时候，保存一些组件的状态防止多次渲染，就可以使用keep-alive组件包裹需要保存的组件。

**keep-alive**是Vue提供的一个内置组件，用来对组件进行缓存------在组件切换过程中将状态保留在内存中，防止重复污染DOM。

如果一个组件包裹了keep-alive，那么它会多出两个生命周期：deactivated、activated。同时，beforeDestroy 和 destroy就不会再被触发了，因为组件不会被真正的销毁。

当组件被换掉时，会被缓存到内存中，触发deactivated 生命周期；当组件被切换回来时，再去缓存里找这个组件、触发activated钩子函数。

# 五、组件通信

# 六、路由

# 七、Vuex

# 八、Vue3.0

# 九、虚拟DOM
