# Execution context

1. 变量环境和词法环境在创建执行上下文时，最初是相同值。即由 function 和 var 声明的会同时保存在变量环境和词法环境中，不同的是变量环境中的值始终不变，词法环境中的值会随着执行发生改变。

> The LexicalEnvironment and VariableEnvironment components of an execution context are always Lexical Environments. When an execution context is created its LexicalEnvironment and VariableEnvironment components initially have the same value. The value of the VariableEnvironment component never changes while the value of the LexicalEnvironment component may change during execution of code within an execution context.
>
> http://www.ecma-international.org/ecma-262/5.1/index.html#sec-10.2
>
> The LexicalEnvironment and VariableEnvironment components of an execution context are always Lexical Environments. When an execution context is created its LexicalEnvironment and VariableEnvironment components initially have the same value.
>
> http://ecma-international.org/ecma-262/6.0/#sec-execution-contexts
>
> ES2020 中移除了此段描述
>
> http://ecma-international.org/ecma-262/#sec-execution-contexts

2. [浏览器工作原理与实践](https://time.geekbang.org/column/article/127495) 中存放在变量环境的 outer 是指向外部执行上下文的，标准中类似概念的是 Environment Records 的 [[OuterEnv]]，意为对外部 Environment Record 的引用。词法环境中的 Outer Reference 指向的也是外部词法环境，而不是执行上下文

