## Node学习

#### Node与浏览器区别

##### 交互对象不同

在浏览器中，大多数时候做的是与 DOM 或其他 Web 平台 API（例如 Cookies）进行交互。 当然，那些在 Node.js 中是不存在的。 没有浏览器提供的 `document`、`window`、以及所有其他的对象。

而且在浏览器中，不存在 Node.js 通过其模块提供的所有不错的 API，例如文件系统访问功能。

##### 对运行环境的控制不同

另一个很大的不同是，在 Node.js 中，可以控制运行环境。 除非构建的是任何人都可以在任何地方部署的开源应用程序，否则你能知道会在哪个版本的 Node.js 上运行该应用程序。 与浏览器环境（你无法选择访客会使用的浏览器）相比起来，这非常方便。

这意味着可以编写 Node.js 版本支持的所有现代的 ES6-7-8-9 JavaScript。

由于 JavaScript 发展的速度非常快，但是浏览器发展得慢一些，并且用户的升级速度也慢一些，因此有时在 web 上，不得不使用较旧的 JavaScript / ECMAScript 版本。

可以使用 Babel 将代码转换为与 ES5 兼容的代码，再交付给浏览器，但是在 Node.js 中，则不需要这样做。

##### 模块系统(/标准)不同

另一个区别是 Node.js 使用 CommonJS 模块系统，而在浏览器中，则还正在实现 ES 模块标准。

在实践中，这意味着在 Node.js 中使用 `require()`，而在浏览器中则使用 `import`。



#### V8 JavaScript引擎

V8 是为 Google Chrome 提供支持的 JavaScript 引擎的名称。 当使用 Chrome 进行浏览时，它负责**处理并执行 JavaScript**。

V8 提供了执行 JavaScript 的**运行时环境**。 DOM 和其他 Web 平台 API 则由浏览器提供。



JavaScript 引擎独立于托管它的浏览器。 此关键的特性推动了 Node.js 的兴起

##### 编译

JavaScript 通常被认为是一门**解释型**的语言，但是现代的 JavaScript 引擎不再只是解释 JavaScript，也会对其进行编译。

JavaScript 是由 V8 在其内部编译的，使用了**即时**（JIT）**编译**以加快执行速度



### 

#### 从node.js读取环境变量

Node.js 的 `process` 核心模块提供了 `env` 属性，该属性承载了在启动进程时设置的所有环境变量。

这是访问 NODE_ENV 环境变量的示例，该环境变量默认情况下被设置为 `development`。

> 注意：`process` 不需要 "require"，它是自动可用的。

```javascript
process.env.NODE_ENV // "development"
```

在脚本运行之前将其设置为 "production"，则可告诉 Node.js 这是生产环境。

可以用相同的方式访问设置的任何自定义的环境变量。



#### package.json

项目的清单，它可以做很多完全互不相关的事情。 例如，它是用于工具的配置中心。 它也是 `npm` 和 `yarn` 存储所有已安装软件包的名称和版本的地方。

##### 唯一的要求

 必须遵守 JSON 格式，否则，尝试以编程的方式访问其属性的程序则无法读取它。

##### 属性

```
version 表明了当前的版本。
name 设置了应用程序/软件包的名称。
description 是应用程序/软件包的简短描述。
main 设置了应用程序的入口点。
private 如果设置为 true，则可以防止应用程序/软件包被意外地发布到 npm。
scripts 定义了一组可以运行的 node 脚本。
dependencies 设置了作为依赖安装的 npm 软件包的列表。
devDependencies 设置了作为开发依赖安装的 npm 软件包的列表。
engines 设置了此软件包/应用程序在哪个版本的 Node.js 上运行。
browserslist 用于告知要支持哪些浏览器（及其版本）。
```

##### 软件包版本

在上面的描述中，已经看到类似以下的版本号：`〜3.0.0` 或 `^0.13.0`。 它们是什么意思，还可以使用哪些其他的版本说明符？

该符号指定了软件包能从该依赖接受的更新。

`^`: 只会执行不更改最左边非零数字的更新。 如果写入的是 `^0.13.0`，则当运行 `npm update` 时，可以更新到 `0.13.1`、`0.13.2` 等，但不能更新到 `0.14.0` 或更高版本。 如果写入的是 `^1.13.0`，则当运行 `npm update` 时，可以更新到 `1.13.1`、`1.14.0` 等，但不能更新到 `2.0.0` 或更高版本。

如果写入的是 `〜0.13.0`，则当运行 `npm update` 时，会更新到补丁版本：即 `0.13.1` 可以，但 `0.14.0` 不可以。

鉴于使用了 semver（语义版本控制），所有的版本都有 3 个数字，第一个是主版本，第二个是次版本，第三个是补丁版本。

还可以在范围内组合以上大部分内容，例如：`1.0.0 || >=1.1.0 <1.2.0`，即使用 1.0.0 或从 1.1.0 开始但低于 1.2.0 的版本

##### npm依赖与开发依赖 

##### Promise

Promise 通常被定义为**最终会变为可用值的代理**。

Promise 是一种处理异步代码（而不会陷入[回调地狱](http://callbackhell.com/)）的方式。

多年来，promise 已成为语言的一部分（在 ES2015 中进行了标准化和引入），并且最近变得更加集成，在 ES2017 中具有了 **async** 和 **await**。





##### Fetch()

Fetch API是基于promise的极致，调用fetch()相当于使用new Promise()来定义promise。

##### async与await

在任何函数之前加上 `async` 关键字意味着该函数会返回 promise。

即使没有显式地这样做，它也会在内部使它返回 promise。





### ECMAScript 

定义**语法** 写js和nodejs都必须遵守

变量定义，循环、判断、函数

原型和原型链、作用域和闭包、异步



不能操作dom,不能监听click事件，不能发送ajax请求

不能处理http请求，不能操作文件

#### JavaScript

使用ECMAScript语法规范，外加Web API,缺一不可

DOM操作，BOM操作，事件绑定，AJax等

两者结合，可以完成浏览器端的任何规范



#### nodejs

使用ECMAScript语法规范，外加nodejs API,缺一不可

处理http请求，处理文件等

两者结合，即可完成server端的任何操作





#### commonjs



module.exports={



}



const { }=require('')





为什么使用模块化





#### debugger调试

package.json  

"main"字段



### 项目需求分析

##### 目标

只开发server端，不关心前端

##### 需求   

首页、作者页、博客页

登录页

管理中心、新建页、编辑页

> 需求一定要明确，需求指导开发

##### 技术方案

数据如何存储

如何与前端对接，即接口设计



### 数据存储

用数据库表存储

本项目主要包含博客与用户两张表

#### 





### 在浏览器地址输入url后发生了什么

DNS解析，建立TCP连接，发送http请求

server接收http请求，处理，并返回

客户端接收到返回数据，处理数据（如渲染页面，执行js



 nodejs处理http请求



#### node中的路由与api

##### 将router与controller分开

router 只管路由，保证数据正确错误的格式一样

controller处理数据， 对应路由，怎么包装与它无关

##### API

前端和后端，不同端之间对接的一个术语

url （路由  输入 输出

##### 对路由的理解

- API的一部分
- 后端系统内部的一个模块



#### mysql

最流行的关系型数据库



##### 建表

文章内容可以设置为longtext

创建时间 毫秒数 bigint (20)

##### 增删改查

`password` 输入语句里可以



##### nodejs操作mysql





##### 小技巧

```js

// controller 是最关心数据的层次
const getList=(author,keyword)=>{
	// 为什么 where 1=1 ,因为author和keywords不确定有没有，要是没有这个，sql语句会出错
    // 存在不确定的语句 以and开头 没有where 1=1 ，sql语句就变成 .... from blogs and ...，明显有误
	// 类似的，会有 xxx.html?a=1&k1=v1&k2=v2&k3=v3  设置了a=1 就不需要考虑到底有没有？
	let sql = `select * from blogs where 1=1`;
	if (author) {
		sql += `and author='${author}'`;
	}
	if (keyword) {
		sql += `and title like '%${author}%'`;
	}

	sql += `order by createtime desc;`;
	//  返回promise
	return exec(sql);
}
```

##### 报错信息

Error: ER_PARSE_ERROR: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'by create_time desc' at line 1







#### tips

删除类似更新，不要一下子删除，要保证数据的可恢复性





### 登录

#### 核心

登录校验&登录信息存储

##### 重点

- cookie和session
- session写redis
- 用到nginx反向代理

#### cookie

- 存储浏览器的一段字符串（最大5kb）
- 跨域不共享
- 格式如k1=v1;k2=v2;因此可以存储结构化数据
- 每次发送cookie请求，会将请求域的cookie一起发给server
- server端可以修改cookie并返回给浏览器
- 浏览器中可以通过JavaScript修改cookie（有限制

##### cookie的限制

```js
// 操作cookie cookie的生效路由改为/ 跟路由
// httpOnly 只允许后端来改
res.setHeader('Set-Cookie', `username=${data.username};path=/;httpOnly;expires=${getCookieExpires()}`);
```



#### session

即server端存储用户信息

cookie会暴露username，很危险  会暴露用户的隐私信息

不适合存储敏感的，明文的

##### 如何解决：

cookie中存储userid，server端对应username



#### 从session到redis

目前session是js变量，放在nodeJs进程内存中

线程内存优先，访问量过大，内存暴增怎么办

正式上线运行是多进程，进程之间内存无法共享

##### 解决方案redis

web server最常用的缓存数据库，数据库放在内存中

相较于mysql，访问速度快

成本更高，可存储的数据量更小（内存的硬伤



将webserver和redis拆分成两个单独的服务

双方都是独立的，都是可扩展的（例如都扩展成集群

mysql也是一个服务，也可扩展

##### 为什么session适合用redis?

session频繁访问，对性能要求极高

session可不考虑断电丢失数据的问题（内存的硬伤

session数据量不会很大（相较于mysql中存储的数据



#### nginx代理

npm install http-server -g 

http-server -p 8001

##### 介绍

高性能web服务器，开源免费

一般做静态服务（所以用nginx，不太用nodejs、负载均衡

反向代理

##### nginx反向代理

![](C:\Users\刘颖\Documents\tips截图\node学习\2-nginx反向代理.png)

反向代理：对客户端做反向的代理，反向代理，客户端控制不了，对客户端而言是个黑盒

正向代理：公司的内网访问不了，装个代理工具，浏览器客户端控制代理，就是正向代理

nginx做一个统一的入口



用nginx配置的端口进入

然后axios发送请求时，直接用/api /之类，不需要再配置host与port



#### 日志

系统没有日志，就等于人没有眼睛

访问日志access log（server端最重要的日志）

自定义日志，包括自定义事件，错误记录等



日志功能 开发和使用

日志文件拆分，日志内容分析

##### 为什么日志存储到文件中，不存储到mysql，不存储到redis中？

日志文件非常大

mysql联动查询，表结构，不需要管理和基础 ，成本高



#### node文件操作

为什么需要path？

因为mac与windows路径的拼接方式不一样，需要统一的处理方式

##### fs

fs.readFile 读取

fs.writeFile 写入

fs.exists 判断是否存在

##### IO操作的性能瓶颈

io包括网络io和文件io

网络io 执行网络请求 网络带宽 

相比较CPU计算和内存读写，IO的突出特点就是：**慢**



##### stream

source~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~dest

![](C:\Users\刘颖\Documents\tips截图\node学习\4-stream.png)





标准输入输出 pipe就是管道（符合水流管道的模型图

process.stdin 获取数据，直接通过管道传递个process.stdout



解放cpu和内存

![](C:\Users\刘颖\Documents\tips截图\node学习\4-stream-网络io例子.png)

![](C:\Users\刘颖\Documents\tips截图\node学习\4-stream-使用.png)





拷贝文件

![](C:\Users\刘颖\Documents\tips截图\node学习\4-stream-拷贝文件.png)





返回文件给api

![](C:\Users\刘颖\Documents\tips截图\node学习\4-stream-返回一个文件给网络请求.png)

##### 日志拆分

日志内容会累积，放在一个文件中不好吃力

按时间划分日志文件





```
启动redis
redis-server.exe redis.windows.conf

redis-cli.exe -h 127.0.0.1 -p 6379

启动html
npm install http-server -g 

http-server -p 8001
```



### 安全

#### sql注入

最原始最简单的攻击

输入一个sql片段，最终拼接成一段攻击代码

> 预防措施：使用escape -- 注释 

#### xss 攻击

攻击方式：在页面展示内容中掺杂js代码，以获取网页信息，

预防措施：转换生成js的特殊字符

> npm -i xss



确实配置

缺少session写入redis

##### cryptp

 nodejs中用于加密的库

```js
const cryp =require('crypto')

// 密匙


const SECRET_KEY='WJio1_jshd#'

// MD5加密

function md5(content){
  let md5=crypto.createHash('md5')
  return md5.update(content).digest('hex')
  //hex 变成十六进制的输出的形式
}

// 加密函数
function genPassword(password){

  const str = `password=${password}&key=${SECRET_KEY}`
  return  md5(str)
}

module.exports={
  genPassword
}
```



### 总结

#### 功能模块

- 处理http接口
- 连接数据库
- 实现登录
- 安全
- 日志
- 上线

![](C:\Users\刘颖\Documents\tips截图\node学习\5-流程图.png)



### express使用

nodejs最常用的web server框架



express下载安装和使用，express 中间件机制

开发接口，连接数据库，实现登录，日志记录

分析express中间件原理

#### 安装

使用脚手架 express-generator

```
cnpm install express-generator -g

express 项目名

cd 项目名
cnpm install
npm run dev
```





#### app.js

##### 中间件机制

app.use() 第一个参数没有路由的话，默认全部访问

next() 寻找下一个符合的 



##### 登录

express-session connect-redis

req.session保存登录信息，登录校验做成express

##### 日志

使用脚手架推荐的morgan

自定义日志使用console.log console.error  pm?pm2 线上环境时会写

日志文件拆分、日志文件内容分析



##### express中间件原理

必须实现的接口 use listen

遇到http请求，根据method和path判断触发

实现next机制，即上一个通过next触发下一个

传路由，

slice  

res.json是自定义的



##### 异步回调

nodejs 8.0以上才能支持async/await语法



### koa2

express中间件是异步回调，koa2原生支持async/await

js单线程

新开发框架和系统，都开始基于koa2，例如egg.js

express虽然未过时，但是koa2肯定是未来趋势



##### async与await

await 后面可以追加promise对象，获取resolve的值

await必须包裹在async函数里

async函数执行返回的也是一个promise对象

try catch可以捕获reject的值



#### koa2安装

cnpm install koa-generator -g

#### 开发接口

##### 实现登录

基于koa-generic-session和koa-redis

cnpm install koa-generic-session koa-redis redis -S

##### 开发路由

cnpm install mysql xss -S

post放在body里的数据 需要用ctx.request.body ctx.body是响应体

##### 记录日志

cnpm install koa-morgan -S

