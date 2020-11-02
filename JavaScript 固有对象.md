# 现有JavaScript标准规定的全局变量

## 3个值

1. **Infinity**
2. **NaN**
3. **undefined**

## 9个函数

1. **eval**
2. **isFinite**
3. **isNaN**
4. **parseFloat**
5. **parseInt**
6. **decodeURI**
7. **decodeURIComponent**
8. **encodeURI**
9. **encodeURIComponent**

## 一些构造器

### 4个基本类型

1. **Number**
2. **String**
3. **Boolean**
4. **Object**
5. `ES6` **Symbol**

### 基本功能及数据结构

1. **Array**
2. **Date**
3. **Function**
4. **RegExp**
5. `ES6` **Promise**
6. `ES6` **Proxy**
7. `ES6` **Map**
8. `ES6` **WeakMap**
9. `ES6` **Set**
10. `ES6` **WeakSet**

### 错误类型

1. **Error**
2. **EvalError**
3. **RangeError**
4. **ReferenceError**
5. **SyntaxError**
6. **TypeError**
7. **URIError**

### 二进制操作

1. **ArrayBuffer**
2. **SharedArrayBuffer**
3. **DataView**

### 带类型的数组

1. **Float32Array**
2. **Float64Arrat**
3. **Int8Array**
4. **Int16Array**
5. **Int32Array**
6. **UInt8Array**
7. **UInt16Array**
8. **UInt32Array**
9. **UInt8ClampedArray**

```typescript

var set = new Set();
var objects = [
    eval,
    isFinite,
    isNaN,
    parseFloat,
    parseInt,
    decodeURI,
    decodeURIComponent,
    encodeURI,
    encodeURIComponent,
    Array,
    Date,
    RegExp,
    Promise,
    Proxy,
    Map,
    WeakMap,
    Set,
    WeakSet,
    Function,
    Boolean,
    String,
    Number,
    Symbol,
    Object,
    Error,
    EvalError,
    RangeError,
    ReferenceError,
    SyntaxError,
    TypeError,
    URIError,
    ArrayBuffer,
    SharedArrayBuffer,
    DataView,
    Float32Array,
    Float64Array,
    Int8Array,
    Int16Array,
    Int32Array,
    Uint8Array,
    Uint16Array,
    Uint32Array,
    Uint8ClampedArray,
    Atomics,
    JSON,
    Math,
    Reflect];
objects.forEach(o => set.add(o));

for(var i = 0; i < objects.length; i++) {
    var o = objects[i]
    for(var p of Object.getOwnPropertyNames(o)) {
        var d = Object.getOwnPropertyDescriptor(o, p)
        if( (d.value !== null && typeof d.value === "object") || (typeof d.value === "function"))
            if(!set.has(d.value))
                set.add(d.value), objects.push(d.value);
        if( d.get )
            if(!set.has(d.get))
                set.add(d.get), objects.push(d.get);
        if( d.set )
            if(!set.has(d.set))
                set.add(d.set), objects.push(d.set);
    }
}
```

