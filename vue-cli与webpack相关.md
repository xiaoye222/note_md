#### 查看版本

##### 查看vue版本

终端输入npm ls vue或者在package.json中直接查看

![](C:\Users\刘颖\Documents\tips截图\【版本】vue命令.png)

##### 查看vue-cli版本

终端输入vue-V

![](C:\Users\刘颖\Documents\tips截图\【版本】vue-cli命令.png)



##### 查看webpack版本

![](C:\Users\刘颖\Documents\tips截图\【版本】查看webpack命令.png)

```
如果没有出现，npm install --global webpack-cli，因为webpack 4x以上，webpack将命令相关的内容都放到了webpack-cli，所以还需要安装webpack-cli； 之后再webpack -v
```





#### 

### vue-cli相关

#### 利用vue-cli快速创建项目

vue init webpack 项目名 vue-cli 2.x旧版本

vue create 项目名

##### 拉取 2.x 模板 (旧版本)

Vue CLI >= 3 和旧版使用了相同的 `vue` 命令，所以 Vue CLI 2 (`vue-cli`) 被覆盖了。如果你仍然需要使用旧版本的 `vue init` 功能，你可以全局安装一个桥接工具：

```bash
npm install -g @vue/cli-init
# `vue init` 的运行效果将会跟 `vue-cli@2.x` 相同 
vue init webpack my-project
```



##### vue下没有webpack.config.js，相关配置应该配置在哪个文件?

自己在根目录下新建vue.config.js，

```js
// vue.config.js
module.exports = {
  
}
```

有些 webpack 选项是基于 `vue.config.js` 中的值设置的，所以不能直接修改。例如你应该修改 `vue.config.js` 中的 `outputDir` 选项而不是修改 `output.path`；应该修改 `vue.config.js` 中的 `publicPath` 选项而不是修改 `output.publicPath`。这样做是因为 `vue.config.js` 中的值会被用在配置里的多个地方，以确保所有的部分都能正常工作在一起。



##### vue inspect 审查项目的 webpack 配置

因为 `@vue/cli-service` 对 webpack 配置进行了抽象，所以理解配置中包含的东西会比较困难，尤其是当你打算自行对其调整的时候。`vue-cli-service` 暴露了 `inspect` 命令用于审查解析好的 webpack 配置

```bash
vue inspect > output.js
```



#### 浏览器兼容性

package.json文件里browserslist字段或者一个单独.browerslistrc文件，hiding项目的目标浏览器范围

#### HTML和静态资源

##### 相对路径导入

当在 JavaScript、CSS 或 `*.vue` 文件中使用相对路径 (必须以 `.` 开头) 引用一个静态资源时，该资源将会被包含进入 webpack 的依赖图中。在其编译过程中，所有诸如 `<img src="...">`、`background: url(...)` 和 CSS `@import` 的资源 URL **都会被解析为一个模块依赖**。

例如，`url(./image.png)` 会被翻译为 `require('./image.png')`，而：

```html
<img src="./image.png">
```

将会被编译到：

```js
h('img', { attrs: { src: require('./image.png') }})
```

##### URL 转换规则

- 如果 URL 是一个绝对路径 (例如 `/images/foo.png`)，它将会被保留不变。

- 如果 URL 以 `.` 开头，它会作为一个相对模块请求被解释且基于文件系统中的目录结构进行解析。

- 如果 URL 以 `~` 开头，其后的任何内容都会作为一个模块请求被解析。这意味着甚至可以引用 Node 模块中的资源：

  ```html
  <img src="~some-npm-package/foo.png">
  ```

- 如果 URL 以 `@` 开头，它也会作为一个模块请求被解析。它的用处在于 Vue CLI 默认会设置一个指向 `<projectRoot>/src` 的别名 `@`。**(仅作用于模版中)**

#### public文件夹

任何放置在 `public` 文件夹的静态资源会被简单的复制，而不经过 webpack，需要通过绝对路径来引用它们。

推荐将资源作为你的模块依赖图的一部分导入，这样它们会通过 webpack 的处理并获得如下好处：

- 脚本和样式表会被压缩且打包在一起，从而避免额外的网络请求。
- 文件丢失会直接在编译时报错，而不是到了用户端才产生 404 错误。
- 最终生成的文件名包含了内容哈希，因此不必担心浏览器会缓存它们的老版本。

`public` 目录提供的是一个**应急手段**，当通过绝对路径引用它时，留意应用将会部署到哪里。如果应用没有部署在域名的根部，那么需要为 URL 配置 [publicPath](https://cli.vuejs.org/zh/config/#publicpath) 前缀：

##### publicPath

- Type: `string`

- Default: `'/'`

  部署应用包时的基本 URL。用法和 webpack 本身的 `output.publicPath` 一致，但是 Vue CLI 在一些其他地方也需要用到这个值，所以**请始终使用 `publicPath` 而不要直接修改 webpack 的 `output.publicPath`**。

  默认情况下，Vue CLI 会假设应用是被部署在一个域名的根路径上，例如 `https://www.my-app.com/`。如果应用被部署在一个子路径上，就需要用这个选项指定这个子路径。例如，如果应用被部署在 `https://www.my-app.com/my-app/`，则设置 `publicPath` 为 `/my-app/`。

  这个值也可以被设置为空字符串 (`''`) 或是相对路径 (`'./'`)，这样所有的资源都会被链接为相对路径，这样打出来的包可以被部署在任意路径，也可以用在类似 Cordova hybrid 应用的文件系统中。

  相对 publicPath 的限制

  相对路径的 `publicPath` 有一些使用上的限制。在以下情况下，应当避免使用相对 `publicPath`:

  - 当使用基于 HTML5 `history.pushState` 的路由时；
  - 当使用 `pages` 选项构建多页面应用时。

  这个值在开发环境下同样生效。如果你想把开发服务器架设在根路径，你可以使用一个条件式的值：

  ```js
  module.exports = {
    publicPath: process.env.NODE_ENV === 'production'
      ? '/production-sub-path/'
      : '/'
  }
  ```





##### [scss(sass 3）与less区别](https://www.cnblogs.com/lzy666/p/7336994.html)

###### 变量符 

$ scss       

@ less

scss引用的外部文件命名必须以_开头，

###### [父选择符](https://www.cnblogs.com/ibabyli/p/9856351.html)

& 父选择符



#### 模式和环境变量

##### 模式

默认情况下，一个 Vue CLI 项目有三个模式：

- `development` 模式用于 `vue-cli-service serve`
- `test` 模式用于 `vue-cli-service test:unit`
- `production` 模式用于 `vue-cli-service build` 和 `vue-cli-service test:e2e`



可以通过传递 `--mode` 选项参数为命令行覆写默认的模式。例如，如果想要在构建命令中使用开发环境变量

```js
vue-cli-service build --mode development
```



当运行 `vue-cli-service` 命令时，所有的环境变量都从对应的环境文件中载入。

如果文件内部不包含 `NODE_ENV` 变量，它的值将取决于模式，例如，在 `production` 模式下被设置为 `"production"`，在 `test` 模式下被设置为 `"test"`，默认则是 `"development"`。

`NODE_ENV` 将决定应用运行的模式，是开发，生产还是测试，因此也决定了创建哪种 webpack 配置。

例如通过将 `NODE_ENV` 设置为 `"test"`，Vue CLI 会创建一个优化过后的，并且旨在用于单元测试的 webpack 配置，它并不会处理图片以及一些对单元测试非必需的其他资源。

同理，`NODE_ENV=development` 创建一个 webpack 配置，该配置启用热更新，不会对资源进行 hash 也不会打出 vendor bundles，目的是为了在开发的时候能够快速重新构建。

当运行 `vue-cli-service build` 命令时，无论部署到哪个环境，应该始终把 `NODE_ENV` 设置为 `"production"` 来获取可用于部署的应用程序。

##### 环境变量

可以在项目根目录中放置下列文件来指定环境变量：

```bash
.env                # 在所有的环境中被载入
.env.local          # 在所有的环境中被载入，但会被 git 忽略
.env.[mode]         # 只在指定的模式中被载入
.env.[mode].local   # 只在指定的模式中被载入，但会被 git 忽略
```

一个环境文件只包含环境变量的“键=值”对：

```text
FOO=bar
VUE_APP_NOT_SECRET_CODE=some_value
```

不要在应用程序中存储任何机密信息（例如私有 API 密钥）！环境变量会随着构建打包嵌入到输出代码，意味着任何人都有机会能够看到它。

请注意，只有 `NODE_ENV`，`BASE_URL` 和以 `VUE_APP_` 开头的变量将通过 `webpack.DefinePlugin` 静态地嵌入到*客户端侧*的代码中。这是为了避免意外公开机器上可能具有相同名称的私钥。

被载入的变量将会对 `vue-cli-service` 的所有命令、插件和依赖可用。

###### 环境文件加载优先级

为一个特定模式准备的环境文件 (例如 `.env.production`) 将会比一般的环境文件 (例如 `.env`) 拥有更高的优先级。

此外，Vue CLI 启动时已经存在的环境变量拥有最高优先级，并不会被 `.env` 文件覆写。

`.env` 环境文件是通过运行 `vue-cli-service` 命令载入的，因此环境文件发生变化，需要重启服务。

###### 在客户端侧代码中使用环境变量

只有以 `VUE_APP_` 开头的变量会被 `webpack.DefinePlugin` 静态嵌入到客户端侧的包中。可以在应用的代码中这样访问它们：

```js
console.log(process.env.VUE_APP_SECRET)
```

在构建过程中，`process.env.VUE_APP_SECRET` 将会被相应的值所取代。在 `VUE_APP_SECRET=secret` 的情况下，它会被替换为 `"secret"`。

除了 `VUE_APP_*` 变量之外，在应用代码中始终可用的还有两个特殊的变量：

- `NODE_ENV` - 会是 `"development"`、`"production"` 或 `"test"` 中的一个。具体的值取决于应用运行的[模式](https://cli.vuejs.org/zh/guide/mode-and-env.html#模式)。
- `BASE_URL` - 会和 `vue.config.js` 中的 `publicPath` 选项相符，即你的应用会部署到的基础路径。







### 