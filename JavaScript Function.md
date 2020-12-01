# 分类

## 1. Function 声明的函数

```typescript
function foo(){
}
```

## 2. => 箭头函数

- 箭头函数没有 this，函数体中的 this 为外部上下文的 this
- 箭头函数也没有 prototype 属性，所以无法使用 new （无法作为构造函数）
- 不可以使用 arguments 对象
- 不可以使用 yield 关键字，因此也不能作为 generator 函数

```typescript
const foo = () => {}
```

## 3. class定义的类也是函数

```typescript
class C{
	constructor(){}
}
```

## 4. class中定义的方法函数

```typescript
class C{
	foo(){}
}
```

## 5. function * 生成器函数

```typescript
function *  foo(){
}
```

## 6. async 异步函数

```typescript
async function foo(){}

const foo = async () => {}

async function * foo(){}
```



# 执行上下文

​		在JavaScript标准中，为函数规定了用来保存定义时上下文的私有环境 **`[[Environment]]`**

​		当一个函数执行时，会创建一个新的执行环境记录，记录的外层词法环境会被设置成函数的`[[Environment]]`，这一过程被称为 **`切换上下文`**

```typescript
// a.js
const { foo } = require('./b');
var a = 1
foo()

// b.js
var b = 2
export function foo(){
	console.log(b) // b
	console.log(a) // error
}
```

# this

## [[thisMode]]

​		this 则是一个更为复杂的机制，JavaScript 标准定义了 **`[[thisMode]]`** 私有属性

### 	取值

#### 		1. lexical

​				表示从上下文中寻找this，对应箭头函数

#### 		2. global

​				表示当this为undefined时，取全局对象，对应普通函数

#### 		3. strict

​				 严格模式时使用，this严格按照调用时传入的值，可能为null或undefined
​				注意：**class 设计成了默认按 strict 模式执行**

```typescript
"use strict"
function showThis(){
    console.log(this);
}

var o = {
    showThis: showThis
}

showThis(); // undefined; 非严格模式下为global或者window
o.showThis(); // o
```

```typescript
class C {
    showThis() {
        console.log(this);
    }
}
var o = new C();
var showThis = o.showThis;

showThis(); // undefined
o.showThis(); // o
```

## [[ThisBindingStatus]]

​		函数创建新的执行上下文中的词法环境记录时，会根据`[[thisMode]]`的值来标记新纪录的`[[ThisBindingStatus]]`私有属性

​		代码执行遇到 this 时，会逐层检查当前词法环境记录中的[[ThisBindingStatus]]，当找到有 this 的环境记录时获取 this 的值。

## Reference类型

​		我们获取函数的表达式，它实际上返回的并非函数本身，而是一个 **`Reference`**类型，`reference`类型由两部分组成：**一个对象和一个属性值**，比如 **`obj.fn()`**，对象即`obj`，属性值即`fn`

​		当做一些算术运算（或者其他运算时），Reference 类型会被**解引用**，即获取真正的值（被引用的内容）来参与运算，而类似函数调用、delete 等操作，都需要用到 Reference 类型中的对象。

```typescript
const obj = {fn(){return this}}
obj.fn() // obj
// 解引用
(false || obj.fn)() // window 
```

## 指向总结

1. 由new调用， 绑定到新创建的对象
2. 由call或者apply(或者bind)调用，绑定到指定的对象
3. 由上下文对象调用， 绑定到那个上下文对象
4. 默认:在严格模式下绑定到undefined，否则绑定到全局对象
5. 例外：箭头函数不适用以上四条规则，它会继承外层函数调用的 this 绑定(无论 this 绑定到什么)



# IIFE

​	Imdiately Invoked Function Expression  立即执行函数表达式

```typescript
(function b() {
    console.log(b, b.name)
    b = 123
    console.log(b, b.name)
})()
console.log(b)

// Function b(){...} 'b'
// Function b(){...} 'b'
// Throw Error
```

1. 函数表达式与函数声明不同，函数名只在该函数内部有效，并且此绑定是常量绑定。
2. 对于一个常量进行赋值，在 strict 模式下会报错，非 strict 模式下静默失败。
3. IIFE中的函数是函数表达式，而不是函数声明。所以 IIFE 执行后不会留下什么，给 IIFE 中的函数加上函数名没有任何意义。

# 有条件地创建函数

​	函数可以被有条件地声明，但不同浏览器会有不同的效果，对于大部分浏览器有条件地创建函数同样存在声明提升，但是不会赋值（类似于 var 的声明提升）。

​	**任何情况都不应使用这种方式，甚至不使用 function 声明函数，改用箭头函数或者函数表达式，使之更符合直觉**

> 只有 safari 还会在声明提升时赋值

```typescript
function a() {
    debugger
    console.log(b)
    if (true) {
        function b() {
            console.log('b')
        }
    }
    console.log(b)
}
a()
```

