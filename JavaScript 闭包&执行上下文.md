# 闭包 closure

​		es6之前只有函数级作用域，函数执行完后会释放自己的执行环境。闭包就是一个函数中有子函数被保存到了外部。使得父函数的执行环境不被释放，被子函数拥有，这个子函数就形成了闭包

## 环境部分

### 	函数的词法环境（执行上下文的一部分）

### 	标签符列表：函数中用到的未声明的变量

## 表达式部分：函数体

# 执行上下文

## ES3

### 	1. scope 作用域

### 	2. variable object 变量对象

### 	3. this value

## ES5

	### 	1. lexical environment 词法环境

### 	2. variable environment 变量环境

### 	3. this value

## ES2018

### 	1. lexical envireonment 词法环境

### 	2. variable environment 变量环境

### 	3. code evaluation state 用于恢复代码执行位置

### 	4. Function 执行的任务是函数时使用，表示正在被执行的函数

### 	5. ScriptModule 执行的任务是脚本或者模块时使用，表示正在被执行的代码

### 	6. Realm 使用的基础库和内置对象实例

### 	7. Generator 仅生成器上下文有这个属性，表示当前生成器

## var

### 	var声明作用域函数执行的作用域，所以会穿透for、if、with等语句

### IIFE : 使用IIFE限制var的作用范围

```typescript
(function (){var a})()

(function (){var a}())
```

<!--注意括号表达式有个缺点，如果上一行代码不写分号，括号会被解释为上一行代码最末的函数调用，所以部分代码规范会要求IIFE括号前跟一个分号-->

```typescript
;(function(){}())

// 更推荐使用void表达式，当然大部分一元运算符都可
void function (){}()
```

### var 会穿透 with，并且会优于传入对象的属性使用

```typescript

var b;
void function(){
    var env = {b:1};
    b = 2;			// 未报错
    console.log("In function b:", b);	// 2
    with(env) {
        var b = 3;
        console.log("In with b:", b);	// 3
    }
}();
console.log("Global b:", b);	// undefined
```

## let & 块级作用域

​		**let 是 ES6 开始引入的新的变量声明模式，比起 var 的诸多弊病，let 做了非常明确的梳理和规定。为了实现 let，JavaScript 在运行时引入了块级作用域。也就是说，在 let 出现之前，JavaScript 的 if for 等语句皆不产生作用域。**

## Realm

​		**在 ES2016 之前的版本中，标准中甚少提及{}的原型问题。但在实际的前端开发中，通过 iframe 等方式创建多 window 环境并非罕见的操作，所以，这才促成了新概念 Realm 的引入。Realm 中包含一组完整的内置对象，而且是复制关系。**

```typescript

var iframe = document.createElement('iframe')
document.documentElement.appendChild(iframe)
iframe.src="javascript:var b = {};"

var b1 = iframe.contentWindow.b;
var b2 = {};

console.log(typeof b1, typeof b2); //object object

console.log(b1 instanceof Object, b2 instanceof Object); //false true
```

## 函数表达式

​		具有名称的函数表达式"会在外层词法环境和它自己执行产生的词法环境之间产生一个词法环境，再把自己的名称和值当作变量塞进去，所以你这里的b = 20 并没有改变外面的b，而是试图改变一个只读的变量b

```typescript
var b = 10;
(function b(){
	b = 20;
	console.log(b); // [Function: b]
})();
```

