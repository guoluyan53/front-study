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



