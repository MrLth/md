1. commonJS
   导入
   	const a = require()
   导出
   	module.export = anything

   解决

   1. 解决变量名冲突
   2. 使用函数和对象实现导入导出（浅复制，默认为 {}），便于运行时环境的兼容
   3. 同步读取，只执行一次
   4. nodeJs 专属 

2. AMD Asynchoronous Module Definition 
   导入

   ​	require([...depModuleIdList], (...ModuleList)=>{})
   定义

   1. define([...depModuleIdList],  (...ModuleList)=>{})
   2. define(fn)

   使用需要加载库
      require.js

   特点

   1. 使用函数实现导入导出，便于运行时环境的兼容
   2. 异步加载
   3. 目前已经少见
   4. 动态插入 script 标签
   5. 浏览器端专属

3. UMD Universal Module Definition
   同时支持 AMD 和 commonJS
   **手写 UMD**

4. CMD，国人开发的规范，和 AMD 类似，用户很少

5. ESModule

   1. ES6 出现
   2. import、export
   3. 由 JS 解释器实现，以上 4 种是在宿主环境中实现
   4. import {a as b} from 'c'，使用别名和对象有区别
      对象 ： const {a:b = 1} = c
   5. import * as all from 'c'，全部导入
   6. import a from 'c'，导入默认导出

6. 尽量使用 ESModule，对于多宿主环境使用 Babel 编译工具实现兼容
   babel 会将 import 编译为 commandJs 或者 AMD 规范，webpack 再将这些代码编译为可以在具体宿主环境运行的模块

7. nodeJs 的 console.log 是异步，浏览器的 console.log 是同步

8. 编译工具（Babel） 解决不同 JS 版本间的语义的问题，主要处理 AST
   打包工具（Webpack） 解决不同环境的模块化的区别，主要处理模块化的兼容性

9. browserify 模块可以让 commondJs 可以在浏览器环境运行

10. **测试 Esmodule 的导出会不会动态更新** 

11. nodeJs 提供 vm 解析字符串代码

12. promise 同时执行两个，然后链式调用
    [![rmYNJs.png](https://s3.ax1x.com/2020/12/13/rmYNJs.png)](https://imgchr.com/i/rmYNJs)

