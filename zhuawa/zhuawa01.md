1. 异步函数（对 promise 的封装），最好加上 Async，比如 createAsync（nodejs 的写法）
2. node 环境中 reject 在 try...catch 不会被捕获和消费
3. 2.3.3 x.then 可以是一个 getter 
4. "thenable" 只要对象或者函数有一个 then 方法，那这个对象就是一个合法的 promise
5. babel 对 async 的封装
6. promise.chain 作业
7. catch 的 区分和执行时32979

