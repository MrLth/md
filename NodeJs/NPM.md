# NPM 命令备忘

1. **npm update**，更新模块
2. **npm ls**，查看已安装的模块
3. **npm root**，显示 npm 根目录
4. **npm cachel**，管理模块缓存

# package.json 两次安装依赖版本不一致

​	原因是因为 NPM 的版本兼容机制比较宽松，NPM 定义的版本是向后兼容的，定义的只要大版号相同（如下方的 2），就允许下载最新版本的模块

```typescript
"dependencies":{
	"koa": "^2.5.0" // ^ 表示大于某个版本，实际安装可以会是 2.5.2
}
```

**解决方法**

​	package-lock.json 锁定了依赖包，只要保存了源文件，就能确保得到完全一致的依赖包，从而提高了模块的稳定性和可用性

# Web 代理工具 Nproxy

​	Nproxy 是一个跨平台、支持单文件、多文件及目录替换、支持 HTTP 和 HTTPS 协议的 Web 代理工具，在文件替换上尤其优秀，相对 Fiddler、Charles 的优点

1. 同时支持 Mac、Windows、Linus
2. 使用多个本地源文件替换线上的 combo 文件
3. 支持目录替换
4. 支持 HTTP 和 HTTPS

```shell
// 安装
npm install -g nporxy
// 启动, replace_rule.js 为开发者创建的替换规则文件，设置浏览器代理为：127.0.0.1:8989
nporxy -l replace_rule.js 

```

# VS Code 调试 NodeJS

1. 在调试窗口中选择 **添加配置**
2. 添加 **Node.js 附加到进程** 配置
3. 在文件中添加 **断点**，点击调试
4. 打开浏览器输入 **localhost:8080** 查看调试

