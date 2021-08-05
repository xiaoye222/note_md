## html

#### 语义化

##### 优点

为了在没有CSS的情况下，页面也能呈现出很好地内容结构、代码结构——为了裸奔时好看

用户体验：例如title、alt用于解释名词或解释图片信息的标签尽量填写有含义的词语、label标签的活用

有利于搜索引擎的建立索引、抓取

有利于不同设备的解析（屏幕阅读器，盲人阅读器等）满是div的页面这些设备如何区分那些是主要内容优先阅读

有利于构建清晰的机构，有利于团队的开发、维护

##### 注意点

尽可能少的使用无语义的标签div和span；

在语义不明显时，既可以使用div或者p时，尽量用p, 因为p在默认情况下有上下间距，对兼容特殊终端有利；

不要使用纯样式标签，如：b、font、u等，改用css设置。需要强调的文本，可以包含在strong或者em标签中（浏览器预设样式，能用CSS指定就不用他们），strong默认样式是加粗（不要用b），em是斜体（不用i）

使用表格时，标题要用caption，表头用thead，主体部分用tbody包围，尾部用tfoot包围。表头和一般单元格要区分开，表头用th，单元格用td

表单域要用fieldset标签包起来，并用legend标签说明表单的用途

每个input标签对应的说明文本都需要使用label标签，并且通过为input设置id属性，在lable标签中设置for=someld来让说明文本和相对应的input关联起来。

##### html5新增

###### header

header元素代表“网页“和”section”的页眉。通常包含H1~H6元素或者hgroup元素。作为整个页面或者内容块的标题，也可以包裹一节的目录部分，一个搜索框，一个nav，或者任何相关logo。整个页面没有限制header元素的个数，可以拥有多个，可以为每个内容块增加一个header元素

###### footer

footer元素代表“网页”或“section”的页脚，通常含有该页面的一些基本信息，例如：文档创作者的姓名、文档的版权信息、使用条款的链接、联系信息等等。

###### hgroup元素

hgroup元素代表“网页”或“section”的标题，当元素有多个层级时，该元素可以将h1到h6元素放在其内，譬如文章的主标题和副标题的组合

hgroup使用注意：

- 如果只需要一个h1-h6标签就不用hgroup
- 如果有连续多个h1-h6标签就用hgroup
- 如果有连续多个标题和其他文章数据，h1-h6标签就用hgroup包住，和其他文章元数据一起放入header标签

###### nav元素

nav元素代表页面的导航链接区域。用于定义页面的主要导航部分。事实上规范上说nav只能用在页面主要导航部分上。页脚区域中的链接列表，虽然指向不同网站的不同区域，譬如服务条款，版权页等，这些footer元素就能够用了



###### aside元素

aside元素被包含在article元素中作为主要内容的附属信息部分，其中的内容可以是与当前文章有关的相关资料、标签、名词解释等。（特殊的section）

在article元素之外使用作为页面或站点全局的附属信息部分。最典型的是侧边栏，其中的内容可以是日志串连，其他组的导航，甚至广告，这些内容相关的页面。

###### article元素

article元素最容易跟section和div容易混淆，其实article代表一个在文档，页面或者网站中自成一体的内容，其目的是为了让开发者独立开发或重用。譬如论坛的帖子，博客上的文章，一篇用户的评论，一个互动的widget小工具。（特殊的section）

除了它的内容，article会有一个标题（通常会在header里），会有一个footer页脚。



#### [常用页面标签的默认样式、自带属性](https://blog.csdn.net/ferrysoul/article/details/106087052)

##### 默认样式

```css
li {display: list-item }`/*默认以列表显示*/`
head {display: none }/*默认不显示*/
table {display: table }/*默认为表格显示*/
tr {display: table-row }/*默认为表格行显示*/
thead {display: table-header-group }/*默认为表格头部分组显示*/
tbody {display: table-row-group }/*默认为表格行分组显示*/
tfoot {display: table-footer-group }/*默认为表格底部分组显示*/
col {display: table-column }/*默认为表格列显示*/
colgroup {display: table-column-group }/*默认为表格列分组显示*/
td, th { display:table-cell; }/*默认为单元格显示*/
caption {display: table-caption }/*默认为表格标题显示*/
th {font-weight: bolder; text-align: center }/*默认为表格标题显示，呈现加粗居中状态*/
caption {text-align: center }/*默认为表格标题显示，呈现居中状态*/
body {margin: 8px; line-height: 1.12 }
h1 {font-size: 2em; margin: .67em 0 }
h2 {font-size: 1.5em; margin: .75em 0 }
h3 {font-size: 1.17em; margin: .83em 0 }
h4, p,blockquote, ul, fieldset, form, ol, dl, dir, menu { margin: 1.12em 0 }
h5 {font-size: .83em; margin: 1.5em 0 }
h6 {font-size: .75em; margin: 1.67em 0 }
h1, h2,h3, h4, h5, h6, b,strong { font-weight: bolder }
blockquote{ margin-left: 40px; margin-right: 40px }
i, cite,em,var, address { font-style: italic }
pre, tt,code, kbd, samp { font-family: monospace }
pre {white-space: pre }
button,textarea, input, object, select { display:inline-block; }
big {font-size: 1.17em }
small,sub, sup { font-size: .83em }
sub {vertical-align: sub }/*定义sub元素默认为下标显示*/
sup {vertical-align: super }/*定义sub元素默认为上标显示*/
table {border-spacing: 2px; }
thead,tbody, tfoot { vertical-align: middle }/*定义表头、主体表、表脚元素默认为垂直对齐*/
td, th {vertical-align: inherit }/*定义单元格、列标题默认为垂直对齐默认为继承*/
s, strike,del { text-decoration: line-through }/*定义这些元素默认为删除线显示*/
hr {border: 1px inset }/*定义分割线默认为1px宽的3D凹边效果*/
ol, ul,dir, menu, dd { margin-left: 40px }
ol {list-style-type: decimal }
ol ul, ulol, ul ul, ol ol { margin-top: 0; margin-bottom: 0 }
u, ins {text-decoration: underline }
br:before{ content: ""A" }/*定义换行元素的伪对象内容样式*/
```

##### 自带属性

```css
:before,:after { white-space: pre-line }/*定义伪对象空格字符的默认样式*/
center {text-align: center }
abbr,acronym { font-variant: small-caps; letter-spacing: 0.1em }
:link,:visited { text-decoration: underline }
:focus {outline: thin dotted invert }
/* Beginbidirectionality settings (do not change) */
BDO[DIR="ltr"]{ direction: ltr; unicode-bidi: bidi-override }/*定义BDO元素当其属性为DIR="ltr"时的默认文本读写显示顺序*/
BDO[DIR="rtl"]{ direction: rtl; unicode-bidi: bidi-override }/*定义BDO元素当其属性为DIR="rtl"时的默认文本读写显示顺序*/
*[DIR="ltr"]{ direction: ltr; unicode-bidi: embed }/*定义任何元素当其属性为DIR="ltr"时的默认文本读写显示顺序*/
*[DIR="rtl"]{ direction: rtl; unicode-bidi: embed }/*定义任何元素当其属性为DIR="rtl"时的默认文本读写显示顺序*/
@mediaprint { /*定义标题和列表默认的打印样式*/
h1 {page-break-before: always }
h1, h2,h3, h4, h5, h6 { page-break-after: avoid }
ul, ol, dl{ page-break-before: avoid }
}
```



#### 标签样式初始化

浏览器会给一些元素添加一些css属性的初始值，但是这些属性值可能会影响到项目开发，造成布局与理想布局不一致。

#### [元信息类标签](https://juejin.cn/post/6844903870896963592)

描述自身信息，html文档用于描述自身信息一类的标签，一般提供给浏览器或者搜索引擎

##### title

表示文档的标题，会显示在浏览器的标题栏或页签栏上

##### head

作为其他标签的承载容器出现，本身并不携带任何信息，且必须包含一个title标签，如果作为iframe出现，则不必须包含title标签

##### meta

meta是一种通用的元信息表示标签，head中可以包含多个meta标签，一般由meta标签里的name和content定义，name表示名称，content表示对应的值

meta共有三个属性:1.name属性 2.http-equiv属性 3.scheme 属性，如今scheme属性已经基本废除了。

###### name

主要用于描述网页，包括网页的版权信息，作者，关键字等等。与之对应的属性值为content.可以将content理解为name的取值或者描述

```html
<meta name='参数' content='参数取值'>  //name属性语法格式
```

name属性有以下几种常用的参数。

- keywords 关键字告诉搜索引擎 网页的关键字。

```html
<meta name='keywords content='测试,前端,掘金'>  //例子
```

- author 作者用于标记网页的作者。

```html
<meta name='author' content='zhijie'>  //例子
```

- description 描述告诉搜索引擎 网页的内容描述。

```html
<meta name='description' content='这是一个例子'> //例子
```

- generator 网页制作软件用于标记网页是用什么软件开发。

```html
<meta name='generator' content='Sublime Text3'> //例子
```

- copyright(版权)用于标记版权信息。

```html
<meta name='copyright' content='zhijie所有'> //例子
```

- viewport 移动端的窗口

这个属性较为复杂,主要是用于设计移动端的网页。传送地址:[viewport 传送门](https://juejin.im/post/6844903721697017864)

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale:1.0, user-scalable:no">
```

- robots用于告诉搜索引擎，哪些页面需要被爬虫爬取，哪些页面不需要被爬虫爬取。content的取值有以下几种:

1.all(默认):文件被检索，而且页面上的链接可以被查询。

2.none:与all对应，文件不会被检索，而且页面上的链接也不可以被查询。

3.index:文件被检索。

4.noindex:与index对应，文件将不被检索，但页面上的链接可以被查询。

5.follow:页面上的链接可以被查询。

6.nofollow:与follow对应，文件将被检索，但页面上的链接不可以被查询。

```html
<meta name='robots' content'all'> //例子
<meta name='robots' content'none'> //例子
<meta name='robots' content'index'> //例子
<meta name='robots' content'noindex'> //例子
<meta name='robots' content'follow'> //例子
<meta name='robots' content'nofollow'> //例子
```

- topic 话题用于告诉搜索引擎，网页的话题。

```html
<meta name='topic' content'meta 标签'> //例子
```

- subject 主题用于告诉搜索引擎，网页的主题。

```html
<meta name='subject' content='meta 标签'> 
```

###### http-equip

http-equiv属性可以简单理解为与http协议相关的属性。如果`<meta>`标签不加name属性，content的值会默认对应http-equiv。

- content-type用来设置网页的编码字符集。

```html
<meta http-equiv='content-type' content='text/html; charset=utf-8'>
```

上面的写法比较少，但是下面这种写法，基本每个页面都会有的。

```html
<meta charset='utf-8'>
```

从html5开始，meta标签新增了简化写法charset，拥有包含charset写法的meta，则无需再写name和content属性，在创建html时，定义编码格式的写法如上。



- expires

这个属性设定网页的到期时间。一旦网页过期，必须到服务器上重新传输。使用GMT的时间格式。

```html
<meta http-equiv="expires" content="31 Dec 2008"> //GMT时间格式
```

- refresh

这个属性表示x秒之后重定向到新的链接。如果没有指定地址，则X秒后刷新页面。

```html
<meta http-equiv='refresh' content="5;url=http://xxx.com"> //表示5秒后重定向
```

- cache-control

这个属性用于指定网页的缓存机制。content的取值有以下几种：

1.no-cache: 这个属性并不是说不缓存，而是要求进行协商缓存。

2.no-store：这个属性才是强行页面不进行缓存，每次都必须从服务器获取新的资源。

3.private：指示对于单个用户的整个或部分响应消息，不能被共享缓存处理。

4.public：指示响应可被任何缓存区缓存。

```html
<meta http-equiv="cache-control" content="no-cache">
<meta http-equiv="cache-control" content="no-store">
<meta http-equiv="cache-control" content="private">
<meta http-equiv="cache-control" content="public">
```

- X-UA-Compatible

这个属性用于告诉浏览器以什么模式渲染页面。

```html
<meta http-equiv='X-UA-Compatible' content="IE=edge, chrome=1"> //以最新的模式渲染页面
```



#### [html5离线缓存](http://igeekbar.com/igeekbar/post/306.htm)

html5之前的网页，都是无连接，必须联网才能访问

到了移动互联网时代，设备终端位置不再固定，依赖无线信号，网络的可靠性降低

manifest



