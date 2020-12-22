1.  二维数组变一维数组

   `a[x][y] => a[x*len+y]`

2. 对于只读的对象，使用 Object.create() 代替 clone

   节省内存空间，缺点是无法遍历，并且克隆层级过多时，读取开销会变大

3. **`void fuction(){}()`**  较为优雅的 IIFE 书写方式

4. 使用 **`Object.assign`** 对函数形参对象设置默认值

5. 数组删除元素时，如果不在意元素间的顺序，将要删除的元素与数组最后一位元素交换，然后删除

****

1. **`Array.from({length:12}).map()`** 循环 12 次

****

1. 使用数组实现**`栈`**代替递归（深度遍历优先）
   使用数组实现**`队列`**代替递归（广度遍历优先）

2. 利用对象是引用类型这一特点，对于结构高度相似的树，可以使用中间变量代替递归

   ```typescript
   let node = this.root
   for (const c of word){
   	if (!node[c])
           node[c] = Object.create(null)
       node = node[c]
   }
   ```

    