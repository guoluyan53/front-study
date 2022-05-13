> [Vue为何采用异步渲染](https://www.cnblogs.com/WindrunnerMax/p/14429426.html#:~:text=%E5%AF%B9%E4%BA%8E%20Vue%20%E4%B8%BA%E4%BD%95%E9%87%87%E7%94%A8%E5%BC%82%E6%AD%A5%E6%B8%B2%E6%9F%93%EF%BC%8C%E7%AE%80%E5%8D%95%E6%9D%A5%E8%AF%B4%E5%B0%B1%E6%98%AF%E4%B8%BA%E4%BA%86%E6%8F%90%E5%8D%87%E6%80%A7%E8%83%BD%EF%BC%8C%E5%9B%A0%E4%B8%BA%E4%B8%8D%E9%87%87%E7%94%A8%E5%BC%82%E6%AD%A5%E6%9B%B4%E6%96%B0%EF%BC%8C%E5%9C%A8%E6%AF%8F%E6%AC%A1%E6%9B%B4%E6%96%B0%E6%95%B0%E6%8D%AE%E9%83%BD%E4%BC%9A%E5%AF%B9%E5%BD%93%E5%89%8D%E7%BB%84%E4%BB%B6%E8%BF%9B%E8%A1%8C%E9%87%8D%E6%96%B0%E6%B8%B2%E6%9F%93%EF%BC%8C%E4%B8%BA%E4%BA%86%E6%80%A7%E8%83%BD%E8%80%83%E8%99%91%EF%BC%8C%20Vue%20%E4%BC%9A%E5%9C%A8%E6%9C%AC%E8%BD%AE%E6%95%B0%E6%8D%AE%E6%9B%B4%E6%96%B0%E5%90%8E%EF%BC%8C%E5%86%8D%E5%8E%BB%E5%BC%82%E6%AD%A5%E6%9B%B4%E6%96%B0%E8%A7%86%E5%9B%BE%EF%BC%8C%E4%B8%BE%E4%B8%AA%E4%BE%8B%E5%AD%90%EF%BC%8C%E8%AE%A9%E6%88%91%E4%BB%AC%E5%9C%A8%E4%B8%80%E4%B8%AA%E6%96%B9%E6%B3%95%E5%86%85%E9%87%8D%E5%A4%8D%E6%9B%B4%E6%96%B0%E4%B8%80%E4%B8%AA%E5%80%BC%E3%80%82.,%E4%BA%8B%E5%AE%9E%E4%B8%8A%EF%BC%8C%E6%88%91%E4%BB%AC%E7%9C%9F%E6%AD%A3%E6%83%B3%E8%A6%81%E7%9A%84%E5%85%B6%E5%AE%9E%E5%8F%AA%E6%98%AF%E6%9C%80%E5%90%8E%E4%B8%80%E6%AC%A1%E6%9B%B4%E6%96%B0%E8%80%8C%E5%B7%B2%EF%BC%8C%E4%B9%9F%E5%B0%B1%E6%98%AF%E8%AF%B4%E5%89%8D%E4%B8%89%E6%AC%A1%20DOM%20%E6%9B%B4%E6%96%B0%E9%83%BD%E6%98%AF%E5%8F%AF%E4%BB%A5%E7%9C%81%E7%95%A5%E7%9A%84%EF%BC%8C%E6%88%91%E4%BB%AC%E5%8F%AA%E9%9C%80%E8%A6%81%E7%AD%89%E6%89%80%E6%9C%89%E7%8A%B6%E6%80%81%E9%83%BD%E4%BF%AE%E6%94%B9%E5%A5%BD%E4%BA%86%E4%B9%8B%E5%90%8E%E5%86%8D%E8%BF%9B%E8%A1%8C%E6%B8%B2%E6%9F%93%E5%B0%B1%E5%8F%AF%E4%BB%A5%E5%87%8F%E5%B0%91%E4%B8%80%E4%BA%9B%E6%80%A7%E8%83%BD%E6%8D%9F%E8%80%97%E3%80%82.%20%E5%AF%B9%E4%BA%8E%E6%B8%B2%E6%9F%93%E6%96%B9%E9%9D%A2%E7%9A%84%E9%97%AE%E9%A2%98%E6%98%AF%E5%BE%88%E6%98%8E%E7%A1%AE%E7%9A%84%EF%BC%8C%E6%9C%80%E7%BB%88%E5%8F%AA%E6%B8%B2%E6%9F%93%E4%B8%80%E6%AC%A1%E8%82%AF%E5%AE%9A%E6%AF%94%E4%BF%AE%E6%94%B9%E4%B9%8B%E5%90%8E%E5%8D%B3%E6%B8%B2%E6%9F%93%E6%89%80%E8%80%97%E8%B4%B9%E7%9A%84%E6%80%A7%E8%83%BD%E5%B0%91%EF%BC%8C%E5%9C%A8%E8%BF%99%E9%87%8C%E6%88%91%E4%BB%AC%E8%BF%98%E9%9C%80%E8%A6%81%E8%80%83%E8%99%91%E4%B8%80%E4%B8%8B%E5%BC%82%E6%AD%A5%E6%9B%B4%E6%96%B0%E9%98%9F%E5%88%97%E7%9A%84%E7%9B%B8%E5%85%B3%E9%97%AE%E9%A2%98%EF%BC%8C%E5%81%87%E8%AE%BE%E6%88%91%E4%BB%AC%E7%8E%B0%E5%9C%A8%E6%98%AF%E8%BF%9B%E8%A1%8C%E4%BA%86%E7%9B%B8%E5%85%B3%E5%A4%84%E7%90%86%E4%BD%BF%E5%BE%97%E6%AF%8F%E6%AC%A1%E6%9B%B4%E6%96%B0%E6%95%B0%E6%8D%AE%E5%8F%AA%E8%BF%9B%E8%A1%8C%E4%B8%80%E6%AC%A1%E7%9C%9F%E5%AE%9E%20DOM%20%E6%B8%B2%E6%9F%93%EF%BC%8C%E6%9D%A5%E8%AE%A9%E6%88%91%E4%BB%AC%E8%80%83%E8%99%91%E5%BC%82%E6%AD%A5%E6%9B%B4%E6%96%B0%E9%98%9F%E5%88%97%E7%9A%84%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96%E3%80%82.)

## 为什么vue是异步渲染？

vue在更新DOM时是异步执行的，只要侦听到数据变化，vue将开启一个队列，并缓冲在同一事件循环中发生的所有数据变更，如果一个`watcher`被触发多次，只会被推入到队列中一次，这种在缓冲时去除重复数据对于避免不必要的计算的DOM操作是非常重要的，然后，在下一个的事件循环`tick`中，Vue刷新队列并执行实际工作，`Vue`在内部对异步队列尝试使用原生的`Promise.then`、`MutationObserver`和`setImmediate`，如果执行环境不支持，则会采用`setTimeout(fn, 0)`代替。

### 描述

vue采用异步渲染，简单来说就是为了**提升性能**，因为不采用异步更新，在每次更新数据都会对当前组件进行重新渲染，为了性能考虑，Vue会在本轮数据更新后，再去异步更新视图。



## $nextTick

nextTick的核心是利用了如promise、MutationObserver、setImmediate、setTimeout的原生JavaScript方法来模拟应对的微/宏任务的实现，本质是为了利用JavaScript的这些异步回调任务队列来实现Vue框架中自己的异步回调队列。

​		**nextTick不仅是vue内部的异步队列的调用方法**，同时也允许开发者在实际项目中使用这个方法来满足实际应用中**对DOM更新数据时机的后续处理**。

### 原理

1. vue用异步队列的方式来控制DOM更新和nextTick回调后执行。
2. 微任务因为以高优先级特性，能确保队列中微任务在一次事件循环前被执行完毕。
3. 考虑兼容问题，vue做了微任务向宏任务的降级方案。

### 需求例子

假设此时我们有一个需求，需要在页面渲染完成后取得页面的DOM元素，而由于渲染是异步的，我们不能直接在定义的方法中同步取得这个值，于是就有了 `vm.$nextTick`方法，在vue中 `$nextTick`方法将回调延迟到下一次DOM更新循环之后，也就是在下次DOM更新循环结束之后执行延迟回调，在修改数据之后立即使用这个方法，能够获取更新之后的DOM。

简答来说就是当数据更新时，在DOM中渲染完成之后，执行回调函数。

```javascript
<!DOCTYPE html>
<html>
<head>
    <title>Vue</title>
</head>
<body>
    <div id="app"></div>
</body>
<script src="https://cdn.bootcss.com/vue/2.4.2/vue.js"></script>
<script type="text/javascript">
    var vm = new Vue({
        el: '#app',
        data: {
            msg: 'Vue'
        },
        template:`
            <div>
                <div ref="msgElement">{{msg}}</div>
                <button @click="updateMsg">updateMsg</button>
            </div>
        `,
        methods:{
            updateMsg: function(){
                this.msg = "Update";
                console.log("DOM未更新：", this.$refs.msgElement.innerHTML)
                this.$nextTick(() => {
                    console.log("DOM已更新：", this.$refs.msgElement.innerHTML)
                })
            }
        },
        
    })
</script>
</html>
```

这就能解释以前遇到的一些渲染更新的问题了。妙啊。