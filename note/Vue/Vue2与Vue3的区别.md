# Vue2和Vue3的区别

> [参考文章--掘金](https://juejin.cn/post/7067413380922867725)

## 1. 生命周期

生命周期中变化不大，只是大部分生命周期钩子名称上加上了 “on”，功能上是类似的。

不过需要注意的是：

- Vue3在组合式API（Composition API）中使用生命周期钩子时需要先引入
- 而Vue2在选项API（Options API）中可以直接使用生命周期钩子函数

```vue
<script setup>
import {onMounted} from 'vue';  //使用前需要引入生命周期钩子
onMounted(()=>{
    //....
})
//可将不同的逻辑拆开成多个onMounted,依然按顺序执行，不会被覆盖
onMounted(()=>{
    //...
});
</script>
<script>
export default{
    mounted(){//直接调用生命周期钩子函数
        //...
    }
}
</script>
```

**生命周期钩子函数的对比**：

| Vue2          | Vue3             |
| ------------- | ---------------- |
| beforeCreate  |                  |
| Created       |                  |
| beforeMount   | onBeforeMount    |
| Mounted       | onMounted        |
| beforeUpdate  | onBeforeUpdate   |
| Updated       | onUpdated        |
| beforeDestroy | onBeforeUnmounte |
| destroyed     | onUnmounted      |

> tip：setup是围绕beforeCreate 和 created生命周期钩子运行的，所以不需要显式地去定义。

## 2. 多根节点

在vue2中，在模板中如果使用多个根节点时会报错：

```vue
//下面是会报错的
<template>
	<header></header>
	<main></main>
	<footer></footer>
</template>
```

但是，在Vue3中是支持多个根节点的，也就是fragment。

## 3. Composition API（组合API）

- Vue2是选项API（Options API），一个逻辑会散乱在文件不同位置（data，props，computed，watch，生命周期钩子），导致代码的可读性变差。当需要修改某个逻辑时，需要上下来回跳转文件位置。
- Vue3组合式API（Composition API）则很好地解决这个问题，可将统一逻辑的内容写到一起，增强了代码的可读性、内聚性，其还提供了较为完美的逻辑复用性方案。

## 4. 异步组件（Suspense）

Vue3提供Suspense组件，允许程序在等待异步组件加载完成前渲染兜底的内容，如loading，使用户的体验更平滑。（也就是在异步组件加载前，可以给用户看默认的东西）

使用它，需要在模板中声明，并包括两个命名插槽：default 和 fallback。Suspense确保加载完异步内容时显示默认插槽，并将fallback插槽用作加载状态。

```html
<template>
	<suspense>
    	<template #default>
        	<List/>
        </template>
        <template #fallback>
        	<div>
                Loading...
            </div>
        </template>
    </suspense>
</template>
```

在List组件（有可能是异步组件，也有可能是组件内部处理逻辑或查找操作过多导致加载过慢等）未加载完成前，显示Loading...（即fallback插槽内容），加载完成时显示自身（即default插槽内容）

## 5. Teleport

Vue3提供Teleport组件可将部分DOM移动到Vue app之外的位置，比如项目中常见的Dialog 弹窗。

```html
<button @click='dialogVisible = true'>显示弹窗</button>
<teleport to="body">
	<div class="dialog" v-if="dialogVisible">
        我是弹窗，我直接移动到了body标签下
    </div>
</teleport>
```

## 6. 响应式原理

- Vue2响应式原理是 Object.defineProperty
  - 无法监听对象或数组的新增、删除元素
  - 所以Vue2针对常用数组原型方法push、pop、shift、unshift、splice、sort、reverse进行了重写
  - 提供Vue.set监听对象或数组新增属性
- Vue3响应式原理基础是Proxy
  - 可以监听对象或数组的新增，删除
  - 监测.length修改
  - map、set、WeakMap、WeakState都支持

## 7. 虚拟DOM

Vue3 相比于 Vue2，虚拟DOM上增加 patchFlag字段。

## 8. 事件缓存

Vue3 的cacheHandler 可以在第一次渲染后缓存我们的事件。相比于Vue2无需每次渲染都传递一个新函数。加一个click事件。

## 9. Diff算法优化

## 10. 打包优化

## 11. TypeScript支持



