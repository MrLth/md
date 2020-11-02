# 对象的创建

```typescript
// 1. 利用字面量
var a = [], b = {}, c = /abc/g
// 2. 利用dom api
var d = document.createElement('p')
// 3. 利用JavaScript内置对象的api
var e = Object.create(null)
var f = Object.assign({k1:3, k2:8}, {k3: 9})
var g = JSON.parse('{}')
// 4.利用装箱转换
var h = Object(undefined), i = Object(null), k = Object(1), l = Object('abc'), m = Object(true)
```

# PropertyDescriptor

## 数据属性

1. value; 属性的值
2. writable; 是否可写
3. enumerable; 是否可枚举
4. configurable; 能否被删除和修改描述属性

## 访问器属性

1. getter; 函数或者`undefined`, 取属性时调用
2. setter; 函数或者`undefined`, 在设置属性时调用
3. enumerable; 是否可枚举
4. configurable; 能否被删除和修改描述属性

## Object.getOwnPropertyDescriptor

```typescript
let obj = {
	a:1
}
Object.getOwnPropertyDescriptor(obj, 'a')
// {value: 1, writable: true, enumerable: true, configurable: true}
```

## Object.defineProperty

```typescript
let obj = {
	a:1
}
// 可直接定义一个新的属性
Object.defineProperty(obj, 'b', {
  value:2,
  writable:false,
  enumerable:false,
  configurable:false
})
obj.b = 3
console.log(obj.b)
```

## Getter和Setter

```typescript
let obj = {
	val:1,
	get a(){
		return this.val
	},
	set a(newVal){
		this.val = newVal
	}
}

Object.defineProperty(obj, 'b', {
  get(){
    return this.val
  },
  set(newVal){
    this.val = newVal
  },
  enumerable: false,
  configurable: false
})
```

# 私有属性

## [[prototype]]

​		原型对象，也就是`__proto__`，使**用`Object.getPrototypeOf`**获取，指向构造函数的`prototyoe`对象

### 访问

- **new**
- **Object.create**
- **Object.getPrototypeOf**
- **Object.setPrototypeOf**

new是比其它出现得更早的操作`[[prototype]]`的方式，此外还有非标准属性`__proto__`可以也可以访问

## [[class]]

> [[class]]和Symbol.toStringTag实质上是控制的“ the creation of the default string description of an object”

### 访问

​		唯一可以访问`[[class]]`属性的方式是的使用**`Object.prototype.toString`**

### 含义

​		在**`ES5`**以前版本的 JavaScript 中，“类”的定义是一个私有属性 `[[class]]`，语言标准为内置类型诸如` Number、String、Date `等指定了`[[class]]`属性，以表示它们的类。

## Symbol.toStringTag

​		在**`ES6`**开始，`[[class]]` 私有属性被 `Symbol.toStringTag` 代替，可以使用它完成自定义的类的类型验证

```typescript
function cls(){return this}
Object.prototype.toString.call(new clas) // '[object Object]'
Object.defineProperty(cls.prototype, Symbol.toStringTag, {
	get:()=>'a'
})
Object.prototype.toString.call(new clas) // '[objdect a]'
```

```typescript
class cls{
  get [Symbol.toStringTag](){
    return 'a'
  }
}
Object.prototype.toString.call(new clas) // '[object a]'
```

```typescript
var o = { [Symbol.toStringTag]: "MyObject" };
console.log(o + ""); // '[object MyObject]'
```

# [ES6]class

​		ES6 中引入了 class 关键字，并且在标准中删除了所有[[class]]相关的私有属性描述，类的概念正式从属性升级成语言的基础设施，从此，基于类的编程方式成为了 JavaScript 的官方编程范式。

# 对象的分类

## 宿主对象

### 宿主：浏览器 | node.js

​		简单解释就是由宿主环境（例如浏览器）提供的对象，比如全局对象 **`window`**的上的属性，一部分来自`javascript`，一部分来自浏览器环境

> JavaScript 标准中规定了全局对象属性，W3C 的各种标准中规定了 Window 对象的其它属性。
>

## 内置对象

### 固有对象

​		固有对象是由标准规定，随着 JavaScript 运行时创建而自动创建的对象实例，在任何 JavaScript 代码执行前就已经被创建出来了，它们通常扮演者类似基础库的角色。

### 原生对象

​		我们把 JavaScript 中，能够通过语言本身的构造器创建的对象称作原生对象。在 JavaScript 标准中，提供了 30 多个构造器。

![6cb1df319bbc7c7f948acfdb9ffd99d0](/Users/mrlth/Desktop/6cb1df319bbc7c7f948acfdb9ffd99d0.png)

​		通过这些构造器，我们可以用 new 运算创建新的对象，所以我们把这些对象称作原生对象。**几乎所有这些构造器的能力都是无法用纯 JavaScript 代码实现的，它们也无法用 class/extend 语法来继承。**

​		这些构造器创建的对象多数使用了私有字段, 例如：

- **Error**:`[[ErrorData]]`
- **Boolean**: `[[BooleanData]]`
- **Number**:` [[NumberData]]`
- **Date**: `[[DateValue]]`
- **RegExp**: `[[RegExpMatcher]]`
- **Symbol**: `[[SymbolData]]`
- **Map**: `[[MapData]]`

​	这些字段使得原型继承方法无法正常工作，所以，我们可以认为，所有这些原生对象都是为了特定能力或者性能，而设计出来的“特权对象”。

## 函数对象&构造器对象

### [[call]]

​		任何对象只需要实现`[[call]]`，它就是一个函数对象，可以去作为函数被调用

### [[construct]]

​		实现`[[construct]]`，它就是一个构造器对象，可以作为构造器被调用

1. 用`function`关键字创建的函数必定同时是函数和构造器

   - 宿主对象和内置对象的`[[call]`和`[[constuctor]]`的表现大多不一致

     1. `new Date()` 和 `Date()`表现不一致。前者返回`Date对象`，后台返回`string`

        ```typescript
        new Date() // Sat Sep 12 2020 15:10:08 GMT+0800 (中国标准时间)
        Date() // 'Sat Sep 12 2020 15:10:08 GMT+0800 (中国标准时间)'
        ```

     2. `Number、String、Boolean`，`[[call]]`时是强制类型转换，`[[constructor]]`会创建基本类型对象

        ```typescript
        Number('123') // 123
        new Number('123') // Number {123}
        ```

     3. 浏览器宿主环境提供的`Image`对象，使用函数方式调用时会报错

        ```typescript
        console.log(new Image()) // <img>
        console.log(Image())	// TypeError: failed to construct 'Image'
        ```

   - 而使用`function`关键字声明或者`Function`构造器创建的函数的表现基本一致，不同的是`[[coonstructor]]`总是会返回一个对象

     ```typescript
     function fn(){return this}
     fn() // window
     new fn() // {}
     
     function fn1(){return 1}
     fn() // 1
     new fn() // {}
     
     function fn3(){return {a:1}}
     fn() // {a:1}
     new fn() // {a:1}
     
     // 私有变量
     function fn4(){
     	this.a = 1
       return {
         value(){retur this.a}
       }
     }
     new fn4() // {value()}
     fn4() // {value()}
     ```

2. 用`()=>`创建的函数只会是函数，无法当作构造器使用，因为它没有this

## 特殊行为对象

### Array

​		Array的`length`根据最大下标自动发生改变

### Object.prototype

​		作为所有对象的**`根对象`**，无法再为其添加原型

### String

​		为了支持下标运算，String的正整数下标会去字符串中查找

### Arguments

​		函数的形参列表在非严格模式并且没有使用ES6的形参默认值和剩余参数的情况下，会自动和对应的形参绑定，随之更新

### Moudle NameSpace

​		特殊的地方非常多，跟一般对象完全不一样，尽量只用于 import 吧

### 类型数组与数组缓存区

​		与内存块相关联，拥有特殊的下标运算

### bind过的function

​		与原来的函数相关联





