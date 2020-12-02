# １ 子组件为何不可修改父组件传递的 Prop 及其实现原理

1. Vue 给出的解释是，每次父组件重新渲染时，都会重写 prop，这不利于状态的保存
2. 如果父组件给 prop 传入一个对象，在子组件修改此对象的属性也会导致父组件数据源的更新。假若给这个对象添加 reactive 特性，甚至可能出现死循环
3. 总的来说，就是让数据单向传递，方便定位错误

**具体实现**

​	**开发环境**时， **`初始化组件实例 => 初始化 state => 初始化 props`** 的时候，给这个 props 添加 reactive 特性（ setter、getter ），set 修改此属性时会进行判断

# ２ 生命周期

**单个组件**

​	beforeCreate => created => beforeMount => mounted

​	beforeUpdate => updated

​	beboreUnmount => unmouted

**父组件和子组件的生命周期 HOOK 执行顺序**

****

1. 父：beforeCreate => created => beforeMount 
2. 子：beforeCreate => created => beforeMount => mounted
3. 父：mounted

****

1. 父：beforeUpdate 
2. 子：beforeUpdate => updated\
3. 父：updated

****

1. 父：beboreUnmount
2. 子：beboreUnmount => unmouted
3. 父：unmouted