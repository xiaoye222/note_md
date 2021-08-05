# JS基础

## 数据类型

### 数据类型分类

#### 原始类型

- `boolean`
- `null`
- `undefined`
- `number`
- `string`
- `symbol`

#### 对象（引用）类型

数组

对象

函数

### 数据类型判断

#### typeof

`typeof` 对于原始类型来说，除了 `null` 都可以显示正确的类型

```js
typeof 1 // 'number'
typeof '1' // 'string'
typeof undefined // 'undefined'
typeof true // 'boolean'
typeof Symbol() // 'symbol'
```

`typeof` 对于对象来说，除了函数都会显示 `object`，所以说 `typeof` 并不能准确判断变量到底是什么类型

```js
typeof [] // 'object'
typeof {} // 'object'
typeof console.log // 'function'
typeof Object 'function'
```

#### instanceof

如果我们想判断一个对象的正确类型，这时候可以考虑使用 `instanceof`，因为内部机制是通过原型链来判断的，在后面的章节中我们也会自己去实现一个 `instanceof`。

```js
const Person = function() {}
const p1 = new Person()
p1 instanceof Person // true

var str = 'hello world'
str instanceof String // false

var str1 = new String('hello world')
str1 instanceof String // true
```

### 数据类型转换

### 转Boolean

在条件判断时，除了 `undefined`， `null`， `false`， `NaN`， `''`， `0`， `-0`，其他所有值都转为 `true`，包括所有对象。

```js
const e=''
const f=undefined
const g=null
const h=false
const i=0
const j=-0
console.log(!e); //true
console.log(!f); //true
console.log(!g); //true    !null 
console.log(!h); //true
console.log(!i); //true
console.log(!j); //true
```

##### 应用场景

属性项为数组，要求必须选择一项，不能为空，对应的条件判断

```js
      if (!this.selectedPreviewList.length) {
        //this.selectedPreviewList.length==0
        this.$message.error("请至少选择一道题目预览");
        return;
      }
```

## 数组、对象、字符串常用方法

### 数组

##### Array.includes()

判断是否包含某一元素，除了不能定位外，解决了indexOf的上述的两个问题。它直接返回true或者false表示是否包含元素，对NaN一样能有有效。

ES5，Array已经提供了indexOf用来查找某个元素的位置，如果不存在就返回-1，但是这个函数在判断数组是否包含某个元素时有两个小不足，第一个是它**会返回-1和元素的位置来表示是否包含**，在定位方面是没问题，就是**不够语义化**。另一个问题是**不能判断是否有NaN的元素**。

#### 迭代

##### map方法 

###### **常见应用场景**

后台返回一组数据，每个数据项包含许多信息，前端只需要显示每个数据项中的几个信息，用map返回新的只包含几个所需信息的数据。

...item 



##### forEach方法

forEach会改变原数组的情况是 数组元素是一个引用类型的数据，forEach里的回调函数对引用类型的数组元素的属性有修改操作。

如果直接修改整个元素，不是修改某元素的某个属性，那么原数组不会改变

![](C:\Users\刘颖\Documents\tips截图\【基础】forEach是否修改原数组.png)

##### filter()

过滤符合条件的元素

##### some()

some与every  

函数需要有return，只有一行代码，同时删去{}与return

#### 转换方法

##### toString()与valueOf()

toString()  返回由数组中每个值的字符串形式拼接而成的一个以逗号分隔的字符串

valueOf() 返回的还是数组

```js
arr1=[1,"bww","9",{name:'bww',age:14}]

console.log(arr1.toString());//1,bww,9,[object Object]

console.log(arr1.valueOf());//[ 1, 'bww', '9', { name: 'bww', age: 14 } ]
```

#### 重排序方法

sort不能完全实现想要的效果

#### 操作方法

##### concat() 

没有参数时只复制当前数组并返回副本，参数为一个或多个数组，会将数组中的每一项都添加到结果数组中

##### slice() 

返回起始位置和结束位置之间的所有项   左开右闭

##### splice()

三个参数 从f哪开刀，删几个，补几个

#### ES6新增扩充

##### Array.from() 方法

用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象（包括ES6新增的数据结构Set和Map）

###### 类数组对象使用（array-like object）

```javascript
let arrayLike = { '0': 'a', '1': 'b', '2': 'c', length: 3 };
console.log(Array.from(arrayLike))
//[ 'a', 'b', 'c' ]
```

###### 拥有迭代器的数据

只要是部署了Iterator接口的数据结构，Array.from都能将其转为数组

```javascript
// 拥有迭代器的数据
console.log(Array.from('hello'))
//[ 'h', 'e', 'l', 'l', 'o' ]
```

##### Array.of()

用于将一组值转换为数组。

```javascript
let arr = new Array(1, 2, 'a', true)
console.log(arr)//[ 1, 2, 'a', true ]   ES5中的方法
console.log(Array.of(3, 4, 'b', false))//[ 3, 4, 'b', false ] 使用Arrry.of()实现同样的功能
```

这个方法的主要目的，是弥补数组构造函数Array()的不足。因为参数个数的不同，会导致Array()的行为有差异。

```javascript
let arr = new Array(3)
console.log(arr)//[ <3 empty items> ]
console.log(Array.of(3, 'a', false))//[ 3, 'a', false ]
console.log(Array.of(3))//[ 3 ]
```

##### 数组实例的find()

- 只返回一个符合条件的**值**，如果没有符合条件的，返回**undefined**
- 返回的是当前的数据
- find()接受一个回调函数作为参数
- 回调函数的参数（当前值，当前所处的位置，原数组） 这边都是形参，叫什么无所谓

```javascript
let arr = [5, 6, 3, 9, 1]
//希望找到里面大于5的数据
let result = arr.find((item, index, arr) => {
    return item > 5;
});
console.log(result);
//6
```

##### 数组实例的findIndex()

- 数组实例的findIndex方法的用法与find方法非常类似
- 返回第一个符合条件的数组成员的**位置**
- 如果所有成员都不符合条件，则返回**-1**

```javascript
let arr = [5, 6, 3, 9, 1]
let  result2  =  arr.findIndex((item,  index,  arr)  =>  {  
    return  item  >  5;
});
console.log(result2);
//1
```

##### 数组实例的fill()

- fill方法使用给定值，填充一个数组
- 改变原数组

```javascript
let arr = [5, 6, 3, 9, 1]
let res = arr.fill(10);
console.log(res);
console.log(arr === res);
//[ 10, 10, 10, 10, 10 ]
//true
```

##### 数组实例的entries(),keys(),values()

- 都用于遍历数组，都返回一个遍历器对象

```javascript
let arr = [23, 45, 67, 1, 4, 89]
let keys = arr.keys();
let values = arr.values();
let entries = arr.entries();
console.log(keys);
console.log(values);
console.log(entries);
//打印结果：
//Object [Array Iterator] {}
//Object [Array Iterator] {}
//Object [Array Iterator] {}
```

- 可以用**for...of**循环进行遍历

###### keys()

```javascript
let arr = [23, 45, 67, 1, 4, 89]
let keys = arr.keys();
for (var key of keys) {
    console.log(key);
}
//打印结果： 索引
0
1
2
3
4
5
let arr = [23, 45, 67, 1, 4, 89]
let keys = arr.keys();
for (var key of keys) {
    console.log(keys[key]);
}
//打印结果：  数组不是对象，拿不到键名
undefined
undefined
undefined
undefined
undefined
undefined
```

###### values()

```javascript
let arr = [23, 45, 67, 1, 4, 89]
let values = arr.values();
for (var value of values) {
    console.log(value);
}
//打印结果：
23
45
67
1
4
89
```



### 对象

#### 键值相关

##### Object.keys()  返回给定对象自身可枚举属性组成的数组

返回一个由一个给定对象的自身可枚举属性组成的数组，数组中属性名的排列顺序和正常循环遍历该对象时返回的顺序一致。

数组中属性名的排列顺序和使用for...in 循环遍历该对象时返回的顺序一致，区别是**for-in**循环还会**枚举原型链**上的属性

```js
// simple array
var arr = ['a', 'b', 'c'];
console.log(Object.keys(arr)); // console: ['0', '1', '2']

// array like object
var obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.keys(obj)); // console: ['0', '1', '2']

// array like object with random key ordering
var anObj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.keys(anObj)); // console: ['2', '7', '100']

// getFoo is a property which isn't enumerable
var myObj = Object.create({}, {
  getFoo: {
    value: function () { return this.foo; }
  }
});
myObj.foo = 1;
console.log(Object.keys(myObj)); // console: ['foo']
```



##### **Object.getOwnPropertyNames()**

返回一个由指定对象的所有自身属性的属性名（包括不可枚举属性但不包括Symbol值作为名称的属性）组成的数组



##### Object.values()

- 方法返回一个给定对象自己的所有可枚举属性值的数组，值的顺序与使用for...in循环的顺序相同 ( 区别在于 for-in 循环枚举原型链中的属性 )。
- Object.values会过滤属性名为 Symbol 值的属性

##### **Object.entries()**

返回一个给定对象自身可枚举属性的键值对数组，其排列与使用 `for...in` 循环遍历该对象时返回的顺序一致（区别在于 for-in 循环也枚举原型链中的属性）。



##### Object.assign(**target,source1,source2,...**)

- 主要用于对象的合并，将源对象source的所有**可枚举**属性合并到目标对象target上,此方法只拷贝源对象的自身属性，不拷贝继承的属性
- 是浅拷贝
- 只能进行值的复制，如果要复制的值是一个取值函数，那么将求值后再复制
- 可以用来处理数组，会把数组视为对象，

```js
Object.assign([1, 2, 3], [4, 5])   // [4,5,3] 1与2 被4与5覆盖
```



#### 属性相关

##### **hasOwnProperty()**

判断对象**自身**属性中是否具有指定的属性。

obj.hasOwnProperty('name')



##### **Object.getOwnPropertyDescriptor(obj,prop)**

返回指定对象上一个自有属性对应的属性描述符。（**自有属性指的是直接赋予该对象的属性，不需要从原型链上进行查找的属性**）.

如果指定的属性存在于对象上，则返回其属性描述符对象（property descriptor），否则返回 undefined。



#### 原型相关

##### **Object.getPrototypeOf()**

返回指定对象的原型（内部[[Prototype]]属性的值，即__proto__，而非对象的prototype）

##### **isPrototypeOf()**

判断一个对象是否存在于另一个对象的原型链上

##### **Object.setPrototypeOf(obj,prototype)**

设置对象的原型对象



#### 其他：比较

##### **Object.is()**

如果下列任何一项成立，则两个值相同：

- 两个值都是 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)
- 两个值都是 [`null`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/null)
- 两个值都是 `true` 或者都是 `false`
- 两个值是由相同个数的字符按照相同的顺序组成的字符串
- 两个值指向同一个对象
- 两个值都是数字并且
  - 都是正零 `+0`
  - 都是负零 `-0`
  - 都是 [`NaN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN)
  - 都是除零和 [`NaN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN) 外的其它同一个数字



##### **Object.freeze()**

冻结一个对象，冻结指的是不能向这个对象添加新的属性，不能修改其已有属性的值，不能删除已有属性，以及不能修改该对象已有属性的可枚举性、可配置性、可写性。也就是说，这个对象永远是不可变的。该方法返回被冻结的对象

##### **Object.isFrozen()**

判断一个对象是否被冻结



### 字符串

#### 查询 查找

##### charAt() 

返回指定下标位置的字符。如果index不在0-str.length(不包含str.length)之间，返回空字符串

##### charCodeAt()

返回指定下标位置的字符的unicode编码,这个返回值是 0 - 65535 之间的整数

##### indexOf()

返回某个指定的子字符串在字符串中第一次出现的位置

indexOf()方法对大小写敏感，如果子字符串没有找到，返回-1

第二个参数表示从哪个下标开始查找，没有写则默认从下标0开始查找。

##### lastIndexOf()

返回某个指定的子字符串在字符串中最后出现的位置



#### 截取 转换

##### toLowerCase() 

把字符串转为小写，返回新的字符串

##### toUpperCase()

把字符串转为大写，返回新的字符串

##### slice()

返回字符串中提取的子字符串

##### substring()

提取字符串中介于两个指定下标之间的字符

substring()用法与slice()一样，但不接受负值的参数

##### substr()

返回从指定下标开始指定长度的的子字符串

#### 操作

##### split()

把字符串分割成字符串数组

##### replace()

在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串

##### match()

返回所有查找的关键字内容的数组

#### es6新增

##### codePointAt方法

能够正确处理4个字节储存的字符

解决这个问题的一个办法是使用for...of循环，因为它会正确识别32位的UTF-16字符。

codePointAt方法是测试一个字符由两个字节还是由四个字节组成的最简单方法。

ES6为字符串添加了遍历器接口，使得字符串可以被for...of，循环遍历除了遍历字符串，这个遍历器最大的优点是可以识别大于0xFFFF的码点，传统的for循环无法识别这样的码点

##### at()

ES5对字符串对象提供charAt方法，返回字符串给定位置的字符。该方法不能识别码点大于0xFFFF的字符。字符串实例的at方法，可以识别Unicode编号大于0xFFFF的字符

##### repeat()

repeat方法返回一个新字符串，表示将原字符串重复n次。

##### padStart()，padEnd()

ES7推出了字符串补全长度的功能。如果某个字符串不够指定长度，会在头部或尾部补全。padStart用于头部补全，padEnd用于尾部补全



## 原型与原型链

##### 原型关系

每个class都有显式原型
每个实例都有隐式原型
实例的隐式原型指向对应class的显式原型

##### 基于原型的执行规则

实例获取某个属性或者执行方法时
先在自身属性和方法寻找
如果找不到自动去**proto**中查找



`__proto__` 属性

- 每个 JS 对象都有 `__proto__` 属性，这个属性指向原型
- 已经不推荐直接去使用这个属性，这属性是浏览器在早期为了让访问到内部属性 `[[prototype]]` 来实现的一个东西
- 原型也是一个对象，该对象包含很多函数
- 对于obj来说，可以通过`__proto__` 找到一个原型对象，在该对象中定义了很多函数提供使用



原型的constructor属性指向构造函数，构造函数通过prototype属性指回原型，但并不是所有函数都有这个属性，Function.prototype.bind()就没有这个属性



#### 原型链

- `Object` 是所有对象的爸爸，所有对象都可以通过 `__proto__` 找到它
- `Function` 是所有函数的爸爸，所有函数都可以通过 `__proto__` 找到它
- 函数的 `prototype` 是一个对象
- 对象的 `__proto__` 属性指向原型， `__proto__` 将对象和原型连接起来组成了原型链



## JS闭包

#### 闭包的定义

闭包的定义很简单。函数A内部有一个函数B，函数B可以访问到函数A中的变量，那么函数B就是闭包。

在JS中，闭包存在的意义就是让我们可以间接访问函数内部的变量。

#### 优缺点

##### 优点

- 能够读取函数内部的变量 
- 让这些变量一直存在于内存中，不会在调用结束后，被垃圾回收机制回收

##### 缺点

会使函数中的变量保存在内存中，内存消耗很大，所以不能滥用闭包，解决办法是，退出函数之前，将不使用的局部变量删除

**相关面试题** 解决var定义函数的问题

1. 循环中使用闭包解决var定义函数的问题
2. 第二个使用setTimeout里的第三个参数，这个参数会被当做timer函数的参数传入？
3. let定义来解决问题

#### 使用场景

##### 1.setTimeout

原生的setTimeout传递的第一个函数不能带参数，通过闭包可以实现传参效果。

```js
function f1(a) {
    function f2() {
        console.log(a);
    }
    return f2;
}
var fun = f1(1);
setTimeout(fun,1000);//一秒之后打印出1
```

##### 2.函数封装

##### 3.函数防抖

在事件被触发n秒后再执行回调，如果在这n秒内又被触发，则重新计时。

实现的关键就在于`setTimeOut`这个函数，由于还需要一个变量来保存计时，考虑维护**全局纯净**，可以借助闭包来实现。

```js
/*
* fn [function] 需要防抖的函数
* delay [number] 毫秒，防抖期限值
*/
function debounce(fn,delay){
    let timer = null    //借助闭包
    return function() {
        if(timer){
            clearTimeout(timer) //进入该分支语句，说明当前正在一个计时过程中，并且又触发了相同事件。所以要取消当前的计时，重新开始计时
            timer = setTimeOut(fn,delay) 
        }else{
            timer = setTimeOut(fn,delay) // 进入该分支说明当前并没有在计时，那么就开始一个计时
        }
    }
}
```

##### 4.封装私有变量

```js
function f1() {    
    var sum = 0;    
    var obj = {       
        inc:function () {           
            sum++;           
            return sum;       
        }};    
    return obj;
}
let result = f1();
console.log(result.inc());//1
console.log(result.inc());//2
console.log(result.inc());//3
```

在返回的对象中，实现了一个闭包，该闭包携带了局部变量x，并且，从外部代码根本无法访问到变量x。

隐藏数据，只提供API



###### 

## [防抖与节流](https://www.cnblogs.com/cc-freiheit/p/10827372.html)

#### 防抖

##### 场景

频繁触发事件，比如搜索框实时发送请求，

这可以使用在一些点击请求的事件上，避免因为用户的多次点击向后端发送多次请求

##### 防抖

监听一个输入框，文字变化后触发change时间，直接用keyup事件，则会频繁触发change事件
防抖：用户输入结束或暂停时，才会触发change事件加timer 

短时间内多次触发同一事件，只执行最后一次，或者只执行最开始的一次，中间的不执行

在事件被触发n秒后再执行回调，如果在这n秒内事件又被触发，则重新计时。

```js
// 非立即执行版  非立即执行版的意思是触发事件后函数不会立即执行，而是在 n 秒后执行，如果在 n 秒内又触发了事件，则会重新计算函数执行时间。
function debounce(func, wait) {
    let timer;
    return function() {
      let context = this; // 注意 this 指向
      let args = arguments; // arguments中存着e
         
      if (timer) clearTimeout(timer);
 
      timer = setTimeout(() => {
        func.apply(this, args)
      }, wait)
    }
}
```



```js
// 立即执行版 立即执行版的意思是触发事件后函数会立即执行，然后 n 秒内不触发事件才能继续执行函数的效果
function debounce(func, wait) {
    let timer;
    return function() {
      let context = this; // 这边的 this 指向谁?
      let args = arguments; // arguments中存着e
 
      if (timer) clearTimeout(timer);
 
      let callNow = !timer;
 
      timer = setTimeout(() => {
        timer = null;
      }, wait)
 
      if (callNow) func.apply(context, args);
    }
}
```



#### 节流 throttle

##### 场景

节流可以使用在scroll函数的事件，或者拖拽一个元素时，要随时拿到该元素被拖拽的位置。直接用drag事件，则会频繁触发，很容易导致卡顿

##### 节流

无论拖拽速度多快，都会每隔100ms触发一次

指连续触发事件但是在 n 秒中只执行一次函数。即 2n 秒内执行 2 次... 。节流如字面意思，会稀释函数的执行频率。

规定一个单位时间内，在这个单位时间内，只能有一次触发事件的回调函数执行，如果在一个单位时间内事件被触发多次，只有一次能生效。

同样有两个版本，时间戳和定时器版。

```js
// 时间戳版
function throttle(func, wait) {
    let previous = 0;
    return function() {
      let now = Date.now();
      let context = this;
      let args = arguments;
      if (now - previous > wait) {
        func.apply(context, args);
        previous = now;
      }
    }
}

// 定时器版
function throttle(func, wait) {
    let timeout;
    return function() {
      let context = this;
      let args = arguments;
      if (!timeout) {
        timeout = setTimeout(() => {
          timeout = null;
          func.apply(context, args)
        }, wait)
      }
    }
```

 　时间戳版和定时器版的节流函数的区别就是，时间戳版的函数触发是在时间段内开始的时候，而定时器版的函数触发是在时间段内结束的时候



## this

##### this为什么值不是在函数定义的时候确定，而是在函数执行的时候确认的

#### [this的指向](https://blog.csdn.net/weixin_37722222/article/details/81625826)

记住核心的一句话：哪个对象调用函数，函数里的this指向哪个对象

作为普通函数调用，没特殊意外，指向全局对象

作为对象方法被调用 返回当前对象

使用call apply bind调用 bind会返回新的函数 call就是执行函数，改变this的指向

在class方法中调用 代表新创建的实例

箭头函数 取它上级作用域里的值



## [apply call bind函数](https://juejin.cn/post/6844903567967387656)

#### 存在意义

改变函数执行时的上下文，改变函数运行时的this指向。

#### 来源

继承自Funvtion.prototype中，属于实例方法

#### [call()](https://www.jb51.net/article/99797.htm)

函数实例的call方法，可以**指定该函数内部this的指向**（即函数执行时所在的作用域），然后在所指定的作用域中，调用该函数。并且会**立即执行**该函数。

```js
 var keith = {
 rascal: 123
 };
 var rascal = 456;
 function a() {
 console.log(this.rascal);
 }
 a(); //456
 a.call(); //456
 a.call(null); //456
 a.call(undefined); //456
 a.call(this); //456
 a.call(keith); //123
```

call()方法可以传递**两个参数**。第一个参数是**指定函数内部中this的指向**（也就是函数执行时所在的作用域），第二个参数是函数调用时需要传递的参数。

```js
function keith(a, b) {
 console.log(a + b);
 }
keith.call(null, 1, 2); //3
```

第一个参数是必须的，可以是null，undefined，this，但是不能为空。设置为null，undefined，this表明函数此时处于全局作用域。第二个参数中必须一个个添加。

而在apply中必须以**数组**的形式添加。

**call方法的一个应用是调用对象的原生方法。也可以用于将类数组对象转换为数组。**

```js
var obj = {};
 console.log(obj.hasOwnProperty('toString')); //false
 obj.hasOwnProperty = function() {
 return true;
 }
 console.log(obj.hasOwnProperty('toString')); //true
 console.log(Object.prototype.hasOwnProperty.call(obj, 'toString')); //false
```

上面代码中，hasOwnProperty是obj对象继承的方法，如果这个方法一旦被覆盖，就不会得到正确结果。call方法可以解决这个方法，它将hasOwnProperty方法的原始定义放到obj对象上执行，这样无论obj上有没有同名方法，都不会影响结果。要注意的是，hasOwnProperty是Object.prototype原生对象的方法，而call是继承自Function.prototype的方法。

#### apply()

与call方法类似，唯一的区别就是，它接收一个**数组**作为函数执行时的参数。

```js
function keith(a, b) {
 console.log(a + b);
 }
 keith.call(null, 2, 3); //5
 keith.apply(null, [2, 3]); //5
```

### bind()

bind方法用于指定函数内部的this指向（执行时所在的作用域），然后**返回一个新函数**。bind方法**并非立即执行**一个函数。

bind除了返回是函数以外，参数与call一样。

#### 区别

- call、apply与bind的差别

> call和apply改变了函数的this上下文后便执行该函数,而bind则是返回改变了上下文后的一个函数。

- call、apply的区别

> 他们俩之间的差别在于参数的区别，call和apply的第一个参数都是要改变上下文的对象，而call从第二个参数开始以参数列表的形式展现，apply则是把除了改变上下文对象的参数放在一个数组里面作为它的第二个参数。

```js
let arr1 = [1, 2, 19, 6];
//例子：求数组中的最值
console.log(Math.max.call(null, 1,2,19,6)); // 19
console.log(Math.max.apply(null, arr1)); //  19 直接可以用arr1传递进去
```



三者的参数不限定是string类型，允许是各种类型，包括函数、object等等

## new操作符具体做了什么？new一个对象做了什么？

```js
var Func = function(){};

var func = new Func ();
```

1、new创建了一个空对象，创建了一个新对象（这个对象的类型是Object）

```js
var obj = new Object();
```

2、设置原型链，将新对象的constructor属性设置为构造函数信息，设置新对象的__proto__属性指向构造函数的prototype对象   （链接该对象/即设置该对象的构造y[e函数 到另一个对象）   将创建的新对象的内部链接[[prototype]]关联到构造函数的protptype对象。

```js
obj.__proto__ =  Func.prototype;
```



```js
function Foo() { /* .. */ }
Foo.prototype = { /* .. */ }; // 创建一个新原型对象
var a1 = new Foo();
a1.constructor === Foo; // false!
a1.constructor === Object; // true
```

a1 并没有 .constructor 属性，所以它会委托 [[Prototype]] 链上的 Foo. prototype。但是这个对象也没有 .constructor 属性（不过默认的 Foo.prototype 对象有这 个属性！），所以它会继续委托，这次会委托给委托链顶端的 Object.prototype。这个对象 有 .constructor 属性，指向内置的 Object(..) 函数

实际上，对象的 .constructor 会默认指向一个函数，这个函数可以通过对象的 .prototype 引用。“constructor”和“prototype”这两个词本身的含义可能适用也可能不适用。最好的办法是记住这一点“constructor 并不表示被构造”

a1.constructor 是一个非常不可靠并且不安全的引用。通常来说要尽量避免使用这些引用





3、让Func中的this指向obj：让构造函数的this指向新的对象并执行Func的函数体

```js
var result = Func.call(obj);
```

4、判断Func的返回值类型，将初始化完毕的新对象地址，保存到等号左边的变量中

```js
if (typeof(result) =="object"){

 func=result;

} else {

  func=obj;;

}
```



## Object.create（...）

Object.create（...）凭空创建一个”新"对象，并把新对象内部的[[prototype]]关联到指定的对象。



```js
// 和你想要的机制不一样！ 

Bar.prototype = Foo.prototype; 

// 基本上满足你的需求，但是可能会产生一些副作用 :( 

Bar.prototype = new Foo(); 
```

Bar.prototype = Foo.prototype 并不会创建一个关联到 Bar.prototype 的新对象，它只是让 Bar.prototype 直接引用 Foo.prototype 对象。因此执行类似 Bar.prototype. myLabel = ... 的赋值语句时会直接修改 Foo.prototype 对象本身。

显然这不是想要的结 果，否则根本不需要 Bar 对象，直接使用 Foo 就可以了，这样代码也会更简单一些。



Bar.prototype = new Foo() 的确会创建一个关联到 Bar.prototype 的新对象。但是它使用 了 Foo(..) 的“构造函数调用”，如果函数 Foo 有一些副作用（比如写日志、修改状态、注 册到其他对象、给 this 添加数据属性，等等）的话，就会影响到 Bar() 的“后代”，后果 不堪设想。



因此，要创建一个合适的关联对象，我们必须使用 Object.create(..) 而不是使用具有副 作用的 Foo(..)。这样做唯一的缺点就是需要创建一个新对象然后把旧对象抛弃掉，**不能直接修改**已有的默认对象。



如果能有一个标准并且可靠的方法来修改对象的 [[Prototype]] 关联就好了。在 ES6 之前， 我们只能通过设置 .__proto__ 属性来实现，但是这个方法并不是标准并且无法兼容所有浏览器。

ES6 添加了辅助函数 Object.setPrototypeOf(..)，可以用标准并且可靠的方法来修改关联

##### Object.setPrototypeOf(..)   修改对象的[[prototype关联

```JS
我们来对比一下两种把 Bar.prototype 关联到 Foo.prototype 的方法：
// ES6 之前需要抛弃默认的 Bar.prototype
Bar.ptototype = Object.create( Foo.prototype );
// ES6 开始可以直接修改现有的 Bar.prototype
Object.setPrototypeOf( Bar.prototype, Foo.prototype );
```

如果忽略掉 Object.create(..) 方法带来的轻微性能损失（抛弃的对象需要进行垃圾回收），它实际上比 ES6 及其之后的方法更短而且可读性更高。不过无论如何，这是两种完全不同的语法



#### 检查“类”关系，内省反射

检查“类”关系，寻找对象a委托的对象（如果存在) ，检查一个实例（JavaScript中的对象）的继承祖先（JavaScript中的委托关联）通常被称为内省（或者反射）

##### 1、instanceof

```js
a instanceof Foo; // true 
```

instanceof 操作符的左操作数是一个普通的对象，右操作数是一个函数。instanceof 回答 的问题是：在 a 的整条 [[Prototype]] 链中是否有指向 Foo.prototype 的对象？

这个方法只能处理对象（a）和函数（带 .prototype 引用的 Foo）之间的关系。

无法判断两个对象是否通过[[prototype]]链关联

##### 2、Foo.prototype.isPropertyOf(a);  isPropertyOf

isPrototypeOf(..) 回答的问题是：在 a 的整 条 [[Prototype]] 链中是否出现过 Foo.prototype ？

我们只需要两个对象就可以判断它们之间的关系。举例来说： 

```js
// 非常简单：b 是否出现在 c 的 [[Prototype]] 链中？这个方法并不需要使用函数（“类”），它直接使用 b 和 c 之间的对象引用来判断它们的关系。
b.isPrototypeOf( c );
```



我们也可以直接获取一个对象的 [[Prototype]] 链。在 ES5 中，标准的方法是： 

```js
Object.getPrototypeOf( a ); 
```

可以验证一下，这个对象引用是否和我们想的一样： 

```js
Object.getPrototypeOf( a ) === Foo.prototype; // true
```

绝大多数（不是所有！）浏览器也支持一种非标准的方法来访问内部 [[Prototype]] 属性： 

```js
a.__proto__ === Foo.prototype; // true 
```

这个奇怪的 .__proto__（在 ES6 之前并不是标准！）属性“神奇地”引用了内部的 [[Prototype]] 对象，如果你想直接查找（甚至可以通过 .__proto__.__ptoto__... 来遍历） 原型链的话，这个方法非常有用

和我们之前说过的 .constructor 一样，.__proto__ 实际上并不存在于你正在使用的对象中 （本例中是 a）。实际上，它和其他的常用函数（.toString()、.isPrototypeOf(..)，等等）一样，存在于内置的 Object.prototype 中。（它们是不可枚举的） 此外，.__proto__ 看起来很像一个属性，但是实际上它更像一个 getter/setter

```js
.__proto__ 的实现大致上是这样的（对象属性的定义参见第 3 章）：
Object.defineProperty( Object.prototype, "__proto__", {
get: function() {
return Object.getPrototypeOf( this );
},
set: function(o) {
// ES6 中的 setPrototypeOf(..)
Object.setPrototypeOf( this, o );
return o;
}
} );
```

#### 对象关联 原型链

现在我们知道了，[[Prototype]] 机制就是存在于对象中的一个内部链接，它会引用其他对象

通常来说，这个链接的作用是：如果在对象上没有找到需要的属性或者方法引用，引擎就 会继续在 [[Prototype]] 关联的对象上进行查找。同理，如果在后者中也没有找到需要的 引用就会继续查找它的 [[Prototype]]，以此类推。这一系列对象的链接被称为“原型链”。



#### JavaScript [[prototype]]机制的意义

##### Object.create（..）ES5新增函数 

创建一个新对象并把它关联到指定的对象，可以充分发挥[[prototype]]机制的委托，而且避免不必要的麻烦（使用new的构造函数调用会生成.prototype和.constructor引用。



###### Object.create(null)  字典，适用于存储数据

Object.create(null) 会 创 建 一 个 拥 有 空（ 或 者 说 null）[[Prototype]] 链接的对象，这个对象无法进行委托。由于这个对象没有原型链，所以 instanceof 操作符（之前解释过）无法进行判断，因此总是会返回 false。 这些特殊的空 [[Prototype]] 对象通常被称作“字典”，它们完全**不会受到原型链**的干扰，因此非常适合用来存储数据



##### Object.create()的polyfill代码

```js
if (!Object.create) {
Object.create = function(o) {
function F(){}
F.prototype = o;
return new F();
};
}
```

这段 polyfill 代码使用了一个一次性函数 F，我们通过改写它的 .prototype 属性使其指向想 要关联的对象，然后再使用 new F() 来构造一个新对象进行关联。



##### 代理

但是如果你这样写只是为了让 myObject 在无法处理属性或者方法时可以使用备用的 anotherObject，那么你的软件就会变得有点“神奇”，而且很难理解和维护。 这并不是说任何情况下都不应该选择备用这种设计模式，但是这在 JavaScript 中并不是很 常见。所以如果你使用的是这种模式，那或许应当退后一步并重新思考一下这种模式是否合适

Proxy代理，实现的就是“**方法无法找到**”时的行为，是ES6的内容。



##### 内部委托设计

当给开发者设计软件时，假设要调用 myObject.cool()，如果 myObject 中不存在 cool() 时这条语句也可以正常工作的话，那你的 API 设计就会变得很“神奇”，对于未来维护软件的开发者来说这可能不太好理解。 但是可以让你的 API 设计不那么“神奇”，同时仍然能发挥 [[Prototype]] 关联的威力：

```js
var anotherObject = {
cool: function() {
console.log( "cool!" );
}
};
var myObject = Object.create( anotherObject );
myObject.doCool = function() {
this.cool(); // 内部委托！
};
myObject.doCool(); // "cool!"

```

内部委托比起直接委托可以让API接口设计更加清晰。



如果要访问对象中并不存在的一个属性，[[Get]] 操作就会查找对象内部 [[Prototype]] 关联的对象。

这个关联关系实际上定义了一条“原型链”（有点像嵌套的作用域链），在查找属性时会对它进行遍历。 

所有普通对象都有内置的 Object.prototype，指向原型链的顶端（比如说全局作用域），如 果在原型链中找不到指定的属性就会停止。toString()、valueOf() 和其他一些通用的功能 都存在于 Object.prototype 对象上，因此语言中所有的对象都可以使用它们。 

**关联两个对象**最常用的方法是**使用 new 关键词进行函数调用**，在调用的 4 个步骤（第 2 章）中会创建一个关联其他对象的新对象。 使用 new 调用函数时会把新对象的 .prototype 属性关联到“其他对象”。

带 new 的函数调用 通常被称为“构造函数调用”，尽管它们实际上和传统面向类语言中的类构造函数不一样。 

虽然这些 JavaScript 机制和传统面向类语言中的“类初始化”和“类继承”很相似，但 是 JavaScript 中的机制有一个核心区别，那就是不会进行复制，对象之间是通过内部的 [[Prototype]] 链关联的。 

出于各种原因，以“继承”结尾的术语（包括“原型继承”）和其他面向对象的术语都无法帮助你理解 JavaScript 的真实机制（不仅仅是限制我们的思维模式）。 

相比之下，“委托”是一个更合适的术语，因为对象之间的关系**不是复制**而是**委托**。



## 行为委托

#### [[protptype]]机制

对象中的一个内部链接引用另一个对象。

如果在第一个对象上没有找到需要的属性或者方法引用，引擎就会继续在 [[Prototype]] 关联的对象上进行查找。同理，如果在后者中也没有找到需要的引用就会继续查找它的 [[Prototype]]，以此类推。这一系列对象的链接被称为“原型链”。

此机制的本质就是**对象之间**的**关联**关系。





## JS延迟（异步）加载的6种方法

#### 延迟加载js的必要性

##### 好处：

​	有助于提高页面加载速度，js延迟加载就是等页面加载完成之后再加载文件

​	HTML元素是按照其在页面中出现的次序调用的，如果js使用文档对象模型dom，而且js加载于预操作的html元素之前，代码将出错

​	js语句来获取dom对象，由于dom结构没有加载完成，获取到的是空对象

#### js的加载模式

##### 同步加载

**又称阻塞模式**，是我们平时使用最多的方式，也**就是直接将<script>写在<head>里**。这种方式会阻止浏览器的后续处理，停止后续的解析，直到当前的加载完成。一般来说，**同步加载是安全的**，但如果我们js里涉及到document内容输出、获取或修改DOM结构等行为，就会产生页面阻塞代码出错。所以一般就会建议把<script>写在**页面最底部**,以减少页面阻塞。(这种方式可能也是我们刚开始接触到js优化，最常使用的一种方式

##### 异步加载

**又称为非阻塞加载**，在浏览器下载执行js的同时，还会继续后续页面的处理

#### js延迟加载的六种方式

##### 1、defer属性

HTML 4.01为 <script>标签定义了defer属性（**延迟脚本**的执行）。

**表明脚本在执行时不会影响页面的构造，浏览器会立即下载，但延迟执行，即脚本会被延迟到整个页面都解析完毕之后再执行**。

defer属性只适用于外部脚本文件，只有 Internet Explorer 支持 defer 属性。

并且defer属性解决了async引起的脚本顺序问题，使用defer属性，脚本将按照在页面中出现的顺序加载和运行

```html
//脚本1
<script defer src="js/vendor/jquery.js"></script>
//脚本2
<script defer src="js/script2.js"></script>
//脚本3
<script defer src="js/script3.js"></script>

```

脚本将按照在页面中出现的顺序加载

可确保脚本1必定加载于脚本2和 脚本3之前，同时脚本2必定加载于脚本3之前

《高级程序设计》里提及，实际上延迟脚本并不一定会按照顺序执行，也不一定会在DOMContenterLoaded事件触发前执行，因此最好只包含一个延迟脚本

##### 2、async属性 

HTML 5为 <script>标签定义了async属性。添加此属性后，脚本和HTML将一并加载（异步），代码将顺利运行。

浏览器遇到async脚本时不会阻塞页面渲染，而是直接下载然后运行。

问题是，不同脚本运行次序就无法控制，只是脚本不会阻止剩余页面的显示。

async属性只适用于外部脚本文件。

```html
//脚本1
<script async src="js/vendor/jquery.js"></script>
//脚本2
<script async src="js/script2.js"></script>
//脚本3
<script async src="js/script3.js"></script>

```

上述代码添加async 属性，这三者的调用顺序是不确定的，脚本1可以在脚本2和脚本3之前会之后调用，这是完全不确定的。如果脚本2和脚本3需要依赖脚本1中的函数，那么不确定的调用顺序会导致错误。

异步脚本一定会在页面的load事件前执行，但可能会在DOMContenterLoaded事件触发之前或之后执行

所以，当页面的不同脚本之间彼此独立，且不依赖于本页面的其他任何脚本时，async是最理想的选择

##### defer和async的异同点

相同：

- 加载文件时不会阻塞页面渲染
- 对于内部的js不起作用
- 使用这两个属性的脚本中不能调用document.write方法

区别：

- 如果脚本无需等待页面解析，且无依赖独立运行，那么应使用 `async`。也就是每一个async属性的脚本都在它下载结束之后立即执行，同时会在window的load事件之前执行。
- 如果脚本需要等待解析，且依赖于其它脚本，调用这些脚本时应使用 `defer`，将关联的脚本按所需顺序置于 HTML 中。

##### 3、动态创建dom 

##### 4、使用jquery的getScript方法 

##### 5、使用setTimout延迟方法 

##### 6、js最后加载

## 原生Ajax请求具体步骤

Ajax（Asynchronous JavaScript and XML）

具体来说，AJAX 包括以下几个步骤

- 1.创建 XMLHttpRequest 对象，也就是创建一个异步调用对象
- 2.创建一个新的 HTTP 请求，并指定该 HTTP 请求的方法、URL 及验证信息
- 3.设置响应 HTTP 请求状态变化的函数
- 4.发送 HTTP 请求
- 5.获取异步调用返回的数据
- 6.使用 JavaScript 和 DOM 实现局部刷新

一般实现：

```js
const SERVER_URL = "/server";
let xhr = new XMLHttpRequest();
// 创建 Http 请求
xhr.open("GET", SERVER_URL, true);
// 设置状态监听函数
xhr.onreadystatechange = function() {
  if (this.readyState !== 4) return;
  // 当请求成功时
  if (this.status === 200) {
    handle(this.response);
  } else {
    console.error(this.statusText);
  }
};

// 设置请求失败时的监听函数
xhr.onerror = function() {
  console.error(this.statusText);
};

// 设置请求头信息
xhr.responseType = "json";
xhr.setRequestHeader("Accept", "application/json");

// 发送 Http 请求
xhr.send(null);

// promise 封装实现：

function getJSON(url) {
  // 创建一个 promise 对象
  let promise = new Promise(function(resolve, reject) {
    let xhr = new XMLHttpRequest();

    // 新建一个 http 请求
    xhr.open("GET", url, true);

    // 设置状态的监听函数
    xhr.onreadystatechange = function() {
      if (this.readyState !== 4) return;

      // 当请求成功或失败时，改变 promise 的状态
      if (this.status === 200) {
        resolve(this.response);
      } else {
        reject(new Error(this.statusText));
      }
    };

    // 设置错误监听函数
    xhr.onerror = function() {
      reject(new Error(this.statusText));
    };

    // 设置响应的数据类型
    xhr.responseType = "json";

    // 设置请求头信息
    xhr.setRequestHeader("Accept", "application/json");

    // 发送 http 请求
    xhr.send(null);
  });

  return promise;
}
```



## [JS继承](https://www.cnblogs.com/ranyonsue/p/11201730.html)

#### 原型链继承

##### 重点

让新实例的原型等于父类的实例

##### 特点

1、实例可继承的属性有：实例的构造函数的属性，父类构造函数属性，父类原型的属性。（新实例不会继承父类实例的属性！）

##### 缺点

1、新实例无法向父类构造函数传参。
2、继承单一。
3、所有新实例都会共享父类实例的属性。（原型上的属性是共享的，一个实例修改了原型属性，另一个实例的原型属性也会被修改！



#### 借用构造函数继承

##### 重点

用.call()和.apply()将父类构造函数引入子类函数（在子类函数中做了父类函数的自执行（复制））

##### 特点

1、只继承了父类构造函数的属性，没有继承父类原型的属性。

2、解决了原型链继承缺点1、2、3。
3、可以继承多个构造函数属性（call多个）。
4、在子实例中可向父实例传参。

##### 缺点

1、只能继承父类构造函数的属性。
2、无法实现构造函数的复用。（每次用每次都要重新调用）
3、每个新实例都有父类构造函数的副本，臃肿



#### 组合继承（组合原型链继承和借用构造函数继承）（常用）

##### 重点

结合了两种模式的优点，传参和复用

##### 特点

1、可以继承父类原型上的属性，可以传参，可复用。

2、每个新实例引入的构造函数属性是私有的。

##### 缺点

调用了两次父类构造函数（耗内存），子类的构造函数会代替原型上的那个父类构造函数。



#### 原型式继承

##### 重点

用一个函数包装一个对象，然后返回这个函数的调用，这个函数就变成了个可以随意增添属性的实例或对象。object.create()就是这个原理。

##### 特点

类似于复制一个对象，用函数来包装。

##### 缺点

1、所有实例都会继承原型上的属性。
2、无法实现复用。（新实例属性都是后面添加的）



#### 寄生式继承

##### 重点

给原型式继承外面套了个壳子。

##### 优点

没有创建自定义类型，因为只是套了个壳子返回对象（这个），这个函数顺理成章就成了创建的新对象。

##### 缺点

没用到原型，无法复用



#### 寄生组合式继承

寄生：在函数内返回对象然后调用
组合：1、函数的原型等于另一个实例。2、在函数中用apply或者call引入另一个构造函数，可传参



　　继承这些知识点与其说是对象的继承，更像是函数的功能用法，如何用函数做到复用，组合，这些和使用继承的思考是一样的。上述几个继承的方法都可以手动修复他们的缺点，但就是多了这个手动修复就变成了另一种继承模式。
　　这些继承模式的学习重点是学它们的思想，不然你会在coding书本上的例子的时候，会觉得明明可以直接继承为什么还要搞这么麻烦。就像原型式继承它用函数复制了内部对象的一个副本，这样不仅可以继承内部对象的属性，还能把函数（对象，来源内部对象的返回）随意调用，给它们添加属性，改个参数就可以改变原型对象，而这些新增的属性也不会相互影响。







## 异步与同步

##### 单线程和异步（异步由单线程的背景而来）

js是单线程语言 ，只能同时做一件事儿

浏览器和nodejs已支持JS启动进程，如Web Worker

JS和DOM渲染共用一个线程，因此JS可以修改DOM结构

遇到等待（网络请求，定时任务）不能卡住

需要异步，基于回调callback函数形式

异步基于回调来实现（event loop就是异步回调的实现原理）

##### 异步和同步

基于js是单线程

异步不会阻塞代码 执行(setTimout)

同步会阻塞代码执行（alert）

##### 异步应用场景

网络请求，如ajax 、图片加载 onload是callback的一种实现方式

定时任务，如setTimeout setInterval

##### 回调地狱

回调地狱是嵌套形式

Promise产生解决回调地狱的问题 .then是管道形式，还是callback形式，但是.then使其变成管道形式，串联的形式

不是避免回调函数，而是把回调函数变成串联的形式

new Promise(resolve,reject)

resolve和reject这两参数是函数

 

##### promise有几种状态？状态的表现和变化？then和catch对状态的影响

三种状态

pending、resolved、rejected

pending--> resolved pending-->rejected 变化不可逆

初始化promise时，传入的函数会立刻被执行（同步里的）

##### 状态的表现

pending状态，不会触发then和catch

resolved会触发后续的then回调函数

rejected会触发后续的catch回调函数

立即resolved的Promise是在本轮事件循环的末尾执行，总是晚于本轮循环的同步任务。一般来说，调用resolve或rejecte以后，promise的使命就完成了，后续操作应该放到then方法里面，而不该直接写在resolve或rejecte的后面，所以最好在它们前面加上return语句，后面的语句就不会执行。

##### then

then方法的作用是为Promise实例添加状态改变时的回调函数，then方法返回的是一个新的Promise实例（不是原来的Promise实例）。

采用链式的then，可以指定一组按照次序调用的回调函数。这时前一个回调函数，有可能返回的还是一个promise对象(即有异步操作)，这时后一个回调函数，就会等待该promise对象的状态发生变化，才会被调用。

##### async

一句话，async 函数就是 Generator 函数的语法糖。

async函数返回一个Promise对象，可以使用then方法添加回调函数。当函数执行时，遇到await就会先返回，等到触发的异步操作完成，再接着执行函数体内部的语句。



## 事件机制

### 事件

JavaScript与HTML之间的交互通过事件来实现。

事件就是文档或浏览器窗口中发生的一些特定的交互瞬间。事件是用户操作网页时发生的交互动作，比如 click/move， 事件除了用户触发的动作外，还可以是文档加载，窗口滚动和大小调整。事件被封装成一个 event 对象，包含了该事件发生时的所有相关信息（ event 的属性）以及可以对事件进行的操作（ event 的方法）。

> Node.js中使用回调函数来代替事件。

#### DOM事件流

描述的是从页面中接收事件的顺序，**事件**发生时会在元素节点之间按照特定**的**顺序传播，这个**传播过程**即 **DOM 事件流**。

”DOM2级事件“规定的事件流包括3个阶段：

1. 捕获阶段
2. 当前目标阶段
3. 冒泡阶段

我们向水里面扔一块石头，首先它会有一个下降的过程，这个过程就可以理解为从最顶层向事件发生的最具体元素（目标点）的捕获过程；之后会产生泡泡，会在最低点（ 最具体元素）之后漂浮到水面上，这个过程相当于事件冒泡。



**注意**

1. JS 代码中只能执行捕获或者冒泡其中的一个阶段。
2. onclick 和 attachEvent 只能得到冒泡阶段。【因为attachEvent 是IE的。】
3. addEventListener(type, listener[, useCapture])第三个参数如果是 true，表示在事件捕
   获阶段调用事件处理程序；如果是 false（不写默认就是false），表示在事件冒泡阶段调用事件处理
   程序。
4. 实际开发中很少使用事件捕获，更关注事件冒泡。
5. 有些事件是没有冒泡的，比如 onblur、onfocus、onmouseenter、onmouseleave



##### 事件绑定

event.preventDefault()阻止默认行为

##### 事件冒泡  

IE的事件流是事件冒泡，即事件开始时由最具体的元素（文档中嵌套层次最深的那个节点）接收，然后逐渐向上传播到较为不具体的节点（文档）

e.stopPropagation()

基于DOM树形结构
事件会顺着触发元素往上冒泡
应用场景：代理

##### 事件捕获

Netscape Communicator团队提出的另一种事件流叫做事件捕获。



#### 内存与性能

##### 事件委托（事件代理）

事件委托就是利用事件冒泡，只指定一个事件处理程序，就可以管理某一类型的所有事件

代码简洁，减少浏览器内存使用，但是不要滥用

###### 应用：

无限下拉图片列表，如何监听每个图片的点击

使用事件代理，用e.target获取触发元素，用matches来判断是否是触发元素

##### 移除事件处理程序





### 事件循环 event loop

事件循环是**user agent**(浏览器)用于协调事件，用户交互(鼠标、键盘)，脚本（JS），渲染（如HTML、CSS样式），网络等行为的一种机制

与其说是 JavaScript 提供了事件循环，不如说是嵌入 JavaScript 的 user agent（浏览器） 需要通过事件循环来与多种事件源进行交互。



### 事件执行机制（eventloop)

###### js如何执行？

从前到后，一行一行地执行

如果某一行执行报错，则执行下面代码的执行

先把同步代码执行完，再执行异步

##### 表述一

event loop就是异步回调的实现原理、机制

同步代码，一行一行放在Call Stack执行

遇到异步，会先“记录”下来Web Apis（浏览器环境），等待时机（定时、ajax网络请求回来等）到了，就移动到Callback Queue（回调函数队列）

如果Call Stack（调用栈）为空，即同步代码执行完，Event Loop开始工作

轮询查找Callback Queue（回调函数队列），如有则移动到Call Stack执行

然后继续轮询查找

同步代码执行完，就启动event loop机制 一遍一遍地循环，从异步回调里找

定时器把函数推到回调函数队列里面，event loop把队列里的放到执行栈中执行

##### 表述二

（1）所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）。

（2）主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件。

（3）一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。

（4）主线程不断重复上面的第三步。





##### DOM事件和event loop有什么联系

js是单线程的，异步（setTimeout ajax等）使用回调，基于event loop，DOM事件也使用回调，基于event loop

但DOM事件不是异步，虽然也是基于event loop实现，都是回调，就是触发时机不一样（触发时机由浏览器来控制，做定时，控制点击，监听网络返回）

##### 宏任务和微任务

macroTask 宏任务:setTimeout、setInterval、AJAX、DOM事件（不是异步，但是使用回调，event loop）

microTask 微任务:Promise async await

微任务执行时机比宏任务早

##### event loop和DOM渲染

每次Call Stack清空（即每次轮询结束），即同步任务执行完
都是DOM重新渲染的机会，DOM结构如有改变则重新渲染
然后再去触发下一次Event Loop

js是单线程的，而且和dom渲染共用一个线程
js执行的时候，得留一些时机供DOM渲染

##### 什么是DOM渲染?

把DOM结构或者js操作内容渲染到浏览器让人肉眼可见

##### 微任务和宏任务的区别

微任务：DOM渲染前触发，如Promise
宏任务：DOM渲染后触发，如setTimeout
从event loop解释，为什么微任务执行更早
微任务是由ES6语法规定的，宏任务是由浏览器规定的，两个任务存放的地方就不一样了

1.Call stack清空
2.执行当前的微任务
3.尝试DOM渲染
4.触发event loop 将Callback Queue中的移到Call stack

##### 描述event loop机制

event loop就是异步回调的实现原理、机制
同步代码，一行一行放在Call Stack执行
遇到异步，会先“记录”下来(Web Apis)，等待时机（定时、ajax网络请求回来？？等），时机到了，就移动到Callback Queue
如果Call Stack为空（即同步代码执行完） Event Loop开始工作
轮询查找Callback Queue，如有则移动到Call Stack执行
然后继续轮询查找 