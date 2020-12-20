# 面向对象特性

1. 封装
2. 继承
3. 多态，不同对象的行为产生的结果不同

# 面向对象使用场景

1. 开发维护人员比较多时，语义化更好
2. 函数的操作都涉及一个对象

# 工厂模式

**缺点**

1. 无法确定类型
2. 方法重复创建

# 构造函数

​	工厂模式的强化版，new 和 this

# 方法唯一

所有构造函数的原型的原型对象都是 Function.prototype，所有原型对象都有一个属性 constructor ，指向构造函数。constructor 也是 instanceof 的判断依据

```typescript
function Person{}
Person.prototype.eat = function(){}
/*
Person.prototype:{
	constructor: Person,
	eat: anonymous function
	__proto__: Function.prototype
}

new Person:{
	__proto__: Person.prototype
}
*/
// 实例的 __proto__ 指向构造函数的 prototype
```

# 1

1. Child.prototype = Parent.prototype 问题
   Child 无法获取 Parent 的属性，方法是正确获取了
2. 