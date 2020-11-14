# node filename.js 

​	直接执行 js 文件

# 基础代码

```typescript
const koa = request('koa')
const app = new Koa()
app.use(async (ctx, next)=>{
  	await next()
  	ctx.response.type = 'text/html'
  	ctx.response.body = '<h1>hello world</h1>'
})
app.listen(3000, ()=>{
  console.log('server is running at http://127.0.0.1:3000')
})
```

# Context

​	Koa 将 NodeJs 的 request 和 response 对象封装到 context 对象中，可以把 context 当作一次会话的上下文。通过加工 context 对象，可以控制返回给用户的内容。

​	context 内容了一些属性，如 `context.cookies`、`context.state`、`context.app`、`context.throw`，还可以自定义属性以便全局使用

​	可以使用 this 访问 context，当然箭头函数是没有 this 的，很可能更改的是外部的 this 的指向

**基本**

1. ctx.host
2. ctx.url

## 1. ctx.request

1. ctx.request.url
2. ctx.request.query，处理过的查询字符串，为一个对象 
3. ctx.request.queryString，原始查询字符串
4. ctx.request.accept()，判断用户期望的数据类型
5. ctx.request.is()，检查响应类型是否是所提供的类型之一

## 2. ctx.response

1. ctx.response.status，状态码
2. ctx.response.type，响应头的 Content-type 字段，常用 image/png，text/html，默认 text/plain
3. ctx.response.body，响应体
4. ctx.response.redirect(url, [alt]), 重定向 302

## 3. ctx.state

​	是推荐的命名空间，用于通过中间件传递信息和前端视图，类似于 koa-view 这些渲染 view 层的中间件也会默认把 ctx.state 里面的属性作为 view 的上下文传入

## 4. ctx.cookies

1. ctx.cookies.set(name, value, [options])
2. ctx.cookies.get(name, [option])

### 4.1 cookie options

1. MaxAge，最大存活时间
2. signed，cookie 签名值 
3. expires，过期时间
4. path，默认 /
5. domain
6. secure
7. httpOnly
8. overwrite 是否覆盖以前设置的同名 cookie 默认 false

## 5. ctx.throw

​	ctx.throw 用于抛出错误，把错误信息返回给用户，比如 `ctx.throw(500)`会在页面上看到一个状态码为 500 的错误页 ‘Internal Server Error’

# 中间件

​	中间件就是一个带有 ctx 和 next 这两个参数的特殊的函数，next 就是把中间件的执行权交给下游的中间件。使用 app.use() 加载

```typescript
const logger = async (ctx, next)=>{
	console.log(ctx.method, ctx.host + ctx.url)
	await next()
}

app.use(logger)
app.use(async (ctx, next)=>{
	ctx.body = 'holle world'
})
```

中间件的执行流程是一个典型的洋葱模型

<img src="https://segmentfault.com/img/bV6D5Z?w=470&h=411" style="zoom: 67%;" />

## 1. 洋葱模型实现原理

1. app.use 将中间件函数加入 **中间件数组** 中
2. 使用 koa-compose 对中间件数组加工，创建一个闭包，维护一个 index 表示当前执行的中间件函数的索引，最后返回一个回调函数
3. 这个回调函数执行时就将 ctx 和下一个中间件函数作为参数带入

```typescript
function compose (middleware) {
  return function (context, next) {
    // last called middleware #
    let index = -1
    return dispatch(0)
    
    function dispatch (i) {
      if (i <= index) return Promise.reject(new Error('next() called multiple times'))
      index = i
      let fn = middleware[i]
      if (i === middleware.length) fn = next
      if (!fn) return Promise.resolve()
      try {
        return Promise.resolve(fn(context, function next () {
          return dispatch(i + 1)
        }))
      } catch (err) {
        return Promise.reject(err)
      }
    }
  }
}
```

## 2. 多个中间件组合为一个中间件

​	使用 koa-compose 即可

```typescript
app.use(compose([middleware1,middleware2,middleware3]))
```

## 3. 常用中间件

### 3.1 koa-bodyparser

​	将 POST 请求数据写入到 ctx.request.body 中

```typescript
const bodyParser = require('koa-bodyparser')
app.use(bodyParser())
app.use(async (ctx)=>{
	if (ctx.url === '/' && ctx.method === 'POST'){
		console.log(ctx.request.body)
	}
})
```

### 3.2 koa-router

**基础用法**

```typescript
const koa = require('koa')
const bodyparser = require('koa-bodyparser')
const Router = requer('koa-router')

const router = new Router()
const app = new koa()

router.get('/', (ctx,next)=>{})
router.post('/', (ctx, next)=>{})

app.use(bodyparser())
	.use(router.routes())
	.use(router.allowedMethods()) // 异常状态码的处理
	.listen(300)
```

**router.all**

```typescript
// all 不验证方法，只要路径匹配就执行，一般用于添加请求头
router.all('/',(ctx,next)=>{
	ctx.set('Access-Control-Allow-Origin', "http://www.mrlth.com")
})
```

**命名路由**

```typescript
// 命名路由
router.get('user', '/user/:id', (ctx,next)=>{})
// 生成 uri
router.url('user', 3) // 				"/user/3"
router.url('user', {id:3}) // 	"/user/3"
// 重定向
app.user(async (ctx, next)=>{
	router.redirect( ctx.router.url('user') )  
})
```

**多中间件**

```typescript
// 多中间件，步骤分离
router.get('/', (ctx, next)=>{}, (ctx, next)=>{})
```

**嵌套路由**

```typescript 
const user = new Router()
const info = new Router()

info.get('/name', ()=>{})
	.post('/name', ()=>{})
	.get('/password', ()=>{})

user.get('/login')
	.use('/info', info.routes(), info.allowedMethods())

app.use(user.routes())
	.use(user.allowedMethods())
```

**路由前缀**

```typescript
// 为一组路由添加统一的前缀，和嵌套路由类似，但嵌套路由还可以配合中间件实现权限控制
let router = new Router({prefix:'/user'})
router.get('/') // 匹配 "/user"
router.get('/:id') // 匹配 "/user/:id"
```

**URL 参数**

​	koa-router 支持 URL 参数，该参数会被添加到 **ctx.params** 中，参数也可以是一个正则表达式（由 path-to-regexp 实现）

```typescript
router.get('\:category\:title', (ctx, next)=>{
  // 请求 ‘/world/holle’
  console.log(ctx.params) // {category: 'world', title: 'holle'}
})
```



### 3.3 koa-static 和 koa-view

​	koa-static 用于加载静态资源文件，通过它可以为页面请求加载 CSS、JavaScript 等静态资源文件

​	koa-view 用于加载 HTML 模板文件

```typescript
const views = require('koa-view')
app.use(views(__dirname+'/views', {
  map: {html:ejs}
}))
```

```typescript
const static = require('koa-static')
cosnt path = require('path')
app.use(static(path.join(__dirname, '/static')))
```



# Koa 处理 POST 请求

​	koa 没有封闭获取 POST 请求参数的方法，因此需要解析 Context 中原生的 NodeJs 请求对象 req

```typescript
app.use(async (ctx)=>{
	let postData = ''
	ctx.req.on('data', data => postData += data )
	ctx.req.end('end', ()=>{
		console.log(postData)
	})
})
```

> 还可以使用 koa-bodyparser 等中间件来获取 POST 请求参数，而且更加方便

# 使用 curl 模拟 post 请求

​	-d 后面是 post 请求的参数

```shell
curl -d "param1=value1&param2=value2" http://127.0.0.1:3000
```

# Router 实现原理

```typescript
class Router{
  constructor(){
    this._routes = []
  }
  
  get(url, handler){
		this._routes.push({
      method: 'get',
      url,
      handler
    })
  }
  
  routes(){
    return async (ctx, next){
      const {method, url} = ctx
      const matchRouter = this._routes.find(v => v.method===method && v.url===v.url)
      if (matchRouter && matchRouter.hander){
        	await matchRoute.hander()
      }else{
        	await next()
      }
    }
  }
}

module.export = Router
```

# RESTful

​	REST 全称 Representational State Transfer，表现层状态转移，于 2000 年由 HTTP 主要设计人在博士论文中提出。REST 为开发者提供了一组架构的约束条件和指导原则。

​	REST 设计一艘符合以下条件：

1. 程序和应用的事物都应该被抽象为资源
2. 每个资源对应唯一的 **URI** ( Uniform Resoure Identifier，统一资源定位符)
3. 使用统一的接口对资源进行操作
4. 对资源的各种操作不会改变资源标识
5. 所有的操作都是无状态的

​    在 RESTful 架构中，所有的关键点在于如何定义资源和如何提供资源的访问上。

## 1. OOP 方式

​	OOP ( Object Oriented Programming 面对对象编程 )，将资源当作类，一个资源提供多个方法。而具体的实现就是通过 HTTP 的 **GET**、**POST**、**DELETE**、**PUT**

```html
<!-- 传统设计 -->
http://api.test.com/addUser 		<!-- POST -->
http://api.test.com/updUser 		<!-- POST -->
http://api.test.com/removeUser 	<!-- POST -->
http://api.test.com/getUser 		<!-- GET -->
<!-- RESTful -->
http://api.test.com/users 			<!-- POST 请求体用新增用户信息-->
http://api.test.com/users/:id 	<!-- PUT 请求体为用户更新信息，用户 ID 是 URI 的一部分-->
http://api.test.com/users/:id 	<!-- DELETE 用户 ID 是 URI 的一部分-->
http://api.test.com/users/:id 	<!-- GET 用户 ID 是 URI 的一部分-->
```

```typescript
router.post('/user', (ctx,next)=>{})
router.put('/user/:id', (ctx,next)=>{})
router.delete('/user/:id', (ctx,next)=>{})
router.get('/user/:id', (ctx,next)=>{})
router.all('/user', (ctx,next)=>{}) // 请求指定路径总是会执行，一般用于添加请求头
```

> RESTful 具体的使用可以参考 GitHub，GitHub 的 Api 设计堪称 RESTful 的教科书，http://developer.github.com/v3
>
> GitHub v4 的 API 设计使用的 **GraphQL**

# koa-router 配合 koa-jwt 实现接口的权限控制

## 1. 用户权限鉴别

​	常见的用户鉴别的方式有两种：

1. Cookie-Based Authentication
2. Token-Based Authentication
   - 优点：采用无状态机制，天然的跨域支持
   - 缺点：
     1. 服务器每次都需要对 Token 进行校验
     2. 无状态 API 缺少对用户流程或异常的控制，为了避免出现一些例如 **回放攻击** 的异常情况，应该设置较短的过期时间，且需要对密钥进行严格的保护。对安全性要求较高的场景（支付），应慎用