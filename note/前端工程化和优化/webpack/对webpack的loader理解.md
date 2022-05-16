# 说说webpack中常见的Loader？解决了什么问题？

![img](https://s2.loli.net/2022/05/15/w9F5IoznYGVORK3.png)

## 一、Loader是什么

`loader`用于对模块的“源代码”进行转换，在 `import`或 “加载”模块时预处理文件。

`webpack`做的事情，仅仅是分析出各种模块的依赖关系，然后形成资源列表，最终打包生成到指定的文件中。如下图所示：

![img](https://s2.loli.net/2022/05/15/35F79IYjhTe6kbl.png)

在 `webpack`内部中，任何文件都是模块，不仅仅只是 js 文件

在默认情况下，在遇到 `import`或者 `require`加载模块的时候， `webpack`只支持对 `js`或者 `json`文件打包。

想 `css`、`sass`、`png`等这些类型的文件的时候，`webpack `则无能为力，这时候就需要配置对应的 `loader`

进行文件内容的解析。

在加载模块的时候，执行顺序如下：

![img](https://s2.loli.net/2022/05/15/zyKuchMxNHfdESF.png)

当 `webpack`碰到不识别的模块的时候，`webpack`会在配置中查找该文件解析规则。

关于配置 `loader`的方式有三种：

- 配置方式（推荐）：在webpack.config.js文件中指定loader
- 内联方式：在每个import语句中显示指定loader
- CLI方式：在shell命令中指定他们

### 配置方式

关于 `loader`的配置，我们是写在 `module.rules`属性中，属性介绍如下：

- `rules`是一个数组的形式，因此我们可以配置多个 `loader`
- 每一个 `loader`对应一个对象的形式，对象属性 `test`为匹配的规则，一般情况为正则表达式
- 属性 `use`针对匹配到文件类型，调用对应的 `loader`进行处理

代码编写，如下形式：

```javascript
module.exports = {
    module:{
        rules:[
            {
                test: /\.css$/,
                use:[
                    {loader: 'style-loader'},
                    {
                        loader: 'css-loader',
                        options:{
                            modules:true
                        }
                    },
                    {loader: 'sass-loader'}
                ]
            }
        ]
    }
};
```

## 二、loader的特性

从上面代码可以看到，在处理 css 模块的时候， `use`属性中配置了三个 `loader`分别处理 `css`文件。

因为 `loader`支持链式调用，链中的每个 `loader`会处理之前已处理过的资源，最终变为 js 代码。**顺序为相反的顺序执行**，即上述执行方式为 `sass-loader`、`css-loader`、`style-loader`

**除此之外，loader的特性还有如下**：

- loader可以是同步的，也可以是异步的
- loader运行在Nodejs中，并且能够执行任何操作
- 除了常见的通过 `package.json`的 `main`来将一个 npm模块导出为loader，还可以在 module.rules中使用 `loader`字段直接引用一个模块
- 插件（plugin）可以为loader带来更多特性
- loader能够产生额外的任意文件

## 三、常见的loader

在页面开发过程中，我们经常性加载除了js文件以外的内容，这时我们就需要配置响应的 `loader`进行加载

常见的 loader如下：

- `style-loader`：将css添加到DOM的内联样式标签style里
- `css-loader`：允许将css文件通过require的方式引入，并返回css代码
- `less-loader`：处理less
- `sass-loader`：处理sass
- `postcss-loader`：用postcss来处理css
- `file-loader`：分发文件到output目录并返回相对路径
- `url-loader`：和file-loader类似，但是当文件小于设定的limit时可以返回一个 Data URL
- `html-minify-loader`：压缩HTML
- `babel-loader`：用babel来转换ES6文件到ES

