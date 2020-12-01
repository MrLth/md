# 浏览器和 Node 事件循环的区别

[浏览器与Node的事件循环(Event Loop)有何区别?_Fundebug博客-CSDN博客_node事件循环和浏览器事件循环区别](https://blog.csdn.net/Fundebug/article/details/86487117)

​	简单来说，两者的事件处理机制不同，NodeJs 的 timer 执行阶段会将到期的 timer 全部执行，然后再执行微任务，而浏览器是每个宏任务执行完毕后都会执行一次微任务