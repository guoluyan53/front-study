![img](https://gitee.com/guoluyan53/image-bed/raw/master/img/1621500410361-1f8976b5-7b26-4803-b5c3-d0ec8cd819d8.png)

# 一、数据类型

## 1、JavaScript有哪些数据类型，它们的区别？

> JavaScript一个有8种数据类型，分别是undefined、null、Boolean、number、String、Object、**Symbol**、**BigInt**。

其中 `symbol`和 `BigInt`是ES6中新增的数据类型：

- `symbol`代表创建后独一无二且不可变的数据类型，它主要是为了解决可能出现的全局变量冲突的问题。
- `BigInt`是一种数字类型的数据，它可以表示任意精度格式的整数，使用BigInt可以安全地存储和操作大整数，即使这个数已经超出了 Number能够表示的安全整数范围。

这些数据可以分为**原始数据类型**和**引用数据类型**：

- **栈**：原始数据类型（undefined、null、Boolean、number、string）
- **堆** ：引用数据类型（对象、数组和函数）



两种类型的区别在于存储位置不同：

- **原始数据类型**直接存储在栈（stack）中的简单数据段，占据空间小，大小固定，属于被频繁使用数据，所以放入栈中存储数据
- **引用数据类型**存储在堆（heap）中的对象，占据空间大、大小不固定。如果存储在栈中，将会影响程序运行的性能；引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体。

## 2、数据类型检测的方式有哪些？

**1、typeof**

```javascript
console.log(typeof 3);   //number
console.log(typeof []);  //object
console.log(typeof function(){});  //object
console.log(typeof null);    //object
console.log(typeof 'str');    //string
```

其中数组、对象、null都会被判断为object，其他判断都正确。

**2、instanceof**

`instanceof`可以正确判断对象的类型，**其内部运行机制是判断在其原型链中能否找到该类型的原型**。

```javascript
console.log(2 instanceof Number);                    // false
console.log(true instanceof Boolean);                // false 
console.log('str' instanceof String);                // false 
 
console.log([] instanceof Array);                    // true
console.log(function(){} instanceof Function);       // true
console.log({} instanceof Object);                   // true
```

- `instanceof`**只能正确判断引用数据类型**，而不能判断基本数据类型。
- `instanceof`运算符可以用来测试一个对象在其原型链中是否存在一个构造函数的 `prototype`属性。

**3、constructor**

```javascript
console.log((2).constructor === Number); // true
console.log((true).constructor === Boolean); // true
console.log(('str').constructor === String); // true
console.log(([]).constructor === Array); // true
console.log((function() {}).constructor === Function); // true
console.log(({}).constructor === Object); // true
```

`constructor`有两个作用，一是判断数据的类型，二是实例对象通过 `constructor`对象访问它的构造函数。

需要注意，如果创建一个对象来改变它的原型 ，`constructor`就不能用来判断数据类型了：

```javascript
function Fn(){};
 
Fn.prototype = new Array();
 
var f = new Fn();
 
console.log(f.constructor===Fn);    // false
console.log(f.constructor===Array); // true
```

**4、object.prototype.toString.call()**

`Object.prototype.tpString.call()`使用Object对象的原型方法toString来判断数据类型：

```javascript
var a = Object.prototype.toString;
 
console.log(a.call(2));
console.log(a.call(true));
console.log(a.call('str'));
console.log(a.call([]));
console.log(a.call(function(){}));
console.log(a.call({}));
console.log(a.call(undefined));
console.log(a.call(null));
```

同样是检测对象obj调用toString方法，obj.toString()的结果和Object.prototype.toString.call(obj)的结果不一样，这是为什么？

这是因为toSting是object的原型方法，而Array、function等 **类型作为Object的实例，都重写了toString方法**。不同的对象类型调用toString方法时，根据原型链的知识，调用的是对应的重写之后的toString方法（function类型返回内容为函数体的字符串，Array类型返回元素组成的字符串...），而不会去调用Object上原型toString方法（返回对象的具体类型），**所以采用`obj.toString()`不能得到其对象类型，只能将obj转换为字符串类型，因此，在想要得到对象的具体类型时，应该调用Object原型上的toString方法。**

## 3、判断数组的方法有哪些？

**1、通过 object.prototype.toString.call()做判断**

```javascript
Object.prototype.toString.call(obj) === 'Array';
```

**2、通过原型链判断**

```javascript
obj.__proto__ === Array.prototype;
```

**3、通过ES6的Array.isArray()做判断**

```javascript
Array.isArray(obj);
```

**4、通过Array.prototype.isPrototypeOf(obj)**

```javascript
Array.prototype.isPrototypeOf(obj)
```

## 4、null和undefined的区别

- undefined和null都是基本数据类型，分别都只有一个值，就是undefined和null
- undefined代表的含义是 **未定义**，null代表的含义是**空对象**。一般变量声明了但还没有定义的时候会返回undefined，null主要用于赋值给一些可能会返回对象的变量，作为初始化。
- undefined在JavaScript中不是一个保留字，这意味着可以用undefined作为一个变量名，但是这样做非常危险，它会影响对undefined值的判断。我们可以通过一些方法安全的获得undefined值，比如说void 0.
- 当对这两种类型使用typeof进行判断时，null类型会返回“object”，这是一个历史遗留问题，当使用双等号对两种类型的值进行比较时会返回true，使用三个等号时会返回false。

## 5、为什么0.1+0.2！=0.3，如何让其相等

```javascript
let n1 = 0.1,n2 = 0.2;
console.log(n1+n2);   //0.30000000000000004
```

想要等于0.3，就要把它转化：

```javascript
(n1+n2).toFixed(2)   //注意：toFixed为四舍五入
```

计算机是通过二进制的方式存储数据的，所以计算0.1+0.2是计算两个数的二进制的和。这两个数的二进制都是无限循环的数。

一般我们认为数字包括整数和小数，但是在JavaScript中只有一种数字类型 Number，它的实现遵循IEEE754标准，使用64位固定长度来表示，也就是标准的double双精度浮点数。在二进制科学表示法中，双精度浮点数的小数部分最多只能保留52位，再加上前面的1，其实就是保留53位有效数字，剩余的需要舍去，遵从“0舍1入”的原则。

根据这个原则，0.1和0.2的二进制数相加，再转化为十进制数就是：0.30000000000000004

## 6、instanceof操作符的实现原理及实现

`instanceof`运算符用于判断构造函数的prototype属性是否出现在对象的原型链中的任何位置。

```javascript
function myInstanceof(left,right){
    //获取对象的原型
    let proto = Object.getPrototypeOf(left);
   //获取构造函数的prototype对象
    let prototype = right.prototype;
    //判断构造函数的prototype对像是否在对象的原型链上
    while(true){
        if(!proto) return false;
        if(proto === prototype) return true;
        //如果没有找到，就继续从其原型上找，Object.getPrototypeOf方法用来获取指定对象的原型
        proto = Object.getPrototypeOf(proto);
    }
}
```

## 7、如何安全的获取undefined的值？

因为undefined是一个标识符，所以可以被当作变量来使用和赋值，但是这样会影响undefined的正常判断。表达式void__没有返回值，因此返回结果是undefined。

void并不改变表达式的结果，只是让表达式不返回值。因此可以用void 0来获取undefined的值。

```javascript
let a = void 0;
console.log(a);
```

##  8、typeof NaN的结果是什么

`NaN`指“不是一个数字”（not a number），NaN是一个“警戒值”，用于指出数字类型中的错误情况，即“执行属性运算没有成功，这是失败后的返回结果“。

```javascript
typeof NaN;  //"number"
```

NaN是一个特殊值，它和自身不相等，是唯一一个非自反的值，而 NaN！=NaN为true。

## 9、isNaN和Number.isNaN函数的区别？

- 函数isNaN接收参数后，**会尝试将这个参数转换为数值**，任何不能被转换为数值的值都会返回true，因此非数字值传入也会返回true，会影响NaN的判断。
- 函数Number.isNaN会首先判断传入的参数是否为数字，如果是数字再继续判断是否为NaN，不会进行数据类型的转换，这种方法对于NaN的更加准确。

##   10、==操作符的强制类型转换规则

对于 `==`来说，如果对比双方的类型**不一样**，就会进行 **类型转换**。假如对比x和y是否相同，就会进行如下判断流程：

1. 首先会判断两者类型是否相同，相同的话就会比较大小
2. 类型不相同的话，就会进行类型转换
3. 会先判断是否在对比 `null` 和 `undefined`，是的话就会返回 `true`
4. 判断两者类型是否为 `string` 和 `number`，是的话就会将字符串转换为 `number`
5. 判断其中一方是否为 `Boolean`，是的话就会将 `Boolean`转换为 `number`再进行判断
6. 判断其中一方是否为 `object`且另一方为 `string`、`number`或者 `symbol`，是的化就会把 `object`转为原始类型再判断

![img](https://gitee.com/guoluyan53/image-bed/raw/master/img/1615475217180-eabe8060-a66a-425d-ad4c-37c3ca638a68.png)

## 11、Object.is() 与比较操作符 “===” 、“==“ 的区别？

- 使用双等号（==）进行相等判断时，如果两边的类型不一致，则会进行强制类型转换后再进行比较
- 使用三等号（===）进行相等判断时，如果两边类型不一致时，不会做强制类型转换，直接返回false
- Object.is()来进行相等判断，一般情况下和三等号的判断相等相同，它处理了一些特殊情况，比如+0和-0不再相等，两个NaN是相等的。

# 二、ES6

## 1、let、const、var的区别

**（1）作用域不同：**块级作用域由 `{}`包括，let和const具有块级作用域，var不存在块级作用域。块级作用域解决了ES5中的两个问题：

- 内层变量可能覆盖外层变量
- 用来计数的循环变量泄露变为全局变量

**（2）变量提升**：var存在变量提升，let和const不存在变量提升，即在变量只能在声明之后使用，否则会报错。

**（3）给全局添加属性：**var声明的变量为全局变量，并且会将该变量添加为全局对象的属性，但是let和const不会。

**（4）重复声明：**var声明变量时，可以重复声明，后声明的同名变量会覆盖之前声明的变量。const和let不允许重复声明变量。

**（5）暂时性死区**：在使用let、const声明变量之前，该变量都是不可用的，这在语法上，称为暂时性死区。使用var声明的变量不存在暂时性死区。

**（6）初始值设置**：在声明变量时，var和let可以不用设置初始值，但是const声明变量必须设置初始值。

**（7）指针指向**：let和const都是ES6新增的用于创建变量的语法。let创建的变量是可以改变指针指向（可以重新赋值），但是const声明的变量是不允许改变指针的指向。

> 总结来说就是：var声明的变量是全局或者整个函数块的，而let和const声明的变量是块级的变量，var声明的变量存在变量提升，let、const不存在；let声明的变量允许重新赋值，const不允许。

## 2、const对象的属性可以修改吗？

const保证的并不是变量的值不能改动，而是变量指向的那个内存地址不能改动。对于基本类型的数据（数值、字符串、布尔值），其值就保存在变量指向的那个内存地址，因此就等同于常量。

但对于引用类型的数据（主要是对象和数组）来说，变量指向数据的内存地址，保存的只是一个指针，const只能保证这个指针是固定不变的，至于它指向的数据结构是不是可变的，就完全不能控制了。

## 3、如果new一个箭头函数会怎样？

箭头函数是es6中提出来的，它没有prototype，也没有自己的this指向，更不可以适用arguments参数，所以不能new一个箭头函数。

new操作符的步骤如下：

1. 创建一个对象
2. 将构造函数的作用域赋值给新对象（也就是将对象的__ proto __属性指向构造函数的prototype属性）
3. 指向构造函数中的代码，构造函数中的this指向该对象（也就是为这个对象添加属性和方法）
4. 返回新的对象

所以，上面的第二、第三步，箭头函数都是没有办法执行的。

```javascript
let b = ()=> console.log('aaa')
b()
new c = ()=> console.log('ccc')
c()
```

![image-20220205154157632](https://gitee.com/guoluyan53/image-bed/raw/master/img/image-20220205154157632.png)

## 4、箭头函数与普通函数的区别

- 箭头函数更加简洁
- **箭头函数没有自己的this**。箭头函数不会创建自己的this，所以它没有自己的this，他只会在自己作用域的上一层继承this。所以箭头函数中this的指向在它定义时就已经确定了，之后不会改变。
- **继承来的this指向永远不会改变**
- call()、apply()、bind()等方法不能改变箭头函数中this的指向
- 箭头函数不能作为构造函数使用
- 箭头函数没有自己的arguments和prototype
- 箭头函数不能用作Generator函数，不能使用 `yeild`关键字。

## 5、Proxy可以实现什么功能

在vue3.0中通过Proxy来替换原本的Object.defineProperty来实现数据响应式。

Proxy是es6中新增的功能，它可以用来自定义对象中的操作。

```javascript
let p = new Proxy(target,handlder)
```

`target`代表需要添加代理的对象， `handler`用来自定义对象中的操作，比如可以用来自定义 `set`或者 `get`函数

## 6、Symbol

- 不是构造函数，不能通过new创建。而是直接调用Symbol()，返回的变量值永远不变。
- Symbol.for()全局搜索被登记的Symbol中是否有该字符串参数作为名称的Symbol值，如果有即返回该Symbol值，若没有则新建并返回一个以该字符串参数为名称的Symbol值，并登记在全局环境中供搜索。
- Symbol.keyFor（参数为Symbol变量）返回一个已登记的Symbol类型的值的key，用来检测该字符串参数作为名称的Symbol值是否已被登记。。

## 7、Map和Set

- Map是一个映射数据结构

  **API**：Get、Set、Delete、Has（键）、Clear、Size、可以forEach遍历

- Set是一个集合数据结构（自带去重效果）

  **API**：Add、Has、Delete、Clear

## 8、Promise

**定义：**

promise是一种异步编程的解决方案，能够将异步操作封装，并将其返回的结果或者错误原因同Promise实例的then方法传入的处理函数联系起来。

**New Promise**:

传入构造器函数，两个参数，都是敲定函数，第一个成功的敲定，第二个失败的敲定，构造器同步执行，但是可以一步异步的执行敲定函数。

构造器中调用对应的敲定函数，返回的promise实例的对象状态就是那种，当然中间可能会抛出异常throw，那么状态就是拒绝。

状态的变化只有两种pending=> fulfilled,pending => rejected,一旦发生变化就不会再改变。

**Promise.then返回的Promise状态**：

- 非Promise对象，直接包裹称为Promise（Promise.resolve)
- Promise对象，状态跟随，值跟随，通过这一点可以串联Promise
- 报错返回拒绝的Promise，错误对象是Promise的错误原因

**异常穿透**：

如果实例.then方法没有处理实例状态对于的回调函数，那么.then返回的promise实例状态跟随调用then方法的promise，Catch在最后进行异常捕获。

**中断Promise链**：

处理函数返回等待状态的Promise

**判断Promise的执行顺序**：

- 注意，知道前面的Promise状态发生变化时，才能放进微任务队列
- async返回Promise和Promise.then返回Promise都会导致中间有两个层级的微任务队列间隔
- async每有await延后一个层次发生，如果返回promise再加两个层级

```javascript
new Promise((resolve)=>{
    resolve()
}).then(()=>{
    
})

Promise.resolve().then(()=>{
    
})
//这两个Promise的then发生的层级一致
```





