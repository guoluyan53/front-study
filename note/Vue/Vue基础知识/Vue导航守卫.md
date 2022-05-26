> [参考文章](https://zhuanlan.zhihu.com/p/54112006)

## 一、什么是导航守卫？

官方解答：

> vue-router 提供的导航守卫主要用来通过跳转或取消的方式守卫导航。

通俗来讲就是，导航守卫就是路由跳转过程中的一些钩子函数。

可以理解为路由跳转其实是一个大的过程，这个大的过程里又分为跳转前中后等细小的过程，在每一个过程中都有一个函数，这个函数能让你操作一些其他的事情的时机，这就是导航守卫。

## 二、导航解析的流程

1. 导航被触发
2. 在失活的组件里调用 `beforeRouteLeave`守卫
3. 调用全局的 `beforeEach` 守卫
4. 在重用的组件里调用 `beforeRouteUpdate` 守卫（2.2+）
5. 在路由配置里调用 `beforeEnter`
6. 解析异步路由组件
7. 在被激活的组件里调用 `beforeRouteEnter`
8. 调用全局的 `beforeResolve` 守卫（2.5+）
9. 导航被确认
10. 调用全局的 `afterEach`钩子
11. 触发DOM更新
12. 调用 `beforeRouteEnter`守卫中传给 `next`的回调函数，创建好的组件实例会作为回调函数的参数传入。

### 钩子函数的执行顺序

1. 全局前置守卫 beforeEach
2. 路由 beforeEnter守卫
3. 组件路由守卫 beforeRouteEnter，此时this并不指向该组件实例
4. 全局解析守卫 beforeResolve
5. 全局后置守卫 afterEach
6. 组件生命周期 beforeCreate
7. 组件生命周期 created
8. 组件生命周期 beforeMounted
9. 组件生命周期 mounted
10. 组件路由守卫 beforeRouteEnter的next回调

## 三、导航守卫分类

### （1）全局的

全局的导航守卫是指路由实例上直接操作的钩子函数，特点是所有路由配置的组件都会触发，也就是说触发路由就会触发这些钩子函数。

**包括：beforeEach、beforeResolve、afterEach三个**

```javascript
const router = new VueRouter({...})

router.beforeEach((to,from,next)=>{
    //...
})                          
```

#### 【beforeEach】

- 在路由跳转前触发，参数包括`to 、from、next`三个
- 这个钩子函数主要作用是用于登录验证，也就是路由还没跳转提前告知，以免跳转了再通知就晚了。

#### 【beforeResolve】

- 这个钩子和beforeEach类似，也是路由跳转前触发，参数也是to、from、next三个。
- 是获取数据或执行任何其他操作（如果用户无法进入页面时你希望避免执行的操作）的理想位置。

> 它和beforeEach的区别是在导航被确认之前，同时在 所有组件内守卫和异步路由组件被解析之后，解析守卫就被调用。

**即在 beforeEach 和 组件内 beforeRouteEnter之后，afterEach之前调用**

#### 【afterEach】

- 和beforeEach相反，他是在路由跳转完成之后触发，参数包括to、from。**没有next**
- 它发生在beforeEach 和 beforeResolve之后，beforeRouteEnter()
- 对于分析、更改页面标题、声明页面等辅助功能以及许多其他事情都很有用。

### （2）路由独享的守卫

是指在单个路由配置的时候也可以设置的钩子函数

可以直接在路由配置上定义 `beforeEnter`守卫：

```javascript
const routes = [
    {
        path:'/user/:id',
        component: User,
        beforeEnter:(to,from)=>{
            return false;
        }
    }
]
```

#### 【beforeEnter】

`beforeEnter`守卫 **只在进入路由时触发**，不会在 `params`、`query`或 `hash`改变时触发。例如，从 `/users/2` 进入到 `/users/3` 或者从 `/users/2#info` 进入到 `/users/2#projects`。它们只有在 **从一个不同的** 路由导航时，才会被触发。

> `beforeEnter`和 `beforeEach`完全相同，如果都设置则在beforeEach之后紧随执行，参数to、from、next。

### （3）组件内的守卫

是指在组件内执行的钩子函数，类似于组件的生命周期函数，相当于为配置路由的组件添加生命周期钩子函数。

钩子函数的执行顺序包括：

1. beforeRouteEnter
2. beforeRouteUpdate
3. beforeRouteLeave

```vue
export default{
	data(){//...},
	beforeRouteEnter(to,from,next){
		//在渲染该组件的对应路由被 confirm前调用
		//不能 获取组件实例 this
		//因为当前守卫执行时，组件实例还没被创建！
	},
	beforeRouteUpdate(to,from){
		//在当前路由改变，但是该组件被复用时调用
		//举例来说，对于一个带有动态参数的路径`/user/:id`,在`/user/1`和 `/user/2`之间跳转时，
		//由于会渲染同样的`UserDetails`组件，因此组件实例会被复用。而这个钩子就会在这个情况下调用
		//在这个情况发生的时候，组件已经挂载好了，导航守卫可以访问组件实例`this`
	},
	beforeRouteLeave(to,from){
		//在导航离开渲染该组件的对应路由时调用
		//与 `beforeRouteUpdate`一样，它可以访问到组件实例 this
	}
}
```

#### 【beforeRouteEnter】

`beforeRouteEnter`守卫不能访问 this，不过，可以通过传一个 next来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数：

```javascript
beforeRouteEnter (to, from, next) {
  next(vm => {
    // 通过 `vm` 访问组件实例
  })
}
```

## 四、例子

[例子参考](https://www.jianshu.com/p/691379025334)