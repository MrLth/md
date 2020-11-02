# 类型判断

## typeof

- 原始类型
  1. `number (含infinity、NaN)` => `'number'`
  2. `boolean (true/false)` => `'boolean'`
  3. `string` => `'string'`
  4. `undefined` => `'undefined'`
  5. `Symbol()` => `'symbol'`
  6. `BigInt()` => `'bigint'`
- 引用类型
  1. `函数名，例Array` => `'function'`
  2. `object` => `'object'`
  3. 🔺 `null` => `'object'`
  4. 🔺 `array` => `'object'` 
  5. 🔺 `其它类型实例，例new Date()` => `'object'`

> `null`: 对应机器码的NULL指针，一般为全零
>
> `undefined`: 用`-2^30`表示

## instanceof

用于检测构造函数的`prototype`属性是否出现在某个实例对象的原型链上，类似`Object.getPrototypeOf(obj) === Consturctor.prototype`，不同的是instaceof会一直向上对比`prototype`, 直到找到或者到`Object.prototype`为止

```typescript
obj instanceof Constuctor // true
obj instanceof Object // true
Constuctor instanceof Object // true
Constuctor.prototype instanceof Object // true
Constuctor.prototype = {}
obj instanceof Constructor // false
Constuctor instanceof Constuctor // false Constuctor.__proto__ : f(){[native code]}
	// Contuctor.__proto__ 是函数身为Function实例的原型对象，继承自Function.protoype
	// Contuctor.prototype 是构造函数特有的原型对象，Contuctor的实例继承的就是Contuctor.prototype
```

```typescript
// 实现
function myInstanceof(left, right){
  const isLeftObj = typeof left === 'object' && left !== null
  if (!isLeftObj) return
  let rightProto = right.prototype,
      leftProto = Object.getPrototypeOf(left)
  while(true){
    if (leftProto === null) return false
    if (leftProto === rightProto) return true
    leftProto = Object.getPrototypeOf(leftProto)
  }
}
```

## Object.prototype.toString

- 原始类型
  1. `number (含infinity、NaN)` => `'[object Number]'`
  2. `boolean (true/false)` => `'[object Boolean]'`
  3. `string` => `'[object String]'`
  4. `undefined` => `'[object Undefined]'`
  5. `Symbol()` => `'[object Symbol]'`
  6. `BigInt()` => `'[object BigInt]'`
- 引用类型
  1. `函数名，例Array` => `'[object Function]'`
  2. `object` => `'[object Object]'`
  3. `null` => `'[object Null]'`
  4. `array` => `'[object Array]'` 
  5. `其它类型实例，例new Date()` => `'[object Date]'`

## 最终方案

- `typeof`判断原始类型时是ok的，但在判断引用类型时精度不够，并且有`null`这个特例
- `intanceof` 用于判断一个实例是否属于一个构造函数
- `Object.prototype.toString`的精度最高

```typescript
let type = obj => 
	typeof obj !== 'object' 
		? typeof obj 
    : Object.prototype.toString.call(obj).slice(8,-1).toLowerCase()
```

# Array.from

```typescript
Array.from = Array.from ?? (arrayLike, mapFn, thisArg) => {
  if (arrayLike === null) throw new TypeError
  if (mapFn !== void 0 && type(mapFn) !== 'function') throw new TypeError	
	// undefined不是保留字，有可能被修改。使用void 0更为保险
  
  mapFn = mapFn ?? function (v){ return v }
  
  const newArr = [],
    		len = arrayLike.length
  for (let i = 0; i < len; i++){
      newArr.push(mapFn.call(thisArg, arrayLike[i]))
  }
  return newArr
}
```

```typescript
Array.from = Array.from ?? (arrayLike, mapFn, thisArg) => {
  if (arrayLike === null) throw new TypeError
  if (mapFn && type(mapFn) !== 'function') throw new TypeError	
  
  const newArr = [],
        len = arrayLike.length
  for (let i = 0; i < len; i++){
    	const v = arrayLike[i]
      newArr.push( mapFn === 'undefined' 
                      ? v
                      : thisArg 
                          ? mapFn.call(thisArg, v)
                          : mapFn(v)
                 )
  }
  return newArr
}
```

# Array.prototype.pop

```typescript
--arr.length
```

# 数组去重

## Set

```typescript
const unit = arr => Array.from(new Set(arr))
```

## DeepUnit

```typescript
const type = obj =>
    typeof obj !== 'object'
        ? typeof obj
        : Object.prototype.toString.call(obj).slice(8, -1).toLowerCase()

const isSame = (a, b)=>{
	if (a === null || typeof a !== 'object') return a===b
    
    const at = type(a),
          bt = type(b)
    
    if (at !== bt) return false
    
    if (at === 'date'){
        if (av.valueOf() !== bv.valueOf()) return false
    }
    
    const ak = Object.keys(a),
          bk = Object.keys(b)
    if (ak.length !== bk.length) return false
    
    for (let i=0, len = ak.length; i<len; i++){
        const k = ak[i]
        if (!isSame(a[k], b[k])) return false
    }
    return true
}

const isExist = (arr, item) => {
	for (const v of arr){
        if (isSame(item, v)) return true
	}
    return false
}

const unitDeep = arr => {
    if (arr.length === 1) return [...arr]
    const newArr = []
    for (const v of arr){
        if (!isExist(newArr, v)) newArr.push(v)
    }
    return newArr
}
```

## 删除原数组重复，返回删除项

```typescript
Array.prototype.distnct = function (){
    if (this.length < 2) return this
    
    const arr = []
    for (let i=0; i<this.length; i++){
        for (let j=1; j<this.length;){
            if (this[j] === this[i]){
                arr.push(this.splice(j,1)[0])
                continue
            }
            j++
        }
    }
    return arr
}
// deep版
Array.prototype.distnct = function (){
    if (this.length < 2) return this
    
    const arr = []
    for (let i=0; i<this.length; i++){
        for (let j=i+1; j<this.length;){
            if (isSame(this[i], this[j])){
                arr.push(this.splice(j,1)[0])
                continue
            }
            j++
        }
    }
    return arr
}
```



# 深克隆

```typescript
const type = obj =>
    typeof obj !== 'object'
        ? typeof obj
        : Object.prototype.toString.call(obj).slice(8, -1).toLowerCase()

const cloneDeep = (obj)=>{
    if (Array.isArray(obj)){
        return obj.map(v=>cloneDeep(v))
    }
    
    if (type(obj) === 'date'){
        return new Date(obj.valueOf())
    }
    
    if (obj !== null && typeof obj === 'object'){
        const newObj = Object.create(Object.getPrototypeOf(obj))
        Object.entries(obj).forEach(([k,v])=> newObj[k] = cloneDeep(v))
        return newObj
    }
    
    return obj
}
```

# String.prototype.indexOf

```typescript
const indexOf = (a,b)=>{
    let i=0, j=0
    const alen = a.length, blen = b.length
    for (;i<alen; i++ ){
        if (a[i] === b[0] && a.substr(i, blen) === b) return i
    }

    return -1
}
```

```typescript
const indexOf = (a,b) => {
	const rst = a.match(b) // regExp
    return rst ? rst.index : -1
}
```

```typescript
const indexOf = (a,b)=>{
	return a.search(b)  // regExp
}
```

# String.prototype.trim

## subString，[)

```typescript
String.prototype.trim = function () {
    if (this.length === 0) return this
    var i, j
        len = this.length,
        whiteSpace = '\n\r\t\f '

    for (i = 0; i < len; i++) {
        if (whiteSpace.indexOf(this.charAt(i)) === -1) {
            break
        }
    }

    for (j = len - 1; j > i; j--) {
        if (whiteSpace.indexOf(this.charAt(j)) === -1) {
            break
        }
    }

    return this.substring(i, j+1)
}
```

## regExp

- 性能更好，更简洁，比上面性能好30%

```typescript
String.prototype.trim = function () {
    return this.replace(/^\s*/, '').replace(/\s*$/, '')
}
```



# 排序

## 快速排序

```typescript
// 短列表性能大致相当，长列表比原生快2.6倍，并且内存占用更小
const quickSort = (arr)=>{
	if (arr.length <= 1 ) return arr
    
    const less = [],
          greater = [],
          [pivot] = arr.splice(Math.floor(arr.length/2), 1)
    
    for (let i=0, len=arr.length; i<len; i++){
        if (arr[i]<pivot)
            less.push(arr[i])
        else
            greater.push(arr[i])
    }
     return quickSort(greater).concat([pivot], quickSort(less))
}
// 优化版，不影响原数组
const quickSort = (arr)=>{
	if (arr.length <= 1 ) return arr
    
    const less = [],
          greater = [],
          arrLen = arr.length,
          pivotIndex = Math.floor(arrLen/2),
          pivot = arr[pivotIndex]
    
    for (let i=0, len=pivotIndex; i<len; i++){
        if (arr[i]<pivot)
            less.push(arr[i])
        else
            greater.push(arr[i])
    }
    for (let i=pivotIndex+1; i<arrLen; i++){
        if (arr[i]<pivot)
            less.push(arr[i])
        else
            greater.push(arr[i])
    }
   
     return quickSort(greater).concat([pivot], quickSort(less))
}
```

# URL Parameter 解析

```typescript
const parseQueryStr = (url) =>{
    const queryStr = url.split('?')[1]
    if (queryStr === undefined || queryStr === '') return 
    
    const parameters = queryStr.split('&'),
          obj = Object.create(null)
    for (const para of parameters){
        const [k, v] = para.split('=')
        obj[k]=v
    }
    return obj
}
```

# 数字千分位

- (?=)先行断言，这里表示匹配的字符后面必须跟着3个数字
- (?!) 正向否定查找，这里表示匹配的字符不能以\b开头

```typescript
 const thousandsSplit = (num) => {
     return String(num).replace(/(?=(?!\b)(\d{3})+$)/g, ',')
 }
```

