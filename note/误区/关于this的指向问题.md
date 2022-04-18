> this的值是在 函数执行时 决定的，不是在 函数定义时决定的！！！

https://www.ruanyifeng.com/blog/2018/06/javascript-this.html

```javascript
class Person{
    constructor(name,age){
        console.log('constructor 里的 this',this);
        this.name = name;
        this.age = age;
    }
    test(){
        console.log('对象方法里的this',this)
    }
    asyncTest(){
        console.log('this',this);
        setTimeout(function(){
            console.log('setTimeout回调中的this',this)
        },0)
    }
}

const zhangsan = new Person('张三',20);
zhangsan.test();
zhangsan.asyncTest();
```

![image-20220416170444215](https://s2.loli.net/2022/04/16/TiF8eMORdPf6s2h.png)

可以看到setTimeOut里函数的this执行的是window。

**如果没有特殊指向，setInterval和setTImeOut里面的this指向都是指向window的**。

所以这就是为什么在写防抖和节流函数的时候定义一个 context保存当前的this，因为如果直接在setTimeOut里使用this，指向的是window。
