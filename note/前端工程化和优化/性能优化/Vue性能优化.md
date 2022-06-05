> 面试中经常会被问到你的Vue项目中做了哪些优化？或者说你知道Vue有哪些性能优化？下面就来说说吧。

- 参考文章：[Vue 项目性能优化 — 实践指南](https://juejin.cn/post/6844903913410314247)

## 前言

Vue中我们需要去考虑项目的首屏优化、webpack编译配置优化等问题，所以我们要去关注Vue项目性能方面的优化，使项目具有更高效的性能、更好的用户体验。

**主要从三个部分进行优化**：

1. Vue代码层面的优化
2. webpack配置层面的优化
3. 基础的web技术层面优化

## 一、代码层面的优化

### 1.1 v-if 和 v-show的使用要区分场景

- v-if适用于在运行时很少改变条件，不需要频繁切换条件的场景
- v-show则适用于需要非常频繁切换条件的场景

### 1.2 computed 和 watch的使用

**computed**：是计算属性，依赖其他属性值，并且computed的值有缓存，只有它依赖的属性值发生改变，下一次获取computed的值时才会重新计算computed的值。

**watch**：更多的是观察的作用，类似于某些数据的监听回调，每当监听的数据变化时都会执行回调进行后续操作。

**【运用场景】**：

- 当我们需要进行数值计算，并且依赖于其他数据时，应该使用computed，因为可以利用computed的缓存特性，避免每次获取值时，都要重新计算。
- 当我们需要在数据变化时执行异步或开销较大的操作时，应该使用watch，使用watch选项允许我们执行异步操作（访问一个API），限制我们执行该操作的频率，并在我们得到最终结果前，设置中间状态。这些都是计算属性无法做到的。

### 1.3 v-for 和 v-if

**（1）v-for 遍历必须为 item 添加 key**

这是因为在列表数据进行遍历渲染时，需要为每一项item设置唯一key值，方便vue内部机制精准找到该条列表数据。当state更新时，新的状态值和旧的状态值对比，较快地定位到diff。

**（2）v-for遍历时避免同时使用v-if**

v-for和 v-if的优先级在版本不同优先级也不同。但是不管怎样，如果每一次都需要遍历整个数组，将会影响速度，尤其是当之需要渲染很小一部分时，必要情况下应该替换成computed属性。

推荐：

```vue
<ul>
  <li
    v-for="user in activeUsers"
    :key="user.id">
    {{ user.name }}
  </li>
</ul>
computed: {
  activeUsers: function () {
    return this.users.filter(function (user) {
	 return user.isActive
    })
  }
}
```

不推荐：

```vue
<ul>
  <li
    v-for="user in users"
    v-if="user.isActive"
    :key="user.id">
    {{ user.name }}
  </li>
</ul>
```

### 1.4 静态数据的优化

因为Vue会通过Object.defineProperty对数据进行劫持，来实现视图响应数据的变化，但是有时候我们的组件就是纯粹的数据展示，数据不会有任何的修改，那么我们就不需要Vue来劫持数据。这样在大量数据展示的情况下，这能够很明显的减少组件初始化的时间。

我们可以通过 `Object.freeze()`方法来冻结一个对象，一旦被冻结的对象就再也不能被修改了。

```vue
export default {
  data: () => ({
    users: {}
  }),
  async created() {
    const users = await axios.get("/api/users");
    this.users = Object.freeze(users);
  }
};
```

### 1.5 事件的销毁

Vue组件销毁时，会自定清理它与其它实例的连接，解绑它的全部指令及事件监听器，但是仅限于组件本身的事件。

如果在js内使用**addEventListene**等方式是不会自动销毁的，我们需要在组件销毁时**手动移除**这些事件的监听，以免造成内存泄露。

```vue
created() {
  addEventListener('click', this.click, false)
},
beforeDestroy() {
  removeEventListener('click', this.click, false)
}
```

### 1.6 图片资源懒加载

对于图片过多的页面，为了加速页面的加载速度，所以很多时候我们需要将页面内未出现在可视区域的图片先不做加载，等到滚动到可视区域后再去加载。

这样对于页面加载性能上会有很大的提升，也提高了用户体验。

**在项目中使用Vue的 `vue-lazyload`插件**：

（1）安装插件

```
npm install vue-lazyload --save-dev
```

（2）在入口文件main.js中引入并使用

```
import VueLazyload from 'vue-lazyload'
```

然后在vue中直接使用

```
Vue.use(VueLazyload)
```

或者添加自定义选项

```
Vue.use(Vuelazyload,{
	preLoad:1.3,
	error:'dist/error.png',
	loading:'dist/loading.gif',
	attempt:1
})
```

（3）在vue文件中将img标签的`src`属性直接改为`v-lazy`，从而将图片显示方式更改为懒加载显示：

```
<img v-lazy="/static/img/1.png">
```

### 1.7 路由懒加载

Vue是单页面应用，可能会有很多的路由引入，这样使用webpack打包后的文件很大，当进入首页时，加载的资源过多，页面会出现白屏的情况，不利于用户体验。

故要对路由进行按需加载

```javascript
const Foo = () => import('./Foo.vue')
const router = new VueRouter({
  routes: [
    { path: '/foo', component: Foo }
  ]
})

```

### 1.8 第三方插件的按需引入

我们在项目中经常会需要引入第三方插件，如果我们直接引入整个插件，会导致项目的体积太大，我们可以借助 `babel-plugin-component`，然后可以只引入需要的组件，已达到减小项目体积的目的。

### 1.9 优化无限列表性能

如果你的应用存在非常长或者无限滚动的列表，那么需要采用窗口化的技术来优化性能，只需要渲染少部分区域的内容，减少重新渲染组件和创建DOM节点的时间。

可以参考考员项目 [vue-virtual-scroll-list](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Ftangbc%2Fvue-virtual-scroll-list) 和 [vue-virtual-scroller](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2FAkryum%2Fvue-virtual-scroller) 来优化这种无限列表的场景的。

### 1.10 服务端渲染 SSR or 预渲染

- 如果项目的SEO和 首屏渲染是评价项目的关键指标，那么你的项目就需要服务端渲染来达到最佳的初始加载性能和SEO（《[Vue SSR 踩坑之旅](https://juejin.cn/post/6844903824956588040)》）
- 如果项目只需要改善少数营销页面的SEO，那么可能需要预渲染，在构建时简单地生成针对特定路由的静态html文件。优点是设置预渲染更简单，并可以将你的前端作为一个完全静态的站点，具体你可以使用 [prerender-spa-plugin](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fchrisvfritz%2Fprerender-spa-plugin) 就可以轻松地添加预渲染 。

## 二、Webpack层面的优化

### 2.1 Webpack对图片进行压缩

在vue项目中除了可以在 `webpack.base.conf.js`中url-loader中设置limit大小来对图片处理，对小于limit的图片转换为base64格式，其余的不做操作。所以对有些较大的图片资源，在请求资源的时候，加载会很慢，我们可以用 `image-webpack-loader`来压缩图片：

（1）首先，安装image-webpack-loader：

```bash
npm install image-webpack-loader --save-dev
```

（2）然后，在 webpack.base.conf.js 中进行配置：

```base
{
  test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
  use:[
    {
    loader: 'url-loader',
    options: {
      limit: 10000,
      name: utils.assetsPath('img/[name].[hash:7].[ext]')
      }
    },
    {
      loader: 'image-webpack-loader',
      options: {
        bypassOnDebug: true,
      }
    }
  ]
}
```

### 2.2 减少ES6 转为ES5 的冗余代码

在默认情况下，babel会在每个输出文件中内嵌这些依赖的辅助函数代码，如果多个源代码文件都依赖这些辅助函数，那么这些辅助函数的代码将会出现很多次，造成代码冗余。为了不让这些辅助函数的代码重复出现，可以在依赖它们时通过`require('babel-runtime/helpers/XXXXX')` 的方式导入，些辅助函数的代码重复出现，

`babel-plugin-transform-runtime` 插件就是用来实现这个作用的，将相关辅助函数进行替换成导入语句，从而减小 babel 编译出来的代码的文件大小。

（1）首先，安装 `babel-plugin-transform-runtime` ：

```
npm install babel-plugin-transform-runtime --save-dev
```

（2）然后，修改 .babelrc 配置文件为：

```
"plugins": [
    "transform-runtime"
]
```

### 2.3 提取公共代码

如果项目中没有去将每个页面的第三方库和公共模块提取出来，则项目会存在以下问题：

- 相同的资源被重复加载，浪费用户的流量和服务器的成本。
- 每个页面需要加载的资源太大，导致网页首屏加载缓慢，影响用户体验。

所以我们需要将多个页面的公共代码抽离成单独的文件，来优化以上问题 。Webpack 内置了专门用于提取多个Chunk 中的公共部分的插件 CommonsChunkPlugin，我们在项目中 CommonsChunkPlugin 的配置如下：

```
// 所有在 package.json 里面依赖的包，都会被打包进 vendor.js 这个文件中。
new webpack.optimize.CommonsChunkPlugin({
  name: 'vendor',
  minChunks: function(module, count) {
    return (
      module.resource &&
      /\.js$/.test(module.resource) &&
      module.resource.indexOf(
        path.join(__dirname, '../node_modules')
      ) === 0
    );
  }
}),
// 抽取出代码模块的映射关系
new webpack.optimize.CommonsChunkPlugin({
  name: 'manifest',
  chunks: ['vendor']
})
复制代码
```

如果要看插件的更多详细内容，可以查看 CommonsChunkPlugin 的 [详细介绍](https://link.juejin.cn?target=https%3A%2F%2Fwebpack.docschina.org%2Fplugins%2Fcommons-chunk-plugin%2F)。

### 2.4 模板预编译

当使用 DOM 内模板或 JavaScript 内的字符串模板时，模板会在运行时被编译为渲染函数。通常情况下这个过程已经足够快了，但对性能敏感的应用还是最好避免这种用法。

预编译模板最简单的方式就是使用[单文件组件](https://link.juejin.cn?target=https%3A%2F%2Fcn.vuejs.org%2Fv2%2Fguide%2Fsingle-file-components.html)——相关的构建设置会自动把预编译处理好，所以构建好的代码已经包含了编译出来的渲染函数而不是原始的模板字符串。

如果你使用 webpack，并且喜欢分离 JavaScript 和模板文件，你可以使用 [vue-template-loader](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fktsn%2Fvue-template-loader)，它也可以在构建过程中把模板文件转换成为 JavaScript 渲染函数。

### 2.5 提取组件的css

当使用单文件组件时，组件内的 CSS 会以 style 标签的方式通过 JavaScript 动态注入。这有一些小小的运行时开销，如果你使用服务端渲染，这会导致一段 “无样式内容闪烁 (fouc) ” 。将所有组件的 CSS 提取到同一个文件可以避免这个问题，也会让 CSS 更好地进行压缩和缓存。

查阅这个构建工具各自的文档来了解更多：

- [webpack + vue-loader](https://link.juejin.cn?target=https%3A%2F%2Fvue-loader.vuejs.org%2Fzh-cn%2Fconfigurations%2Fextract-css.html) ( vue-cli 的 webpack 模板已经预先配置好)
- [Browserify + vueify](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fvuejs%2Fvueify%23css-extraction)
- [Rollup + rollup-plugin-vue](https://link.juejin.cn?target=https%3A%2F%2Fvuejs.github.io%2Frollup-plugin-vue%2F%23%2Fen%2F2.3%2F%3Fid%3Dcustom-handler)

### 2.6、优化 SourceMap

我们在项目进行打包后，会将开发中的多个文件代码打包到一个文件中，并且经过压缩、去掉多余的空格、babel编译化后，最终将编译得到的代码会用于线上环境，那么这样处理后的代码和源代码会有很大的差别，当有 bug的时候，我们只能定位到压缩处理后的代码位置，无法定位到开发环境中的代码，对于开发来说不好调式定位问题，因此 sourceMap 出现了，它就是为了解决不好调式代码问题的。

SourceMap 的可选值如下（+ 号越多，代表速度越快，- 号越多，代表速度越慢, o 代表中等速度 ）



![1.png](https://s2.loli.net/2022/06/04/XJBPSmT7YqLrfp9.webp)



**开发环境推荐： cheap-module-eval-source-map**

**生产环境推荐： cheap-module-source-map**

原因如下：

- **cheap**： 源代码中的列信息是没有任何作用，因此我们打包后的文件不希望包含列相关信息，只有行信息能建立打包前后的依赖关系。因此不管是开发环境或生产环境，我们都希望添加 cheap 的基本类型来忽略打包前后的列信息；
- **module** ：不管是开发环境还是正式环境，我们都希望能定位到bug的源代码具体的位置，比如说某个 Vue 文件报错了，我们希望能定位到具体的 Vue 文件，因此我们也需要 module 配置；
- **soure-map** ：source-map 会为每一个打包后的模块生成独立的 soucemap 文件 ，因此我们需要增加source-map 属性；
- **eval-source-map**：eval 打包代码的速度非常快，因为它不生成 map 文件，但是可以对 eval 组合使用 eval-source-map 使用会将 map 文件以 DataURL 的形式存在打包后的 js 文件中。在正式环境中不要使用 eval-source-map, 因为它会增加文件的大小，但是在开发环境中，可以试用下，因为他们打包的速度很快。

### 2.7、构建结果输出分析

Webpack 输出的代码可读性非常差而且文件非常大，让我们非常头疼。为了更简单、直观地分析输出结果，社区中出现了许多可视化分析工具。这些工具以图形的方式将结果更直观地展示出来，让我们快速了解问题所在。接下来讲解我们在 Vue 项目中用到的分析工具：`webpack-bundle-analyzer` 。

我们在项目中 `webpack.prod.conf.js` 进行配置：

```
if (config.build.bundleAnalyzerReport) {
  var BundleAnalyzerPlugin =   require('webpack-bundle-analyzer').BundleAnalyzerPlugin;
  webpackConfig.plugins.push(new BundleAnalyzerPlugin());
}
复制代码
```

执行  `$ npm run build --report`  后生成分析报告如下：



![1.png](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/8/15/16c924653df09147~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp)



### 2.8、Vue 项目的编译优化

如果你的 Vue 项目使用 Webpack 编译，需要你喝一杯咖啡的时间，那么也许你需要对项目的 Webpack 配置进行优化，提高 Webpack 的构建效率。具体如何进行 Vue 项目的 Webpack 构建优化，可以参考作者的另一篇文章[《 Vue 项目 Webpack 优化实践》](https://juejin.cn/post/6844903745810104328)

## 三、基础的 Web 技术优化

### 3.1、开启 gzip 压缩

gzip 是 GNUzip 的缩写，最早用于 UNIX 系统的文件压缩。HTTP 协议上的 gzip 编码是一种用来改进 web 应用程序性能的技术，web 服务器和客户端（浏览器）必须共同支持 gzip。目前主流的浏览器，Chrome，firefox，IE等都支持该协议。常见的服务器如 Apache，Nginx，IIS 同样支持，gzip 压缩效率非常高，通常可以达到 70% 的压缩率，也就是说，如果你的网页有 30K，压缩之后就变成了 9K 左右

以下我们以服务端使用我们熟悉的 express 为例，开启 gzip 非常简单，相关步骤如下：

- 安装：

```
npm install compression --save
复制代码
```

- 添加代码逻辑：

```
var compression = require('compression');
var app = express();
app.use(compression())
复制代码
```

- 重启服务，观察网络面板里面的 response header，如果看到如下红圈里的字段则表明 gzip 开启成功 ：

  

  ![1.png](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/8/15/16c924653ef48d24~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp)

### 3.2、浏览器缓存

为了提高用户加载页面的速度，对静态资源进行缓存是非常必要的，根据是否需要重新向服务器发起请求来分类，将 HTTP 缓存规则分为两大类（强制缓存，对比缓存），如果对缓存机制还不是了解很清楚的，可以参考作者写的关于 HTTP 缓存的文章[《深入理解HTTP缓存机制及原理》](https://juejin.cn/post/6844903801778864136)，这里不再赘述。

### 3.3、CDN 的使用

浏览器从服务器上下载 CSS、js 和图片等文件时都要和服务器连接，而大部分服务器的带宽有限，如果超过限制，网页就半天反应不过来。而 CDN 可以通过不同的域名来加载文件，从而使下载文件的并发连接数大大增加，且CDN 具有更好的可用性，更低的网络延迟和丢包率 。

### 3.4、使用 Chrome Performance 查找性能瓶颈

Chrome 的 Performance 面板可以录制一段时间内的 js 执行细节及时间。使用 Chrome 开发者工具分析页面性能的步骤如下。

1. 打开 Chrome 开发者工具，切换到 Performance 面板
2. 点击 Record 开始录制
3. 刷新页面或展开某个节点
4. 点击 Stop 停止录制

![1.png](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/8/15/16c924653fbd6f8e~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp)

更多关于 Performance 的内容可以[点击这里查看](https://link.juejin.cn?target=https%3A%2F%2Fwww.cnblogs.com%2Fxiaohuochai%2Fp%2F9182710.html)。

