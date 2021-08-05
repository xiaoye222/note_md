## CSS布局

```css
#q-app, body, html {
    height: 100%;
    font-family: PingFangSC-Regular,PingFang SC,Roboto,-apple-system,Helvetica Neue,Helvetica,Arial,sans-serif;
}

#q-app, body, html {
    width: 100%;
    direction: ltr;
}

blockquote, body, button, code, dd, div, dl, dt, fieldset, form, h1, h2, h3, h4, h5, h6, input, legend, li, ol, p, pre, td, textarea, th, ul {
    margin: 0;
    padding: 0;
    outline: none;
    box-sizing: border-box;
}
```

布局

```css
.q-layout, .q-page {
    position: relative;
}

.q-layout {
    width: 100%;
    height: 100vh;
}
```



##### 固定头部

```css
.base-header[data-v-1542e1b8] {
    top: 0px;
    left: 0;
    right: 0;
    position: fixed;
    border-top: 2px solid #2672ff;
    display: -ms-flexbox;
    display: flex;
    -ms-flex-align: center;
    align-items: center;
    z-index: 2000;
}
```



##### flex相关

```css
.contact[data-v-0b3c5302] {
    -ms-flex: 1;
    flex: 1;
    position: relative;
    overflow: hidden;
}
```





##### height:100%与height:100vh的区别

vh表示当前屏幕可见高度的1%，也就是说

height:100vh == height:100%;

但是当元素没有内容时候，设置height:100%，该元素不会被撑开，此时高度为0，

但是设置height:100vh，该元素会被撑开屏幕高度一致。





##### height:100%与height:auto的区别

auto根据块内内容自动调节高度

100%是指其相对父块高度而定义的高度，也就是按照离它最近且有定义高度的父层的高度来定义高度







#### [浏览器渲染](https://juejin.cn/post/6844903565610188807)

注意 少**层叠，嵌套**

当解析html的时候，会把新来的元素插入dom树里面，同时去查找css，然后把对应的样式规则应用到元素上，查找样式表是按照**从右到左**的顺序去匹配的。

例如： div p {font-size: 16px}，会先寻找所有p标签并判断它的父标签是否为div之后才会决定要不要采用这个样式进行渲染）。

所以，平时写CSS时，尽量用id和class，不要**过渡层叠**





#### uniapp中scroll-view相关布局 

内容是可以滚动的，用到scroll-view ,把page height设置为100%

swipper  都是100%

swiper-item 都是100%



#### 显示等待，没有数据 组件提示







#### uniapp中flex布局

##### 导航栏固定

设置position：fixed以外，还要设置另一个view去对应高度的位置，配置page.json中的对应页面

			"style": {
				"navigationStyle":"custom",
				"navigationBarTitleText": ""
			}

内容过多要想滑动，overflow不是设置为hidden啊，好歹是scroll。



uniapp编译失败，json格式什么缺少 },检查 对象的最后一个属性，','是否去除。





swiper 轮播广告

scroll-view 滑动 滑动 滑动





#### 顶部原生导航栏

##### 添加搜索框

仅在app下有用，顶部原生导航栏需要在page.json对应页面设置app-plus

##### 缺点

扩展能力有限，在微信下，没有提供太多导航栏的配置。

[自定义顶部搜索框兼容小程序](https://www.cnblogs.com/guiyishanren/p/14135424.html)



## 基数知识点

### [伪类与伪元素](https://juejin.cn/post/6844903957655977998)

伪类与伪元素的根本区别在于：是否创造了新的元素

伪类元素：存在于DOM文档中，逻辑上存在但是在文档树中无需标识的“幽灵”分类。伪类的效果可以通过添加实际的类来实现。伪类只能用:

伪元素不存在DOM文档中，是虚拟的元素，在逻辑上存在，但实际并不存在。伪元素的效果可以通过实际的元素来实现。伪元素既能使用: 也可以使用:: 



#### 伪类

标签没有变化，有变化的只是状态。是为了向某些选择器添加特殊的效果或限制。

伪类是在正常的选择器后面加上伪类名称，中间用冒号隔开。

##### 用处

###### 标记一些特殊的状态

:link 表示没访问过的链接

:visited 选取访问过的超链接元素

:hover 选取鼠标悬停的元素

:active 选取点中的元素

:focus 选取获得焦点的元素

###### 具有筛选的功能

可以根据元素的特点或者索引来给特定的元素加上样式

:empty，选取没有子元素的元素。比如选择空的 span，就可以用 span:empty 选择器来选择。这里要注意元素内有空格的话也不能算空，不会被这个伪类选中。

:checked，选取勾选状态的 input 元素， 只对 radio 和 checkbox 生效。

:disabled，选取禁用的表单元素。

:first-child，选取当前选择器下第一个元素。

:last-child，和 first-child 相反，选取当前选择器下最后一个元素。

:nth-child(an+b)，选取指定位置的元素。这个伪类是有参数的，参数可以支持 an+b 的形式，这里 a 和 b 都是可变的，n 从0起。使用这个伪类可以做到选择第几个，或者选择序号符合 an+b 的所有元素。比如使用 li:nth-child(2n+1)，就可以选中 li 元素中序号是2的整数倍加1的所有元素，也就是第1、3、5、7、9、2n+1个 li 元素。

:nth-last-child(an+b)，这个伪类和 nth-child 相似，只不过在计数的时候，这个伪类是从后往前计数。

:only-child，选取唯一子元素。如果一个元素的父元素只有它一个子元素，这个伪类就会生效。如果一个元素还有兄弟元素，这个伪类就不会对它生效。

:only-of-type，选取唯一的某个类型的元素。如果一个元素的父元素里只有它一个当前类型的元素，这个伪类就会生效。这个伪类允许父元素里有其他元素，只要不和自己一样就可以。

#### 伪元素选择器

用于向某些元素设置特殊效果，选中的不是真实的dom元素，所以被称之为伪元素选择器。

::first-line，为某个元素的第一行文字使用样式。

::first-letter，为某个元素中的文字的首字母或第一个字使用样式。

::before，在某个元素之前插入一些内容。

::after，在某个元素之后插入一些内容。

::selection，对光标选中的元素添加样式。

### [CSS选择器及优先级](https://juejin.cn/post/6844904159305531406)

##### 样式类型

行内样式：`<style></style>`

内联样式：`<div style="color:red;">`

外部样式：`<link>或@import引入`

##### 选择器类型

id选择器、class选择器、属性选择器、*、伪类选择器、伪元素、后代选择器、子类选择器、兄弟选择器

#### 权重计算规则

- 第一优先级：`!important`会覆盖页面内任何位置的元素样式
- 1.内联样式，如`style="color: green"`，权值为`1000`
- 2.ID选择器，如`#app`，权值为`0100`
- 3.类、伪类、属性选择器，如`.foo, :first-child, div[class="foo"]`，权值为`0010`
- 4.标签、伪元素选择器，如`div::first-line`，权值为`0001`
- 5.通配符、子类选择器、兄弟选择器，如`*, >, +`，权值为`0000`
- 6.继承的样式没有权值

#### 优先级

!important>行内样式>ID选择器>类、伪类、属性>元素、伪元素>继承>通配符



### BFC——CSS世界的结界

block foamatting coontext，块级格式化上下文

##### 特性

如果一个元素具有BFC，内部子元素永远不会影响外部的元素，也不会受外部元素影响

不可能发生margin重叠，

因为margin重叠会影响外部的元素，而一个元素具有BFC，内部子元素永远不会影响外部的元素，也不受外部元素的影响，所以具有BFC的元素不可能发生[margin重叠](https://www.jianshu.com/p/661e6d4fd468)。

> margin重叠的意义  使得段落之间的距离保持一致

##### 应用场景

​	消除浮动的影响（如果不清除浮动的影响，子元素浮动则父元素高度塌陷，会影响后面元素布局与定位

##### 触发场景

​	html根元素

​	float的值不为none

​	overflow的值为auto、scroll或hidden

​	display的值为table-cell、table-caption和inline-block

> 只要元素符合上面任意一个条件，就无需使用clear:both清除浮动的影响



##### 最重要的用途

​	不是去margin重叠或者是清除float影响，而是实现更健壮、更智能的**自适应布局**

​	普通流体元素设置overflow:hidden后，会自动填满容器中除了浮动元素意外的剩余空间，行成自适应布局效果。

可用BFC实现**两栏布局**。

​	总结一下，对BFC 声明家族大致过了一遍，能担任自适应布局重任的也就是以下 几个。 

•overflow:auto/hidden，适用于 IE7 及以上版本浏览器； 

• display:inline-block，适用于 IE6 和 IE7； 

• display:table-cell，适用于 IE8 及以上版本浏览器。 

最后，可以提炼出两套 IE7 及以上版本浏览器适配的自适应解决方案。 

（1）借助 overflow 属性，如下： 

.lbf-content { overflow: hidden; }  

（2）融合 display:table-cell 和 display:inline-block，如下： 

.lbf-content {  display: table-cell; width: 9999px;  

/* 如果不需要兼容 IE7，下面样式可以省略 */  

*display: inline-block; *width: auto; } 

​		这两种基于 BFC 的自适应方案均支持无限嵌套，因此，多栏自适应可以通过嵌套方式实现。这两种方案均有一点不足，前者如果子元素要定位到父元素的外面可能会被隐藏， 后者无法直接让连续英文字符换行。

关于 display:table-cell 元素内连续英文字符无法换行的问题，事实上是可以解决的，就是使用类似下面的 CSS 代码： .word-break {  display: table;  width: 100%;  table-layout: fixed;  word-break: break-all;  }





#### 移动端适配

##### flexible rem(font size of the root element)

相对于根元素html元素font-size计算值的倍数

##### vw(viewport width)

###### vw 和 vh 

- `vw`,视窗宽度，1vw=视窗宽度的1%
- `vh`,视窗高度，1vh=视窗高度的1%
- 如果浏览器的高是900px,1vh求得的值为9px。同理，如果显示窗口宽度为750px,1vw求得的值为7.5px。

##### rpx

是信息噪和层序解决自适应屏幕尺寸的尺寸单位，微信小程序规定的屏幕宽度为750rpx.

无论是在iPhone6上面还是其他机型上面都是750rpx的屏幕宽度，拿iPhone6来讲，屏幕宽度为375px，把它分为750rpx后， 1rpx = 0.5px = 1物理像素。





#### 背景图像的虚化

```vue
<view class="my-header">
			<view class="my-header__bg">
				<image :src="userinfo.avatar" mode="aspectFill"></image>
			</view>

			<view class="my-header__logo">
				<view class="my-header__logo-box">
					<image :src="userinfo.avatar" mode="aspectFill"></image>
				</view>
				<text class="my-header__name">{{userinfo.author_name}}</text>
			</view>

    ......
```



```css
	.my-header {
		position: relative;
		padding-bottom: 15px;


		.my-header__bg {
			position: absolute;
			left: 0;
			right: 0;
			top: 0;
			bottom: 0;
			// 虚化 加一个模糊
			filter: blur(8px);
			opacity: 0.3;
			image {
				width: 100%;
				height: 100%;
			}
		}

		.my-header__logo {
			display: flex;
			flex-direction: column;
			align-items: center;
			padding-top: 30px;

			.my-header__logo-box {
				border-radius: 50%;
				width: 60px;
				height: 60px;
				overflow: hidden;

				image {
					width: 100%;
					height: 100%;
				}
			}
            
     ......
```



图片上传样式 九宫格

```css
	.feedback-image-box{
		display: flex;
		// 让能够换行
		flex-wrap: wrap;
		padding: 10px;
		.feedback-image-item{
			// 显示一个方形的内容
			box-sizing: border-box;
			position: relative;
			// 用padding-top撑开高度
			padding-top: 33.33%;
			width: 33.33%;
			// 高度设置为0，是为了让最外层成一个方形的显示
			height: 0;
			// 图片右上角的×号
			.close-icon{
				position: absolute;
				right: 0;
				top: 0;
				z-index: 2;
				display: flex;
				justify-content: center;
				align-items: center;
				width: 22px;
				height: 22px;
				border-radius: 50%;
				background-color: #ff5a5f;
			}
			.image-box{
				position: absolute;
				top: 5px;
				right: 5px;
				left: 5px;
				bottom: 5px;
				display: flex;
				justify-content: center;
				align-items: center;
				border: 1px solid #eee;
				border-radius: 5px;
				overflow: hidden;
				.image{
					width: 100%;
					height: 100%;
				}
			}
		}
		
	}
```



#### 路由与父子组件配置

子路由path设置为"",父子组件都能使用

```js
export default [
  {
    path: "/templates",
    component: () => import("src/layouts/blank"),
    children: [
      {
        path: "",       //设置为空，还能渲染子组，nice
        name: "templates",
        component: () => import("src/micro/templates/views/home/index.vue")
      },
```





#### [less与sass](https://juejin.cn/post/6844904169313140749#heading-20)

都是css预处理器

##### 使用的必要性

CSS是标记语言，不可以自定义变量，不可以引用。

无法嵌套书写，导致模块化开发需要书写很多重复的选择器，

没有变量和合理的样式复用机制，逻辑相关的属性值必须以字面量的形式重复输出，难以维护。

> SASS版本3.0之前的后缀名为.sass，而版本3.0之后的后缀名.scss。



#### 参考链接

sass vs less 简介与比较

https://juejin.cn/post/6844904169313140749#heading-20

sass总结笔记 基础入门

https://juejin.cn/post/6971458017267187719#comment

##### less

###### 变量借@定义

###### 混入（可定义类）

混合（Mixin）是一种将一组属性从一个规则集包含（或混入）到另一个规则集的方法。假设我们定义了一个类（class）如下：

```css
.bordered {
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}
```

如果希望在其它规则集中使用这些属性，只需像下面这样输入所需属性的类（class）名称即可

```less
#menu a {
  color: #111;
  .bordered();
}

.post a {
  color: red;
  .bordered();
}
```





###### 可嵌套(Nesting)

&表示当前选择器的父级

###### 运算

如果运算符两侧变量单位不同，在加、减或比较之前会进行单位换算。计算的结果**以最左侧**操作数的单位类型为准。

###### 函数

###### 导入

可以导入一个 .less 文件，此文件中的所有变量就可以全部使用了。如果导入的文件是 .less 扩展名，则可以将扩展名省略掉

```less
@import "library"; // library.less
@import "typo.css";
```





##### sass

###### 变量

sass使用$符号来标识变量(老版本的sass使用!来标识变量

###### 子组合选择器和同层组合选择器：>、+和~

###### 导入sass文件

css有一个特别不常用的特性，即@import规则，它允许在一个css文件中导入其他css文件。然而，后果是只有执行到@import时，浏览器才会去下载其他css文件，这导致页面加载起来特别慢。

sass也有一个@import规则，但不同的是，sass的@import规则在生成css文件时就把相关文件导入进来。这意味着所有相关的样式被归纳到了同一个css文件中，而无需发起额外的下载请求。

###### **@import** 引入一个.scss后缀的文件作为自己的一部分，被引入的那个文件并不会转换成.css格式的，所以此文件命名要注意以下划线开头，如：_base.scss ，引入它的时候不用写下划线。

语法：@import: ".....路径"; 如： 建立一个文件叫 _base.scss，里面写上一些选择器和样式，然后在一个正常的如one.scss文件里引入它,假如目录是同等级的：

```css
@import: "base.scss";
这样.one.scss就有了_base.scss里的全部内容。
```

###### 默认变量值;

`!default`用于变量，含义是：如果这个变量被声明赋值了，那就用它声明的值，否则就用这个默认值。

```saaa
$fancybox-width: 400px !default;
.fancybox {
width: $fancybox-width;
}
```

在上例中，如果用户在导入你的sass局部文件之前声明了一个`$fancybox-width`变量，那么你的局部文件中对`$fancybox-width`赋值400px的操作就无效。如果用户没有做这样的声明，则`$fancybox-width`将默认为400px。



#### [盒模型](https://juejin.cn/post/6880111680153059341#comment)、看一半，需要继续

盒模型由外而内包括 外边距margin、边框border、内边距padding、内容content

##### W3C标准盒模型

box-sizing:content-box

width=content

##### IE怪异盒模型

box-sizing:border-box

width=border-left+padding-left+content+padding-right+border-right

###### 描述一下下面盒子的大小，颜色什么的（content-box模型）

1. border-color的颜色默认跟字体颜色相同
2. padding颜色跟背景颜色相同

###### 当small盒子设置成圆形时，内容会超出圆形吗？为什么

为border-radius只是改变视觉上的效果，实际上盒子占据的空间还是不变的

###### 当元素设置成inline-block会出现什么问题？怎么消除？

真正意义上的inline-block水平呈现的元素间，换行显示或空格分隔的情况下会有间距

1. 元素间留白间距出现的原因就是标签段之间的空格，因此，去掉HTML中的空格，自然间距就木有了
2. 使用margin负值
3. 让闭合标签吃胶囊
4. 使用font-size:0

###### 行内元素可以设置padding，margin吗

行内元素（inline-block）的padding左右有效 ，但是由于设置padding上下不占页面空间，无法显示效果，所以无效

###### padding:1px2px3px;则等效于什么？

简单来说就是 **这四个值，分别代表上、右、下、左。如果没有写下的话，那就下复制上的，同理左复制右的值。**

因此

1. 当padding的值只有一个时，就是后面三个都复制了第一个
2. 当写两个时，就是写了top和right，因此bottom复制top，left复制right
3. 当写了三个时，就是写了top，right，bottom，因此left复制right。

###### 内边距的百分数值是这么计算的

根据父元素的宽度计算

###### 为什么不根据自己，而要根据父元素

不根据父元素，而是根据本身的宽度的话。那么当padding生效后，本身的宽度不就变大了吗？那么padding不是也要变大吗？这就陷入了死循环（哇塞！）。

或者要是本身没有宽度，那岂不是怎么设置padding都是无效的





#### [浮动](https://juejin.cn/post/6886247611318140942)

##### 浮动可能引起的问题

父盒子高度塌陷 

###### 清除浮动的方法

1、父盒子设定固定高度——使用不够灵活，应用于网页中盒子固定高度区域，比如固定的导航栏。

2、内墙法在浮动元素的后面加一个空的块级元素(通常是div),并且该元素设置`clear:both；`属性。clear属性，字面意思就是清除，那么both,就是清除浮动元素对我左右两边的影响——结构冗余，应用不多

3、伪元素清除法——*一定要有content*，然后clear:both；

​	`content:'.';`表示给`.clearfix`元素内部最后添加一个内容，该内容为行内元素。

​	`display:block;`设置该元素为块级元素，符合内墙法的需求。

​	`clear:both；`清除浮动的方法。必须要写

​	`overflow:hidden;height:0;`如果用`display:none;`,那么就不能满足该元素是块级元素了。	

​    `overflow:hidden;`表示隐藏元素，与`display:none;`不同的是，前者隐藏元素，该元素占位置，而后者不占位置

4、overflow：hidden——那么当一个元素设置了overflow:hidden|auto|scroll属性之后，它会形成一个BFC区域，我们叫做它为`块级格式化上下文`。BFC只是一个规则。它对浮动定位都很重要。浮动定位和清除浮动只会应用于同一个BFC的元素。

浮动不会影响其它BFC中元素的布局，而清除浮动只能清除同一BFC中在它前面的元素的浮动。

> 总结：只要我们让父盒子形成BFC的区域，那么它就会清除区域中浮动元素带来的影响。



#### 水平垂直居中

https://shimo.im/docs/dXqxxVpVhWqdKKgQ

#### 文档流

元素按照其在HTML中的位置顺序决定排布的过程。HTML的布局机制就是用文档流模型，块元素独占一行，内联元素不独占一行。

除了 **float**和**绝对定位方式**布局的，都在文档流里。

#### CSS定位的规则

##### position属性

##### static

默认值。位置设置为static的元素，它始终会处于文档流给予的位置

##### inherit

规定应该从父元素继承 position 属性的值。但是任何的版本的 Internet Explorer （包括 IE8）都不支持属性值 “inherit”。

##### fixed

生成绝对定位的元素。默认情况下，可定位于**相对于浏览器窗口**的指定坐标。

元素的位置通过 “left”, “top”, “right” 以及 “bottom” 属性进行规定。不论窗口滚动与否，元素都会留在那个位置。

但当祖先元素具有transform属性且不为none时，就会相对于**祖先元素**指定坐标，而不是浏览器窗口。

##### absoulute

生成绝对定位的元素，**相对于距该元素最近的已定位的祖先元素**进行定位。此元素的位置可通过 “left”、”top”、”right” 以及 “bottom” 属性来规定。

脱离文档流，根据它祖先元素的定位设置，参照物是 相对于该元素最近的已定位的祖先元素（只要设置了值不为position:static以外的值，都视为有定位，最近的祖先元素会被设置为绝对定位元素的参照物）

如果没有一个祖先定位元素设置定位，那么参照物是body层。

> 在用户没给absolute元素设置left/right、top/bottom的情况下，所对应的参考物会有变化。？？？？？？？？？？？？？

###### 未设置left/right top/bottom的情况

在没设置left/right、top/bottom的情况下，absolute元素的位置就是该元素在文档流里的位置，但是依旧脱离文档流

##### relative

生成相对定位的元素，**相对于该元素在文档中的初始位置进行定位**。通过 “left”、”top”、”right” 以及 “bottom” 属性来设置此元素相对于自身位置的偏移。

相对定位的参照物是**它本身**







#### 动画

##### transition 过渡

transition-duration transition效果需要指定多少秒或毫秒才能完成

transition-property 指定CSS属性的name，transition效果，默认为all

transition-timing-function 指定transition效果的转速曲线

transition-delay 定义transition效果开始的时候，即延迟的时常

###### 简写方式

transition:property duration timing-function delay

###### 总结

是一种状态的转变过程，需要一种条件来触发这种转变，比如常用的:hover :focus :checked、媒体查询或者js

###### 不足点

需要事件触发，所以没法在网页加载时自动发生

一次性的，不能重复发生，除非再次触发

只能定义开始状态和结束状态，不能定义中间状态，也就是说只有两个状态

~~一条transition规则，只能定义一个属性的变化，不能涉及多个属性~~

> 评论区矫正：一条transition规则，是可以定义多个属性的变化的，中间用逗号隔开，如果是全部属性变化则用“all”即可

##### animation 

transition的扩展，弥补transition的许多不足，可以理解为多个transition的效果叠加，可操作性更强

animation是 8 个属性的简写

| 属性                      | 描述                                                         |
| ------------------------- | ------------------------------------------------------------ |
| animation-duration        | 指定动画完成一个周期所需要时间，单位秒（s）或毫秒（ms），默认是 0。 |
| animation-timing-function | 指定动画计时函数，即动画的速度曲线，默认是 "ease"。          |
| animation-delay           | 指定动画延迟时间，即动画何时开始，默认是 0。                 |
| animation-iteration-count | 指定动画播放的次数，默认是 1。                               |
| animation-direction       | 指定动画播放的方向。默认是 normal。                          |
| animation-fill-mode       | 指定动画填充模式。默认是 none。                              |
| animation-play-state      | 指定动画播放状态，正在运行或暂停。默认是 running。           |
| animation-name            | 指定 @keyframes 动画的名称。                                 |

###### @keyframes

定义关键帧，提供更多的控制，尤其是时间轴的控制， `animation-name` 来指定动画使用的关键帧

###### animation-delay 设置延迟时间

延迟可以设置为**负**，负延迟表示动画仿佛开始前就**已经运行过**这么长时间

######  animation-play-state

`CSS` 动画是可以暂停的。属性 `animation-play-state` 表示动画播放状态，默认值 `running` 表示播放， `paused` 表示暂停

可以与负延迟一起实现特殊的效果，比如进度条插件

###### animation-iteration-count

表示动画播放次数。它很好懂，只有一点要注意，无限播放时使用 `infinite`

###### animation-fill-mode 动画填充模式 

- forwards，表示，动画完成后，元素状态保持为最后一帧的状态。——可以应用于开发进度条
- backwards，表示，有动画延迟时，动画开始前，元素状态保持为第一帧的状态。
- both，表示上述二者效果都有。

##### transition与animation

简单一次性的动画推荐使用transition，逻辑清晰，可维护性较好

遇到比较复杂的动画，可以使用animation

不仅可以使用css实现动画，js同样可以控制元素的样式实现动画

​	setTimeout，setInterval可以，requestAnimationFrame更好，更高性能执行动画。

##### transform

###### 语法

```js
div { transform: xxx();
	  transform-origin:center; } 

transform 用来指定一个变形函数对元素执行变形操作
transform-origin 用于指定一个元素变形的原点
如果指定一个值：left / center / right / top / bottom
如果指定两个值，其中一个必须是length,percentage，或left, center, right关键字中的一个。另一个必须是 length,percentage，或top, center, bottom关键字中的一个。
如果指定三个值，前两个值和只有两个值时的用法相同，第三个值必须是length。它始终代表Z轴偏移量
```

###### 旋转-rotate

该函数定义了一种将元素围绕一个定点旋转而不变形的转换。指定的角度定义了旋转的量度。若角度为正，则顺时针方向旋转，否则逆时针方向旋转。

rotateX (angle) 绕X轴旋转，例如单杠运动 30deg
rotateY (angle) 绕Y轴旋转，例如钢管舞运动
rotateZ (angle) 绕Z轴旋转，例如旋转盘。
rotate (angle) 与rotateZ()一致。

###### 倾斜-skew

skew(ax,ay) 函数表示对元素的剪切或者扭转，ax表示水平方向的扭转，ay表示垂直方向的扭转，也可以使用skewX(ax) 和skewY(ay)

###### 缩放-scale

scale() 函数能够更改元素的大小，scale变换的是通过矢量来实现，它的坐标定义了每个方向上的缩放比例。如果两个向量的坐标是相等的，缩放是均匀的

###### 移动-translate

translate(tx, ty) 函数能够移动元素。tx为元素在水平方向上移动的距离，ty为元素 在垂直方向上移动的距离。



#### [flex布局](https://zhuanlan.zhihu.com/p/25303493) 

![flex布局](C:\Users\刘颖\Documents\tips截图\【CSS】\flex布局.png)

##### Flex容器

首先flex布局需要先指定一个容器，任何一个容器都可以被指定为flex布局，主要容器内的元素就可以使用flex来进行布局

```css
.container {
    display: flex | inline-flex;       //可以有两种取值
}
```

> **当时设置 flex 布局之后，子元素的 float、clear、vertical-align 的属性将会失效。**

##### 设置在flex容器上的属性

###### flex-direction

**flex-direction: 决定主轴的方向(即项目的排列方向)**

```css
.container {
    flex-direction: row | row-reverse | column | column-reverse;
}


/*
默认值：row，主轴为水平方向，起点在左端。
row-reverse：主轴为水平方向，起点在右端
column：主轴为垂直方向，起点在上沿
column-reverse：主轴为垂直方向，起点在下沿

*/
```

###### flex-wrap

**决定容器内项目是否可换行**

```css
默认情况下，项目都排在主轴线上，使用 flex-wrap 可实现项目的换行。

.container {
    flex-wrap: nowrap | wrap | wrap-reverse;
}
/*
默认值：nowrap 不换行，即当主轴尺寸固定时，当空间不足时，项目尺寸会随之调整而并不会挤到下一行。
warp: 项目主轴总尺寸超出容器时换行，第一行在上方
wrap-reverse：换行，第一行在下方
*/
```

###### flex-flow

flex-direction和flex-wrap的简写

```css
.container {
    flex-flow: <flex-direction> || <flex-wrap>;
}
/*
默认值为: row nowrap，感觉没什么卵用，老老实实分开写就好了。这样就不用记住这个属性了
*/
```

###### justify-content

**justify-content：定义了项目在主轴的对齐方式。**

```css
.container {
    justify-content: flex-start | flex-end | center | space-between | space-around;
}
```

###### align-items

定义了项目在交叉轴上的对齐方式

```css
.container {
    align-items: flex-start | flex-end | center | baseline | stretch;
}
```

###### align-content

多根轴线的对齐方式，如果项目只有一根轴线，那么该属性将不起作用



##### Flex项目属性

运用在item项目上

###### order

定义项目在容器中的排列顺序，数值越小，排列越靠前，默认值为 0

###### flex-basis

定义了在分配多余空间之前，项目占据的主轴空间，浏览器根据这个属性，计算主轴是否有多余空间

```css
.item {
    flex-basis: <length> | auto;
}
```

默认值：auto，即项目本来的大小, 这时候 item 的宽高取决于 width 或 height 的值。

**当主轴为水平方向的时候，当设置了 flex-basis，项目的宽度设置值会失效，flex-basis 需要跟 flex-grow 和 flex-shrink 配合使用才能发挥效果。**

- 当 flex-basis 值为 0 % 时，把该项目视为零尺寸的，故即使声明该尺寸为 140px，也并没有什么用。
- 当 flex-basis 值为 auto 时，则跟根据尺寸的设定值(假如为 100px)，则这 100px 不会纳入剩余空间。

###### flex-grow

定义项目的放大比例

```css
.item {
    flex-grow: <number>;
}
```

默认值为 0，即如果存在剩余空间，也不放大。

当所有的项目都以 flex-basis 的值进行排列后，仍有剩余空间，那么这时候 flex-grow 就会发挥作用了。

如果所有项目的 flex-grow 属性都为 1，则它们将等分剩余空间。(如果有的话)

如果一个项目的 flex-grow 属性为 2，其他项目都为 1，则前者占据的剩余空间将比其他项多一倍。

当然如果当所有项目以 flex-basis 的值排列完后发现空间不够了，且 flex-wrap：nowrap 时，此时 flex-grow 则不起作用了，这时候就需要接下来的这个属性。

###### flex-shrink

定义了项目的缩小比例

```css
.item {
    flex-shrink: <number>;
}
```

默认值: 1，即如果空间不足，该项目将缩小，负值对该属性无效。

如果所有项目的 flex-shrink 属性都为 1，当空间不足时，都将等比例缩小。

如果一个项目的 flex-shrink 属性为 0，其他项目都为 1，则空间不足时，前者不缩小。

###### flex

flex-grow, flex-shrink 和 flex-basis的简写

```css
.item{
    flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
} 
```

flex 的默认值是以上三个属性值的组合。假设以上三个属性同样取默认值，则 flex 的默认值是 0 1 auto。

有关快捷值：auto (1 1 auto) 和 none (0 0 auto)

关于 flex 取值，还有许多特殊的情况，可以按以下来进行划分：

- 当 flex 取值为一个非负数字，则该数字为 flex-grow 值，flex-shrink 取 1，flex-basis 取 0%，如下是等同的：

```css
.item {flex: 1;}
.item {
    flex-grow: 1;
    flex-shrink: 1;
    flex-basis: 0%;
}
```

- 当 flex 取值为 0 时，对应的三个值分别为 0 1 0%

```css
.item {flex: 0;}
.item {
    flex-grow: 0;
    flex-shrink: 1;
    flex-basis: 0%;
}
```

- 当 flex 取值为一个长度或百分比，则视为 flex-basis 值，flex-grow 取 1，flex-shrink 取 1，有如下等同情况（注意 0% 是一个百分比而不是一个非负数字）

```css
.item-1 {flex: 0%;}
.item-1 {
    flex-grow: 1;
    flex-shrink: 1;
    flex-basis: 0%;
}

.item-2 {flex: 24px;}
.item-2 {
    flex-grow: 1;
    flex-shrink: 1;
    flex-basis: 24px;
}
```

- 当 flex 取值为两个非负数字，则分别视为 flex-grow 和 flex-shrink 的值，flex-basis 取 0%，如下是等同的：

```css
.item {flex: 2 3;}
.item {
    flex-grow: 2;
    flex-shrink: 3;
    flex-basis: 0%;
}
```

- 当 flex 取值为一个非负数字和一个长度或百分比，则分别视为 flex-grow 和 flex-basis 的值，flex-shrink 取 1，如下是等同的：

```css
.item {flex: 11 32px;}
.item {
    flex-grow: 11;
    flex-shrink: 1;
    flex-basis: 32px;
}
```

建议优先使用这个属性，而不是单独写三个分离的属性。

grow 和 shrink 是一对双胞胎，grow 表示伸张因子，shrink 表示是收缩因子。

grow 在 flex 容器下的子元素的宽度和比容器和小的时候起作用。 grow 定义了子元素的尺寸增长因子，容器中除去子元素之和剩下的尺寸会按照各个子元素的 grow 值进行平分加大各个子元素上。

###### flex-wrap 与子项的 flex-shrink、flex-grow 

1. 当 flex-wrap 为 wrap | wrap-reverse，且子项宽度和不及父容器宽度时，flex-grow 会起作用，子项会根据 flex-grow 设定的值放大（为0的项不放大）
2. 当 flex-wrap 为 wrap | wrap-reverse，且子项宽度和超过父容器宽度时，首先一定会换行，换行后，每一行的右端都可能会有剩余空间（最后一行包含的子项可能比前几行少，所以剩余空间可能会更大），这时 flex-grow 会起作用，若当前行所有子项的 flex-grow 都为0，则剩余空间保留，若当前行存在一个子项的 flex-grow 不为0，则剩余空间会被 flex-grow 不为0的子项占据
3. 当 flex-wrap 为 nowrap，且子项宽度和不及父容器宽度时，flex-grow 会起作用，子项会根据 flex-grow 设定的值放大（为0的项不放大）
4. 当 flex-wrap 为 nowrap，且子项宽度和超过父容器宽度时，flex-shrink 会起作用，子项会根据 flex-shrink 设定的值进行缩小（为0的项不缩小）。但这里有一个较为特殊情况，就是当这一行所有子项 flex-shrink 都为0时，也就是说所有的子项都不能缩小，就会出现讨厌的横向滚动条
5. 总结上面四点，可以看出不管在什么情况下，在同一时间，flex-shrink 和 flex-grow 只有一个能起作用，这其中的道理细想起来也很浅显：空间足够时，flex-grow 就有发挥的余地，而空间不足时，flex-shrink 就能起作用。当然，flex-wrap 的值为 wrap | wrap-reverse 时，表明可以换行，既然可以换行，一般情况下空间就总是足够的，flex-shrink 当然就不会起作用

###### align-self

允许单个项目有与其他项目不一样的对齐方式

单个项目覆盖 align-items 定义的属性

默认值为 auto，表示继承父元素的 align-items 属性，如果没有父元素，则等同于 stretch。

```css
.item {
     align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

这个跟 align-items 属性时一样的，只不过 align-self 是对单个项目生效的，而 align-items 则是对容器下的所有项目生效的。

#### CSS3新特性

##### 背景

###### background-origin

规定background-position相对于什么位置定位。通俗来说，盒子的大小有三部分组成：border, padding, content，当设置背景图片时，图片是会以左上角对齐，但是是以border的左上角对齐还是以padding的左上角或者content的左上角对齐，background-origin正是用来设置这个。

主要有三个取值

- border-box
- padding-box
- content-box

默认是padding-box，即以padding的左上角为原点。

###### background-clip

该属性是用来设置背景(背景图片、背景颜色)延伸的范围，有4个值可选

- border-box：背景延伸至边框外沿(但是在边框下层)
- padding-box：背景延伸至内边距(padding)外沿,不会绘制到边框处
- content-box：背景被裁剪至内容区(content box)外沿
- text：背景被裁剪成文字的前景色

###### background-size

background-size用以设置背景图片大小。单张图片的背景大小可以使用以下三种方法中的一种来规定：

- 使用关键词 contain

- 使用关键词 cover

- 设定宽度和高度值

  contain和cover会等比例缩放图片，以使得图片能够最大的被完整包含或者最小的覆盖背景区。

##### 边框

