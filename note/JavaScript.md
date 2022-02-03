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

- `instanceof`只能正确判断引用数据类型，而不能判断基本数据类型。
- `instanceof`运算符可以用来测试一个对象在其原型链中是否存在一个构造函数的 `prototype`属性。



