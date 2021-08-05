# uniapp与小程序

### 规范

- 页面文件遵循vue单文件组件规范

- 组件标签靠近小程序规范

- 接口能力靠近微信小程序

- 数据绑定以及时间处理同vue.js规范

- 为兼容多端运行，建议使用flex布局

## 组件

### 小程序

#### 自定义组件

```js
Component({
    /**
     * 组件的属性列表, 由组件外部传入的数据， 等同于Vue中的props
     */
    properties: {
        title: {
            type: String,
            value: 'title默认值'
        }
    },
    
    lifetimes: {
    attached: function() {
      // 在组件实例进入页面节点树时执行  在组件完全初始化完毕、进入页面节点树后， attached 生命周期被触发，绝大多数初始化工作        可以在这个时机进行
    },
    detached: function() {
      // 在组件实例被从页面节点树移除时执行
    },
    },
    
    // 以下是旧式的定义方式，可以保持对 <2.2.3 版本基础库的兼容  
    attached: function() {
    // 在组件实例进入页面节点树时执行
    }, 
    detached: function() {
    // 在组件实例被从页面节点树移除时执行
    },
    
	//它们并非与组件有很强的关联，但有时组件需要获知，以便组件内部处理。这样的生命周期称为“组件所在页面的生命周期”，在 pageLifetimes 定义段中定义
    pageLifetimes: {
    show: function() {
      // 页面被展示
    },
    hide: function() {
      // 页面被隐藏
    },
    resize: function(size) {
      // 页面尺寸变化
    }  
    }

    /**
     * 组件的初始数据 私有数据，可用于模板渲染
     */
    data: {

    },

    /**
     * 组件的方法列表
     */
    methods: {

    }
})
```

#### 组件代码复用

##### Behavior构造器 

###### 解决js里面代码复用问题

首先创建一个Behavior构造器，export出去

然后在组件中import，用behavior：[]声明

###### css复用

@import 导入common.wxss 注意加分号



#### 常用组件

##### scroll-view

可滚动视图区域。使用竖向滚动时，需要给scroll-view一个固定高度，通过 WXSS 设置 height。

| 属性        | 类型    | 默认值 | 必填 | 说明                                                         | 最低版本                                                     |
| :---------- | :------ | :----- | :--- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| scroll-x    | boolean | false  | 否   | 允许横向滚动                                                 | [1.0.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| scroll-y    | boolean | false  | 否   | 允许纵向滚动                                                 | [1.0.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| enable-flex | boolean | false  | 否   | **css配置对组件设置对应flex，组件本身也需要启用 flexbox 布局**。开启后，当前节点声明了 `display: flex` 就会成为 flex container，并作用于其孩子节点。 |                                                              |

##### swipper

滑块视图容器。其中只可放置swiper-item组件，否则会导致未定义的行为。

### uniapp

类vue

## 授权

https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/authorize.html

小程序登录、用户信息相关接口调整说明

https://developers.weixin.qq.com/community/develop/doc/000cacfa20ce88df04cb468bc52801

## 网络请求

### 小程序

#### 回调函数

- **只要成功接收到服务器返回，无论 `statusCode` 是多少，都会进入 `success` 回调。请开发者根据业务逻辑对返回值进行判断**



#### wx.request(Object object)

##### Object object 相关参数

| 属性         | 类型                      | 默认值 | 必填 | 说明                                                         | 最低版本                                                     |
| :----------- | :------------------------ | :----- | :--- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| url          | string                    |        | 是   | 开发者服务器接口地址                                         |                                                              |
| data         | string/object/ArrayBuffer |        | 否   | 请求的参数                                                   |                                                              |
| header       | Object                    |        | 否   | 设置请求的 header，header 中不能设置 Referer。 `content-type` 默认为 `application/json` |                                                              |
| timeout      | number                    |        | 否   | 超时时间，单位为毫秒                                         | [2.10.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| method       | string                    | GET    | 否   | HTTP 请求方法                                                |                                                              |
| dataType     | string                    | json   | 否   | 返回的数据格式                                               |                                                              |
| responseType | string                    | text   | 否   | 响应的数据类型                                               | [1.7.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| success      | function                  |        | 否   | 接口调用成功的回调函数                                       |                                                              |
| fail         | function                  |        | 否   | 接口调用失败的回调函数                                       |                                                              |
| complete     | function                  |        | 否   | 接口调用结束的回调函数（调用成功、失败都会执行）             |                                                              |



##### **object.dataType 的合法值**

| 值   | 说明                                                       | 最低版本 |
| :--- | :--------------------------------------------------------- | :------- |
| json | 返回的数据为 JSON，返回后会对返回的数据进行一次 JSON.parse |          |
| 其他 | 不对返回的内容进行 JSON.parse                              |          |



##### object.success 回调函数

参数 Object res

| 属性       | 类型                      | 说明                                         | 最低版本                                                     |
| :--------- | :------------------------ | :------------------------------------------- | :----------------------------------------------------------- |
| data       | string/Object/Arraybuffer | 开发者服务器返回的数据                       |                                                              |
| statusCode | number                    | 开发者服务器返回的 HTTP 状态码               |                                                              |
| header     | Object                    | 开发者服务器返回的 HTTP Response Header      | [1.2.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| cookies    | Array.<string>            | 开发者服务器返回的 cookies，格式为字符串数组 | [2.10.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| profile    | Object                    | 网络请求过程中一些调试信息                   | [2.10.4](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |

##### data 参数说明

最终发送给服务器的数据是 String 类型，如果传入的 data 不是 String 类型，会被转换成 String 。转换规则如下：

- 对于 `GET` 方法的数据，会将数据转换成 query string（`encodeURIComponent(k)=encodeURIComponent(v)&encodeURIComponent(k)=encodeURIComponent(v)...`）
- 对于 `POST` 方法且 `header['content-type']` 为 `application/json` 的数据，会对数据进行 JSON 序列化
- 对于 `POST` 方法且 `header['content-type']` 为 `application/x-www-form-urlencoded` 的数据，会将数据转换成 query string `（encodeURIComponent(k)=encodeURIComponent(v)&encodeURIComponent(k)=encodeURIComponent(v)...）`



### uniapp

##### uni.request(OBJECT)

OBJECT 参数说明

| 参数名          | 类型                      | 必填 | 默认值 | 说明                                               | 平台差异说明                                                 |
| :-------------- | :------------------------ | :--- | :----- | :------------------------------------------------- | :----------------------------------------------------------- |
| url             | String                    | 是   |        | 开发者服务器接口地址                               |                                                              |
| data            | Object/String/ArrayBuffer | 否   |        | 请求的参数                                         | App（自定义组件编译模式）不支持ArrayBuffer类型               |
| header          | Object                    | 否   |        | 设置请求的 header，header 中不能设置 Referer。     | App、H5端会自动带上cookie，且H5端不可手动修改                |
| method          | String                    | 否   | GET    | 有效值详见下方说明                                 |                                                              |
| timeout         | Number                    | 否   | 60000  | 超时时间，单位 ms                                  | H5(HBuilderX 2.9.9+)、APP(HBuilderX 2.9.9+)、微信小程序（2.10.0）、支付宝小程序 |
| dataType        | String                    | 否   | json   | 如果设为 json，会尝试对返回的数据做一次 JSON.parse |                                                              |
| responseType    | String                    | 否   | text   | 设置响应的数据类型。合法值：text、arraybuffer      | 支付宝小程序不支持                                           |
| withCredentials | Boolean                   | 否   | false  | 跨域请求时是否携带凭证（cookies）                  | 仅H5支持（HBuilderX 2.6.15+）                                |
| success         | Function                  | 否   |        | 收到开发者服务器成功返回的回调函数                 |                                                              |
| fail            | Function                  | 否   |        | 接口调用失败的回调函数                             |                                                              |
| complete        | Function                  | 否   |        | 接口调用结束的回调函数（调用成功、失败都会执行）   |                                                              |

##### success 返回参数说明

| 参数       | 类型                      | 说明                                         |
| :--------- | :------------------------ | :------------------------------------------- |
| data       | Object/String/ArrayBuffer | 开发者服务器返回的数据                       |
| statusCode | Number                    | 开发者服务器返回的 HTTP 状态码               |
| header     | Object                    | 开发者服务器返回的 HTTP Response Header      |
| cookies    | `Array.<string>`          | 开发者服务器返回的 cookies，格式为字符串数组 |



## uni-app生命周期

完整支持 `Vue` 实例的生命周期，同时还新增 [应用生命周期](https://uniapp.dcloud.io/collocation/frame/lifecycle?id=应用生命周期) 及 [页面生命周期](https://uniapp.dcloud.io/collocation/frame/lifecycle?id=页面生命周期)

### 应用生命周期

应用生命周期仅可在`App.vue`中监听，在其它页面监听无效

```js
// 只在app.vue里执行
	// 应用 初始化完成触发一次，全局只触发一次
	onLaunch: function() {
		// 完成 登录、获取全局变量
		console.log('App Launch')
	},
	// 应用启动的时候，或者从后台进入前台会触发
	onShow: function() {
		console.log('App Show')
	},
	// 应用从前台进入后台触发
	onHide: function() {
		console.log('App Hide')
	}
```

### 页面生命周期

```js
    // 监听页面加载
	onLoad() {
		console.log("page onLoad")
	},
    // 监听页面显示,页面每次出现在屏幕上都触发，包括从下级页面点返回露出当前页面
	onShow() {
		console.log("page页面on show")
	},
	// 监听页面
	onReady(){
		// 监听页面初次渲染完成，如果渲染速度快，会在页面进入动画完成前触发
		console.log("page  onReady")
	},
	// 监听页面显示
	onHide() {
		console.log("page页面on hide")
	},
	// 监听页面显示  关闭相当于卸载
	onUnload() {
		console.log("page页面on unload")
	},
```

### 组件生命周期

​	与vue标准组件的生命周期相同



## 小程序生命周期

### 应用生命周期

`App()` 函数用来注册一个小程序。接受一个 `Object` 参数，其指定小程序的生命周期回调等。
`App()` 必须在 `app.js` 中调用，必须调用且只能调用一次。

```js
App({
  onLaunch: function(options) {
    // 监听小程序初始化。小程序初始化完成时（全局只触发一次）
  },
  onShow: function(options) {
    // 监听小程序显示。小程序启动，或从后台进入前台显示时
  },
  onHide: function() {
    // 监听小程序隐藏。小程序从前台进入后台时。
  },
  onError: function(msg) {
    console.log(msg) // 错误监听函数。小程序发生脚本错误，或者 api 调用失败时触发，会带上错误信息
  },
  onPageNotFound: function(res) {
    // 页面不存在监听函数。小程序要打开的页面不存在时触发，会带上页面信息回调该函数
  },
  globalData: 'I am global data'
})
```

**前台、后台定义**： 当用户点击左上角关闭，或者按了设备 Home 键离开微信，小程序并没有直接销毁，而是进入了后台；当再次进入微信或再次打开小程序，又会从后台进入前台。

1. 用户首次打开小程序，触发 onLaunch 方法（全局只触发一次）。
2. 小程序初始化完成后，触发 onShow 方法，监听小程序显示。
3. 小程序从前台进入后台，触发 onHide 方法。
4. 小程序从后台进入前台显示，触发 onShow 方法。
5. 小程序后台运行一定时间，或系统资源占用过高，会被销毁。

全局的 `getApp()` 函数可以用来获取到小程序 `App` 实例。

```js
// other.js
var appInstance = getApp()
console.log(appInstance.globalData) // I am global data
```

**注意：**

- 不要在定义于 App() 内的函数中调用 getApp() ，使用 this 就可以拿到 app 实例。
- 通过 getApp() 获取实例之后，不要私自调用生命周期函数。



### 页面生命周期

页面生命周期函数就是当你每进入/切换到一个新的页面的时候，就会调用的生命周期函数。`Page(Object)` 函数用来注册一个页面。接受一个`Object`类型参数，其指定页面的初始数据、生命周期回调、事件处理函数等。

`Page(Object)`函数用来注册一个页面。接受一个 `Object` 类型参数，其指定页面的初始数据、生命周期回调、事件处理函数等。

```js
//index.js
Page({
  data: {
    // 页面的初始数据
    text: "This is page data."
  },
  onLoad: function(options) {
    // 生命周期回调—监听页面加载
  },
  onReady: function() {
    // 生命周期回调—监听页面初次渲染完成
  },
  onShow: function() {
    // 生命周期回调—监听页面显示
  },
  onHide: function() {
    // 生命周期回调—监听页面隐藏
  },
  onUnload: function() {
    // 生命周期回调—监听页面卸载
  },
  onPullDownRefresh: function() {
    // 监听用户下拉动作
  },
  onReachBottom: function() {
    // 页面上拉触底事件的处理函数
  },
  onShareAppMessage: function () {
    // 用户点击右上角转发
  },
  onPageScroll: function() {
    // 页面滚动触发事件的处理函数
  },
  onResize: function() {
    // 页面尺寸改变时触发
  },
  onTabItemTap(item) {
    // 当前是 tab 页时，点击 tab 时触发
    console.log(item.index)
    console.log(item.pagePath)
    console.log(item.text)
  },
  // 任意的函数，在页面的函数中用 this 可以访问
  viewTap: function() {
    this.setData({
      text: 'Set some data for updating view.'
    }, function() {
      // this is setData callback
    })
  },
  // 任意数据，在页面的函数中用 this 可以访问
  customData: {
    hi: 'MINA'
  }
})
```

- 小程序注册完成后，加载页面，触发onLoad方法。
- 页面载入后触发onShow方法，显示页面。
- 首次显示页面，会触发onReady方法，渲染页面元素和样式，一个页面只会调用一次。
- 当小程序后台运行或跳转到其他页面时，触发onHide方法。
- 当小程序有后台进入到前台运行或重新进入页面时，触发onShow方法。
- 当使用重定向方法wx.redirectTo(object)或关闭当前页返回上一页wx.navigateBack()，触发onUnload。

**总结**

- `onLoad`: 页面加载。一个页面只会调用一次。参数可以获取`wx.navigateTo`和`wx.redirectTo`及`<navigator/>`中的 `query`。
- `onShow`: 页面显示。每次打开页面都会调用一次。
- `onReady`: 页面初次渲染完成。一个页面只会调用一次，代表页面已经准备妥当，可以和视图层进行交互。对界面的设置如`wx.setNavigationBarTitle`请在`onReady`之后设置。
- `onHide`: 页面隐藏。当`navigateTo`或底部`tab`切换时调用。
- `onUnload`: 页面卸载。当`redirectTo`或`navigateBack`的时候调用。

### 组件生命周期

组件的生命周期，指的是组件自身的一些函数，这些函数在特殊的时间点或遇到一些特殊的框架事件时被自动触发。

其中，最重要的生命周期是 `created` `attached` `detached` ，包含一个组件实例生命流程的最主要时间点。

- 组件实例刚刚被创建好时， `created` 生命周期被触发。此时，组件数据 `this.data` 就是在 `Component` 构造器中定义的数据 `data` 。 **此时还不能调用 `setData` 。** 通常情况下，这个生命周期只应该用于给组件 `this` 添加一些自定义属性字段。
- 在组件完全初始化完毕、进入页面节点树后， `attached` 生命周期被触发。此时， `this.data` 已被初始化为组件的当前值。这个生命周期很有用，绝大多数初始化工作可以在这个时机进行。
- 在组件离开页面节点树后， `detached` 生命周期被触发。退出一个页面时，如果组件还在页面节点树中，则 `detached` 会被触发。

生命周期方法可以直接定义在 `Component` 构造器的第一级参数中。

自小程序基础库版本 [2.2.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 起，组件的的生命周期也可以在 `lifetimes` 字段内进行声明（这是推荐的方式，其优先级最高）。

**代码示例：**

```js
Component({
  lifetimes: {
    attached: function() {
      // 在组件实例进入页面节点树时执行
    },
    detached: function() {
      // 在组件实例被从页面节点树移除时执行
    },
  },
  // 以下是旧式的定义方式，可以保持对 <2.2.3 版本基础库的兼容
  attached: function() {
    // 在组件实例进入页面节点树时执行
  },
  detached: function() {
    // 在组件实例被从页面节点树移除时执行
  },
  // ...
})
```

在 behaviors 中也可以编写生命周期方法，同时不会与其他 behaviors 中的同名生命周期相互覆盖。

但要注意，如果一个组件多次直接或间接引用同一个 behavior ，这个 behavior 中的生命周期函数在一个执行时机内只会执行一次。

#### 组件所在页面的生命周期

还有一些特殊的生命周期，它们并非与组件有很强的关联，但有时组件需要获知，以便组件内部处理。这样的生命周期称为“组件所在页面的生命周期”，在 `pageLifetimes` 定义段中定义。其中可用的生命周期包括：

| 生命周期 | 参数          | 描述                         | 最低版本                                                     |
| :------- | :------------ | :--------------------------- | :----------------------------------------------------------- |
| show     | 无            | 组件所在的页面被展示时执行   | [2.2.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| hide     | 无            | 组件所在的页面被隐藏时执行   | [2.2.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| resize   | `Object Size` | 组件所在的页面尺寸变化时执行 | [2.4.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |

**代码示例：**

```js
Component({
  pageLifetimes: {
    show: function() {
      // 页面被展示
    },
    hide: function() {
      // 页面被隐藏
    },
    resize: function(size) {
      // 页面尺寸变化
    }
  }
})
```

### [小程序与uni-app的全局与页面配置的差异之处](https://editor.csdn.net/md/?articleId=105431757)

| vue                                                          | 小程序                                                       | uni-app                               |                                                         |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------- | ------------------------------------------------------- | ------------------------------------------------------------ |
| 小程序生命周期                                               | 页面生命周期                                                 | 应用生命周期                          | 页面生命周期                                            |                                                              |
| ***\*beforeCreate\****实例组件刚创建元素DOM和数据都还没有初始化 |                                                              | ***\*onLoad\****监听页面加载          |                                                         | ***\*onLoad\****监听页面加载其参数为上个页面传递的数据,参数类型为object(用于页面传参) |
| ***\*created\****实例已经被创建完毕 属性已经绑定 属性是可以操作的 但是dom还不存在 | ***\*onLaunch\****监听小程序初始化小程序初始化完成时         | ***\*onReady\****监听页面初次渲染完成 | ***\*onLaunch\****当uni-app初始化完成时触发             | ***\*onReady\****监听页面初次渲染完成                        |
| ***\*beforeMount\****DOM未完成挂载，数据也初始化完成但是数据的双向绑定还是显示{{}}，就像是占了个坑 | ***\*onShow\****监听小程序显示小程序启动或从后台进入前台显示时 | ***\*onShow\****监听页面显示          | ***\*onShow\****当uni-app启动，或从后台进入前台显示     | ***\*onShow\****监听页面显示                                 |
| ***\*mounted\****虚拟dom已经被渲染到了真实的dom上边可以数据请求或者赋值操作属性等等 | ***\*onHide\****监听小程序隐藏小程序从前台进入后台时         | ***\*onHide\****监听页面隐崴          | ***\*onHide\****当uni-app从前台进入后台                 | ***\*onHide\****监听页面隐崴                                 |
| ***\*beforeUpdate\****只要是页面数据改变了都会触发数据更新之前，页面数据还是原来的数据 | ***\*onError\****错误监听函数小程序发生脚本错误,或者api调用失败时触发，会带上错误信息 | ***\*onUnload\****监听页面卸载        | ***\*onUniNViewMessage\****对nvue页面发送的数据进行监听 | ***\*onUnload\****监听页面卸载                               |
| ***\*updated\****只要是页面数据改变了都会触发数据更新完毕，页面的数据是更新完成的 | ***\*onPageNotFound\**** 页面不存在监听函数小程序要打开的页面不存在时触发，会带上页面信息回调该函数 |                                       |                                                         |                                                              |
| ***\*beforeDestroy\****组件销毁之前执行                      |                                                              |                                       |                                                         |                                                              |
| ***\*Destroyed\****销毁                                      |                                                              |                                       |                                                         |                                                              |

注意：

1.beforeUpdate和updated要谨慎使用，因为页面更新数据的时候都会触发，在这里操作数据很影响性能和容易死循环。一定要加判断条件确保能跳出去；

2.粉色的是全局只触发一次

3.应用生命周期影响页面生命周期

## 页面配置

uni-app中

```
1、在pages目录中新建.vue文件
2、在pages.json中->pages数组内
	{
		"path":"不加后缀的文件路径",
		"style":{页面样式配置}
		
	},
```

小程序中

每一个小程序页面也可以使用 `.json` 文件来对本页面的窗口表现进行配置。页面中配置项在当前页面会覆盖 `app.json` 的 `window` 中相同的配置项。文件内容为一个 JSON 对象。

## 全局配置

`pages.json` 文件用来对 uni-app 进行全局配置，决定页面文件的路径、窗口样式、原生的导航栏、底部的原生tabbar 等。

它类似微信小程序中`app.json`的**页面管理**部分。注意定位权限申请等原属于`app.json`的内容，在uni-app中是在manifest中配置。

## 页面通信





[来源]: https://segmentfault.com/a/1190000008895441	"页面通信"

![小程序页面通信](C:\Users\刘颖\Pictures\笔记\小程序页面通信.png)

**场景**：首页PageA有一个飘数，当我们从PageA新开PageC后，做一些操作，再回退到PageA的时候，这个飘数要刷新。

很显然，这需要在PageC中做操作时，能通知到PageA,以便PageA做相应的联动变化。

**通知**，页面通信。所谓通信，满足下面两个条件：

1.激活对方的一个方法调用

2.能够向被激活的方法传递数据

### 分类

#### 页面层级

1. 兄弟页面间通信。如多Tab页面间通信，PageA，PageB之间通信
2. 父路径页面向子路径页面通信，如PageA向PageC通信
3. 子方式路径页面向父路径页面通信，如PageC向PageA通信

#### 通信时激活对方的时机

1. 延迟激活，即我在PageC做完操作，等返回到PageA再激活PageA的方法调用
2. 立即激活，即我在PageC做完操作，在PageC激活PageA的方法调用

#### 方式一：onShow/onHide + localStorage

利用onShow/onHide激活方法，通过localStorage传递数据

**优点**：实现简单，容易理解
**缺点**：如果完成通信后，没有即时清除通信数据，可能会出现问题。另外因为依赖localStorage，而localStorage可能出现读写失败，从面造成通信失败
**注意点**：页面初始化时也会触发onShow

#### 方式二：onShow/onHide + 小程序globalData

利用onShow/onHide激活方法，通过读写小程序globalData完成数据传递

**优点**：实现简单，实现理解。因为不读写localStorage，直接操作内存，所以相比方式1，速度更快，更可靠
**缺点**：同方式1一样，要注意globalData污染

#### 方式三：eventBus(或者叫PubSub)方式

要先实现一个PubSub，通过订阅发布实现通信。在发布事件时，激活对方方法，同时传入参数，执行事件的订阅方法

**缺点**：要非常注意重复绑定的问题

#### 方式四：gloabelData watcher方式（待了解）

现在数据绑定流行，结合redux单一store的思想，如果我们直接watch一个globalData,那么要通信，只需修改这个data值，通过water去激活调用。
同时修改的data值，本身就可以做为参数数据。

**优点**：数据驱动，单一数据源，便于调试
**缺点**：重复watch的问题还是存在，要想办法避免

#### 方式五：通过hack方法直接调用通信页面的方法

**优点**：一针见血，功能强大，可以向要通信页面做你想做的任何事。无需要绑定，订阅，所以也就不存在重复的情况
**缺点**：使用了__route__这个hack属性，可能会有一些风险

## 数据缓存

### uniapp

###### uni.setStorage(OBJECT)

将数据存储在本地缓存中指定的 key 中，会覆盖掉原来该 key 对应的内容，**异步**。

```js
uni.setStorage({
    key: 'storage_key',
    data: 'hello',
    success: function () {
        console.log('success');
    }
});
```

###### uni.setStorageSync(KEY,DATA)

将 data 存储在本地缓存中指定的 key 中，会覆盖掉原来该 key 对应的内容，**同步**。

data为需要存储的内容，只支持**原生类型**、及能够通过 JSON.stringify **序列化**的对象

```js
try {
    uni.setStorageSync('storage_key', 'hello');
} catch (e) {
    // error
}
```

其他的也类小程序

### 小程序

#### 存储

每个微信小程序都可以有自己的本地缓存，可以通过 [wx.setStorage](https://developers.weixin.qq.com/miniprogram/dev/api/storage/wx.setStorage.html)/[wx.setStorageSync](https://developers.weixin.qq.com/miniprogram/dev/api/storage/wx.setStorageSync.html)、[wx.getStorage](https://developers.weixin.qq.com/miniprogram/dev/api/storage/wx.getStorage.html)/[wx.getStorageSync](https://developers.weixin.qq.com/miniprogram/dev/api/storage/wx.getStorageSync.html)、[wx.clearStorage](https://developers.weixin.qq.com/miniprogram/dev/api/storage/wx.clearStorage.html)/[wx.clearStorageSync](https://developers.weixin.qq.com/miniprogram/dev/api/storage/wx.clearStorageSync.html)，[wx.removeStorage](https://developers.weixin.qq.com/miniprogram/dev/api/storage/wx.removeStorage.html)/[wx.removeStorageSync](https://developers.weixin.qq.com/miniprogram/dev/api/storage/wx.removeStorageSync.html) 对本地缓存进行读写和清理。

###### wx.setStorage(Object object)

```js
wx.setStorage({
  key:"key",
  data:"value"
})
```

###### wx.setStorageSync(string key, any data)

data是需要存储的内容。只支持**原生类型**、**Date**、及能够通过`JSON.stringify`**序列化**的对象。

```js
try {
  wx.setStorageSync('key', 'value')
} catch (e){
    
}
```



###### wx.getStorage(Object object)

```js
wx.getStorage({
  key: 'key',
  success (res) {
    console.log(res.data)
  }
})
```

###### wx.getStorageSync(string key)

```js
try {
  var value = wx.getStorageSync('key')
  if (value) {
    // Do something with return value
  }
} catch (e) {
  // Do something when catch error
}
```



###### wx.getStorageInfo(Object object)

```js
wx.getStorageInfo({
  success (res) {
    console.log(res.keys)//当前 storage 中所有的 key
    console.log(res.currentSize)//当前占用的空间大小, 单位 KB
    console.log(res.limitSize)//限制的空间大小，单位 KB
  }
})
```

###### wx.getStorageInfoSync()

```js
try {
  const res = wx.getStorageInfoSync()
  console.log(res.keys)
  console.log(res.currentSize)
  console.log(res.limitSize)
} catch (e) {
  // Do something when catch error
}
```



###### wx.removeStorage(Object object)

从本地缓存中移除指定 key

```js
wx.removeStorage({
  key: 'key',
  success (res) {
    console.log(res)
  }
})
```

###### wx.removeStorageSync(string key)

```js
try {
  wx.removeStorageSync('key')
} catch (e) {
  // Do something when catch error
}
```



###### wx.clearStorage(Object object)

清理本地数据缓存

```js
wx.clearStorage()
```

###### wx.clearStorageSync()

```js
try {
  wx.clearStorageSync()
} catch(e) {
  // Do something when catch error
}
```



## 路由与页面跳转

### uniapp中路由跳转

##### uni.navigateTo(OBJECT)

需要跳转的应用内非 tabBar 的页面的路径 

保留当前页面，跳转到应用内的某个页面，会隐藏页面，触发onHide，使用`uni.navigateBack`可以返回到原页面	

##### uni.redirectTo(Object）

需要跳转的应用内非 tabBar 的页面的路径

会关闭页面，触发onUnload

##### uni.reLaunch(OBJECT)

关闭所有页面，打开到应用内的某个页面

**注意：** 如果调用了 uni.preloadPage(OBJECT) 不会关闭，仅触发生命周期 `onHide`

需要跳转的应用内页面路径 , 路径后可以带参数。参数与路径之间使用?分隔，参数键与参数值用=相连，不同参数用&分隔；如 'path?key=value&key2=value2'，如果跳转的页面路径是 tabBar 页面则不能带参数

H5端调用`uni.reLaunch`之后之前页面栈会销毁，但是无法清空浏览器之前的历史记录，此时`navigateBack`不能返回，如果存在历史记录的话点击浏览器的返回按钮或者调用`history.back()`仍然可以导航到浏览器的其他历史记录。

##### uni.switchTab(OBJECT)

跳转到 tabBar 页面，并关闭其他所有非 tabBar 页面。

**注意：** 如果调用了 uni.preloadPage(OBJECT) 不会关闭，仅触发生命周期 `onHide`

需要跳转的 tabBar 页面的路径（需在 pages.json 的 tabBar 字段定义的页面），路径后不能带参数

跳转到 tabBar 页面只能使用 switchTab 跳转

##### uni.navigateBack(OBJECT)

关闭当前页面，返回上一页面或多级页面。可通过 `getCurrentPages()` 获取当前的页面栈，决定需要返回几层。

调用 navigateTo 跳转时，调用该方法的页面会被加入堆栈，而 redirectTo 方法则不会。

##### 总结注意点tips

- `navigateTo`, `redirectTo` 只能打开非 tabBar 页面。
- `switchTab` 只能打开 `tabBar` 页面。
- `reLaunch` 可以打开任意页面。
- 页面底部的 `tabBar` 由页面决定，即只要是定义为 `tabBar` 的页面，底部都有 `tabBar`。
- 不能在 `App.vue` 里面进行页面跳转。
- H5端页面刷新之后页面栈会消失，此时`navigateBack`不能返回，如果一定要返回可以使用`history.back()`导航到浏览器的其他历史记录。

### 小程序

类uniapp



## this.data.xxx

小程序读取data数据 要this.data.xxx，不是vue中直接this.xxx

如果是变量 要[] 括起来

##### 向event对象传值

###### 1、id —— event.currentTarget.id（表示唯一  

​	通过id向event传参的时候，如果传的是number类型，会自动转换成string

###### 2、data-key —— event.currentTarget.dataset.key （没有个数的限制

url里面不能直接用js对象，要先将js对象转为json对象

###### 注意点

初始化时需要什么数据，

离开的时候，将什么数据传出去

如何与其他组件或页面的数据 保持同步







