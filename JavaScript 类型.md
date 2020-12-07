# 7种类型

- Undefined
- Null
- Boolean
- Number
- String
- Object
- Symbol

# Undefined、Null

## 取值

`undefined`类型只有一个值：`undefined`,

`null`类型也只有一个值 ： `null`

## 为什么要使用 void 0 代替 undefined?

**JavaScript 中`undefined`是一个全局变量，而非关键字，也就是说有可能被篡改**，`void`运算符用于将表达式变为`undefined`值

## Null和Undefined的区别

`null`表示定义了但是为空

`undefined` 表示定义但是还末赋值

# Boolean

## 取值

`Boolean`类型只有两个： `true`和`false`

# String

## 取值

`String`有最大长度限制：2^53-1。

<!--因为字符串的操作方法中使用的索引最大值就是2^53-1, JavaScript中整数的最大表示长度就是2^53-1-->

> 现行的字符集国际标准，字符是以 Unicode 的方式表示的，每一个 Unicode 的码点表示一个字符，理论上，Unicode 的范围是无限的。UTF 是 Unicode 的编码方式，规定了码点在计算机中的表示方法，常见的有 UTF16 和 UTF8。 Unicode 的码点通常用 U+??? 来表示，其中 ??? 是十六进制的码点值。 0-65536（U+0000 - U+FFFF）的码点被称为基本字符区域（BMP）。

## String类型的值无法被更改

​		JavaScript 中的字符串是永远无法变更的，一旦字符串构造出来，无法用任何方式改变字符串的内容，所以字符串具有值类型的特征。

## String字符串把每个 UTF16 单元当作一个字符来处理

​		所以处理非 BMP（超出 U+0000 - U+FFFF 范围）的字符时，应该格外小心。

# Number

## 取值

	### 可取值数量

​	18437736874454810627(即 2^64-2^53+3)

### 可取值范围

- `Number.MAX_SAFE_INTEGER`: `9007199254740991 (2^53 - 1)` 
- `Number.MIN_SAFE_INTEGER`: `-9007199254740991`
- `Number.MAX_VALUE`: `1.7976931348623157e+308`
- `Number.MIN_VALUE`: `-1.7976931348623157e+308`

### 最小精度值

- `Number.EPSILON` : `2.220446049250313e-16`

###  在使用IEEE 754 - 2008 规定的双精度浮点数规则基础上，引用以下三个值解决除以0出错：

- **NaN**;  据说占用`9007199254740990 (2^53 - 2)`，但验证失败
- **Infinity**；无穷大
- **-Infinity**; 负无穷大

## +0 与 -0

### 区分方式 

```typescript
1/x // 返回finity为+0，-finity为-0 
```

## IEEE 754双精度浮点数的存储

**双精度浮点数在内存中以「科学计数法」存储，使用64位字节：1符号位+11指数位+52尾数位**

> 科学计数法：`12345.6`以`1.23456e4`表示，`0.000123`以`1.23e-4`表示

### IEEE 754如何表示一个实数

- 每个实数都有一个相反数
  符号位改变下就是一个相反数，但是0的相反数就是自己，在IEEE 754下有+0和-0

- 指数可以负数

  1. 为指数部分也设置一个符号位

  2. 设置一个偏移量，使指数部分永远表现为一个非负数。计算时减去偏移量得到真实的指数

     偏移量的设置： 1023  ` 2^(11-1) - 1`, 11为指数位数

     指数存储值: 0 至 2047 

     指数实际值: 指数存储值 - 1023

     指数取值范围：-1023 至 1024

- 规格化：`1.01010101e2`，这个1默认存在，但不占用存储位

- 非规格化：`0.01e-1023`, 指数为最小值`-1023`了，小数点前也无法做到`1`

### 为什么尾数位只有52位却可以表示2^53个安全的整数？

​		前面有说，所有数字最后都以**「科学计数法」**存储，即`1.xxxxeyyy`的形式的存储，其中`xxxx`就保存在52个尾数位中，小数点前有且只有一个数字`1`，如果不是`1`，就`指数-1`，小数点向右移，直到小数点前为`1`，既然都为`1`，所以省略掉就好了。

​		简单的说，就是`指数位`的作用。

## 精度丢失

### 十进制转二进制时丢失

​		`0.1 0.2 0.4 0.8` 在十进制转二进制时都是无限循环的小数，但是只有52位的有效域，所以会出现`舍入`,`IEEE754`默认舍入到最接近的值，如果`舍`和`入`一样接近，那么取结果为偶数的选择

```typescript
0.1 * 2 = 0.2 ... 0
0.2 * 2 = 0.4 ... 0
0.4 * 2 = 0.8 ... 0 
0.8 * 2 = 1.6 ... 1
0.6 * 2 = 1.2 ... 1
0.2 * 2 = 0.4 ... 0
0.4 * 2 = 0.8 ... 0
0.8 * 2 = 1.6 ... 1
0.6 * 2 = 1.2 ... 1
...
0.1n10 => 0.00011001100110011...n2 => 1.1001100110011001100110011001100110011001100110011010e-4 // 精度丢失
```

### 参与运算时发生对阶导致丢失

​		**对阶**：以加法为例，把小的指数域转化为大的指数域，也就是左移小数点，一旦小数点左移，必然会把52位最右位挤出去，挤出去时会发生**`舍入`**

```typescript
10**-7 // 0.0000001 以1结尾，无限循环小数，精度丢失
// 1.1010110101111111001010011010101111001010111101001000e-24
10**-6 // 0.000001	以1结尾，无限循环小数，精度丢失
// 1.0000110001101111011110100000101101011110110110001101e-20
10**10 // 10000000000 精度不会丢失
// 1.0010101000000101111100100000000000000000000000000000e33

10**10 + 10*-6 == 10**10 // falsel; e-20和e33发生对阶，相差53位，指数小的需要向左移53位，虽然大于52位，但是小数前的1不参数存储的，所以不会全丢弃, 所以变成了
0.0000000000000000000000000000000000000000000000000001e33 +
1.0010101000000101111100100000000000000000000000000000e33 =
1.0010101000000101111100100000000000000000000000000001e33 

10**10 + 10*-7 == 10**10 // true; e-24和e33发生对阶，相差57位，指数小的需要向左移57位, 毫无疑问所有数字已经全丢弃了
0.0000000000000000000000000000000000000000000000000000e33 +
1.0010101000000101111100100000000000000000000000000000e33 =
1.0010101000000101111100100000000000000000000000000000e33 
```

### 数值数量级与精确度数量级的关系

​		在`IEEE754 64位双精度`的表示下，如果一个数的数量级在`10**X`，其精确度在`10**Y`，那么`X`和`Y`大致满足：

```typescript
X-16=Y

// 已知0.1是10**-1数量级，那么精确度在10^-17左右
0.10000000000000000 ==
0.10000000000000001 // true
0.1000000000000000 ==
0.1000000000000001 // false  
```



### 0.1输出时未出现精度丢失

​	0.1在存储时会将十进制转为二进制，会出现舍入

​	0.1在输出时会将二进制转为十进制，再由十进制转为字符串，也会出现舍入

### 实际运算时以存储的64bit为准

### [使用工具查看IEEE754的存储](https://babbage.cs.qc.cuny.edu/IEEE-754.old/Decimal.html)

## 解决精度丢失

### 思路

**将小数转为整数，在整数范围内计算结果**

### 注意点

- 整数超过一定值也会出现精度丢失，安全值在`-2^53-1至2^53-1`之内 

## Number.prototype.toFixed

​		使用定点表示法来格式化一个数值。

​		支持一个可选参数**digits**：小数点后数字的个数；介于 0 到 20 （包括）之间，实现环境可能支持更大范围。如果忽略该参数，则默认为 0。

​		一个数值的字符串表现形式，不使用指数记数法，而是在小数点后有 digits（注：digits具体值取决于传入参数）位数字。该数值在必要时进行四舍五入，另外在必要时会用 0 来填充小数部分，以便小数部分有指定的位数。 如果数值大于 1e+21，该方法会简单调用 [`Number.prototype.toString()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/toString)并返回一个指数记数法格式的字符串。

### 解决方案

```typescript
//注意要传入两个小数的字符串表示，不然在小数转成二进制浮点数的过程中精度就已经损失了
function numAdd(num1/*:String*/, num2/*:String*/) { 
    var baseNum, baseNum1, baseNum2; 
    try { 
        //取得第一个操作数小数点后有几位数字，注意这里的num1是字符串形式的
        baseNum1 = num1.split(".")[1].length; 
    } catch (e) {
        //没有小数点就设为0 
        baseNum1 = 0; 
    } 
    try { 
        //取得第二个操作数小数点后有几位数字
        baseNum2 = num2.split(".")[1].length; 
    } catch (e) { 
        baseNum2 = 0;
    }
    //计算需要 乘上多少数量级 才能把小数转化为整数 
    baseNum = Math.pow(10, Math.max(baseNum1, baseNum2)); 
    //把两个操作数先乘上计算所得数量级转化为整数再计算，结果再除以这个数量级转回小数
    
  	return (num1 * baseNum + num2 * baseNum) / baseNum; // 🔺
  	// 最后返回时，传入的字符串直接参与运算了，被隐式转换为了`number`，导致精度丢失
  	return (num1*baseNum + num2*baseNum).toFixed(0) / baseName;
  	// 虽然toFixed会进入四舍五入，但是四舍五入前值的差距只有+-0.000000001，我们明确地知道分子是一个整数，所以			 舍入后也不会造成影响
};
```

# Symbol

## 创建

```typescript
var mySymbol = Symbol("my symbol");
```

## o[Symbol.iterator]

​		一些标准中提到的 Symbol，可以在全局的 Symbol 函数的属性中找到。例如，我们可以使用 Symbol.iterator 来自定义 for…of 在对象上的行为：

```typescript
var o = new Object

o[Symbol.iterator] = function() {
  var v = 0
  return {
    next: function() {
      return { value: v++, done: v > 10 }
    }
  }        
};

for(var v of o) 
  console.log(v); // 0 1 2 3 ... 9
console.log([...o]) // [1, 2, 3 ..., 9]
console.log(o.map(v=>v)) // SyntaxError, o.map is not a funciton
```

# Object

## JS中的「类」与「类型」

​		在C++ 和 Java 中，每个类都是一个类型，二者几乎等同，而JavaScript 中的“类”仅仅是运行时对象的一个私有属性，而 JavaScript 中是无法自定义类型的。

## 基本类型与对象类型

JavaScript 中的几个基本类型，都在对象类型中有一个“亲戚”。它们是：

- Number
- String
- Boolean
- Symbol

### Number、String、Boolean

		1. 这三个函数在和`new`搭配时，变身为构造函数产生对象实例
  		2. 当直接调用时，表示强制类型转换

### Symbol

Symbol 函数比较特殊，直接用 new 调用它会抛出错误，但它仍然是 Symbol 对象的构造器。

### 'abc' 与 new String('abc')的区别

#### 1. 类型上不同

- `'abc'` 为基本类型
- `new String('abc')`是对象类型，记住任何使用`new`实例化的都是对象

#### 2. 都可使用定义在Number.prototype上的方法

**`.` 运算符提供了装箱操作，它会根据基础类型构造一个临时对象，使得我们能在基础类型上调用对应对象的方法。**

```typescript
String.prototype.getLen = function(){return this.length}
new String('abc').getLen() // 3
'abc'.getLen() // 3
```

# 类型转换

## String To Number

支持带进制转换

```typescript
Number('111') // 111
Number('0b111') Number('0B111') // 7
Number('0o111')	Number('0O111') // 73
Number('0x111')	Number('0X111')	// 273
```

也支持科学计数法转换

```typescript
Number('1e-1')	Number('1E-1') // 0.1
```

**parseInt进制转换时与Number有所差异**

```typescript
parseInt('0x111')	parseInt('0X111')	// 273
parseInt('0o111')	parseInt('0O111')	// 0
parseInt('0111')	parseInt('0111')	// 111 部分古老浏览器支持以0开头的8进制转换
// 请明确使用第二个参数进行转换
parseInt('111', 8)	// 73
```

## Number To String

- 数字量值较少时，直接转换为符合直觉的十进制字符串
- 数字量值较大时，则用科学计数法表示

## 装箱转换

​		每一种基本类型 Number、String、Boolean、Symbol 在对象中都有对应的类，所谓装箱转换，正是把基本类型转换为对应的对象，它是类型转换中一种相当重要的种类。

​		装箱机制会频繁产生临时对象，在一些对性能要求较高的场景下，我们应该尽量避免对基本类型做装箱转换。call 本身会产生装箱操作，所以需要配合 typeof 来区分基本类型还是对象类型。

​		每一类装箱对象皆有私有的 Class 属性，通过`Object.prototype.toString.call()`查看

```typescript
var symbolObject = (function(){ return this; }).call(Symbol('a'))

console.log(typeof symbolObject) //object
console.log(symbolObject instanceof Symbol) //true
console.log(symbolObject.constructor == Symbol) //true
```

## 拆箱转换 ToPrimitive

一言以蔽之，将对象类型转换为基本类型

### ES6之后，可通过[Symbol.toPrimitive]覆盖原有行为

```typescript
var o = { 
  valueOf : () => {console.log("valueOf"); return 32979}, 
  toString : () => {console.log("toString"); return 'abc'},
  [Symbol.toPrimitive](hint){	//必须返回原始类型的值
    console.log("toPrimitive"); 
    switch(hint){
      case 'string':
        return this.toString()
      case 'number':
        return this.valueOf()
      case 'default':
        return 'defaultValue'
      default:
        return 'notMatched'
    }
  }
} 

console.log(o + "")  
// toPrimitive
// hello
```

# Reference 

​	引用类型并非 JavaScript 的基本类型，但是确实存在运行时的一种类型，如成员访问 a.b 返回的就是一个引用，一个 Reference 分为两部分：

- Object
- Key

常见用到 Reference 的地方就是 delete 运算符和赋值运算 assgin 了，基本只有这两种情况是操作 Reference ，其它情况如加减乘除则是将其当作具体的值使用

