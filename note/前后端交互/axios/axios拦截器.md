> 参考文章：
>
> [axios 请求拦截器&响应拦截器（简书）](jianshu.com/p/6e10aaf4688b)
>
> [axios 拦截器（掘金）](https://juejin.cn/post/6976598398359044133)

# axios拦截器

axios的拦截器分为两种：请求拦截器、响应拦截器

- **请求拦截器**：在请求发送前进行必要操作处理，例如添加统一cookie、请求体加验证、设置请求头等，相当于是对接口里相同操作的一个封装。
- **响应拦截器**：响应拦截器也是如此功能，只是在请求得到响应之后，对响应体的一些处理，通常是数据统一处理等，也常来判断登录失效等。

## 一、interceptors拦截器

**请求拦截一般可以做什么**：

1. 比如config中的一些信息不符合服务器的要求
2. 比如每次发送网络请求时，都希望在界面中显示一个请求的图标
3.  某些网络请求（比如的登录token），必须携带一些特殊的信息

```javascript
//创建实例
let instance = axios.create({
    baseURL:'xxxxx',
    timeout:1500
})
//添加请求拦截器
instance.interceptors.request.use(function(config){
    //在发送请求之前做些什么
    return config //做完拦截操作后要给他返回出去，让后面的then可以获取到
},function(error){
    //对请求错误做些什么
    return Promise.reject(error);
})

//添加响应拦截器
instance.interceptors.response.use(function(res){
    //对响应数据做点什么
    return response;
},function(err){
    //对响应错误做点什么
    return Promise.reject(err);
})
```

如果想在稍后移除拦截器，可以这样：

```javascript
const myInterceptor = axios.interceptors.request.use(function(){//...})
axios.interceptors.request.eject(myInterceptor);
```



