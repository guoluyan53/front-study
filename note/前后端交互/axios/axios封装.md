## 1.创建axios实例

```javascript
const instance = axios.create({
    baseURL:'http://abc.com',
    timeout: 5000
});

instance({
    url:'/home/'
}).then(res=>{
    .....
})
```

## 2. 封装

在实际开发中，最好将发送请求的axios单独提出来进行一个封装。假设哪天这个框架不维护了，还能够很简单的去改变。

### （1）第一种封装（比较好理解）

这个比较好理解，但是没有很好的使用到promise的优势.

新建一个js文件：

```javascript
import axios from 'axios';
//这里success 和 failure都是一个函数
export function request(config,success,failure){
    //1.创建axios的实例
    const instance = axios.create({
        baseURL:'http://123.3.3.3:8080',
        timeout:5000
    });
    
    //发送真正的网络请求
    instance(config)
    .then(res=>{
        success(res)
    })
    .catch(err=>{
        failure(err);
    })
}
```

使用：

```javascript
import {request} from 'XXXXX'
request({
    url:'/home/hhh'
},res=>{
    console.log(res);
},err=>{
    console.log(err);
})
```

### （2）第二种封装（推荐使用）

充分利用了promise的优势

新建一个js文件：

```javascript
export function request(config){
    return new Promise((resolve,reject)=>{
        //1.创建axios实例
        const instance = axios.create({
            baseURL:'http://123.3.34.2:8080',
            timeout: 5000
        });
        //发送真正的网络请求
        instance(config)
        .then(res=>{
            resolve(res);
        })
        .catch(err=>{
            reject(err);
        })
    })
}
```

使用：

```javascript
import {request} from 'XXX'
request({
    url:'/home/usr'
}).then(res=>{
    console.log(res);
}).catch(err=>{
    console.log(err);
})
```

### （3）第三种方式（更简单）

因为创建的axios实例它本身返回的就是一个promise

```javascript
export function request(config){
    //1.创建axios实例
    const instance = axios.create({
        baseURL:'http://122.23.32.3:8080',
        timeout: 5000
    });
    //发送真正的网络请求
    return instance(config)
}
```

使用

```javascript
import {request} from 'XXX'
request({
    url:'/home/usr'
}).then(res=>{
    console.log(res);
}).catch(err=>{
    console.log(err);
})
```

