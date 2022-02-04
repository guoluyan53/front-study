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

