> 参考文章：[vue的computed实现原理](https://segmentfault.com/a/1190000022169550)
>
> [Vue原理：Computed](https://zhuanlan.zhihu.com/p/357250216)

## 前言

Computed 计算属性是 Vue 中常用的一个功能，在使用中发现他像是介于`data`和`method`之间的一个属性。他可以像是`data`一样在模板中`<span>{{message}}</span>`一样去使用，也可以通过`this.message`来调用；同时他也像`method`一样可以去处理数据逻辑。

## 关于computed缓存

computed计算属性：依赖其它属性值，并且computed的值有缓存，只有依赖的属性值发生改变，下一次获取computed的值时才会重新计算computed的值

computed的基本特性是：
1、当data数据变更时，computed会更新
2、当computed中没有引入的数据更新时，computed不会去重新计算，大幅度节省计算量

## 原理

### 流程：

（1）每个computed属性都会生成对应的观察者（watcher实例），观察者存在value属性和get方法。computed属性的getter函数会在get方法中调用，并将返回赋值给value。初始设置dirty和lazy的值为true，lazy为true不会立即get方法（懒执行），而是会在读取computed值时执行。

（2）将computed属性添加到组件实例上，并通过get、set获取或者设置属性值，并且重新定义getter函数。

（3）页面初始渲染时，读取computed属性值，触发重定义后的getter函数，由于观察者的dirty值为true，将会调用get方法，执行原始getter函数。getter函数中会读取data（响应式）数据，读取数据时会触发data的getter方法，会将computed属性对应的观察者添加到data的依赖收集器dep中（用于data变更时通知更新）。

观察者的get方法执行完成后，更新观察者的value值，并将dirty设置为false，表示value值已更新，之后在执行观察者的depend方法，将上层观察者（该观察者包含页面更新的方法，方法中读取了computed属性值）也添加到getter函数中data的依赖收集器dep中（getter 中的 data 的依赖器收集器包含 computed 对应的观察者，以及包含页面更新方法（调用了 computed 属性）的观察者），最后返回 computed 观察者的 value 值。

![computed1.jpg](https://segmentfault.com/img/bVbFbA5)

（4）当更改了 computed 属性 getter 函数依赖的 data 值时，将会根据之前依赖收集的观察者，依次调用观察者的 update 方法，先调用 computed 观察者的 update 方法，由于 lazy 为 true，将会设置观察者的 dirty 为 true，表示 computed 属性 getter 函数依赖的 data 值发生变化，但不调用观察者的 get 方法更新 value 值。再调用包含页面更新方法的观察者的 update 方法，在更新页面时会读取 computed 属性值，触发重定义的 getter 函数，此时由于 computed 属性的观察者 dirty 为 true，调用该观察者的 get 方法，更新 value 值，并返回，完成页面的渲染。

![computed2.jpg](https://segmentfault.com/img/bVbFbtt)

（5）dirty 值初始为 true，即首次读取 computed 属性值时，根据 setter 计算属性值，并保存在观察者 value 上，然后设置 dirty 值为 false。之后读取 computed 属性值时，dirty 值为 false，不调用 setter 重新计算值，而是直接返回观察者的 value，也就是上一次计算值。只有当 computed 属性 setter 函数依赖的 data 发生变化时，才设置 dirty 为 true，即下一次读取 computed 属性值时调用 setter 重新计算。也就是说，computed 属性依赖的 data 不发生变化时，不会调用 setter 函数重新计算值，而是读取上一次计算值。

### computed如何控制缓存？

通过**【脏数据标志位dirty】**，dirty是watcher的一个属性。

- 当dirty为true时，读取computed会执行get函数，重新计算（表示依赖的值发生了改变）
- 当dirty为false时，读取computed会使用缓存。

**缓存机制简述**

1. 一开始每个 `computed` 新建自己的 `watcher`时，会设置 watcher.dirty = true，以便于 `computed` 被使用时，会计算得到值
2. 当依赖的数据变化了，通知 `computed` 时，会赋值 watcher.dirty = true，此时重新读取 `computed` 时，会执行 `get` 函数重新计算。
3. `computed` 计算完成之后，会设置 watcher.dirty = false，以便于其他地方再次读取时，使用缓存，免于计算。

## 使用例子

```vue
<template>
    <div>
    	<p>{{number}}</p>
        <p>{{double1}}</p>
        <input v-model="double2">
    </div>
</template>
<script>
export default{
    data(){
        return {
            number:5,
            testNum:100
        }
    },
    computed:{
        double1(){
            console.log('double1 运行了');
            return this.number*2;
        },
        double2:{
            get(){
                return this.number*2;
            },
            set(value){
                this.number = value / 2;
            }
        }
    }
}
</script>
```

