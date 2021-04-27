
# let const var 的相关概念

<https://juejin.cn/post/6844903752139276301>

## let const 对比 var 的特点

1. 声明的变量只在声明时的代码块内有效
2. 不存在声明提升
3. 存在暂时性死区，如果在变量声明前使用，会报错
4. 不允许重复声明，重复声明会报错
5. let、const声明的全局变量不会挂在顶层对象下面

## 块级作用域

块级作用域： 在一个代码块（括在一对花括号中的一组语句）中定义的所有变量在代码块的外部是不可见的

es5， Javascript 并不支持块级作用域,就是因为有变量提升

块级声明用于声明在指定块的作用域之外无法访问的变量。

### 形式

1. 函数内部

2. 块中(字符 { 和 } 之间的区域)

## let const 的区别

const 用于声明常量，其值一旦被设定不能再被修改，否则会报错。

值得一提的是：const 声明不允许修改绑定，但允许修改值。这意味着当用 const 声明对象时，不可以重新赋值，但可以改值属性