# Interview-question

## 问题1
请写出一个符合 W3C 规范的 HTML 文件，要求

- 页面标题为「我的页面」
- 页面中引入了一个外部 CSS 文件，文件路径为 /style.css
- 页面中引入了另一个外部 CSS 文件，路径为 /print.css，该文件仅在打印时生效
- 页面中引入了另一个外部 CSS 文件，路径为 /mobile.css，该文件仅在设备宽度小于 500 像素时生效
- 页面中引入了一个外部 JS 文件，路径为 /main.js
- 页面中引入了一个外部 JS 文件，路径为 /gbk.js，文件编码为 GBK
- 页面中有一个 SVG 标签，SVG 里面有一个直径为 100 像素的圆圈，颜色随意
- 注意题目中的路径

## 答案
```
<!DOCTYPE html>
<html>
  <head>
    <title>我的页面</title>
    <link rel="stylesheet" href="./style.css">
    <link rel="stylesheet" href="./print.css" media="print">
    <link rel="stylesheet" href="./mobile.css" media="(max-width: 500px)">
    <style type="text/css">
      svg {
        width: 100px;height: 100px;
      }
    </style>
  </head>
  <body>
    <svg>
        <circle cx="50px" cy="50px" r="50px"/>
    </svg>
    <script type="text/javascript" src="./main.js"></script>
    <script type="text/javascript" src="./gbk.js" charset="gbk"></script>
  </body>
</html>
```

## 问题2
[2016腾讯前端面试题](https://github.com/Bless-L/MyBlog/blob/master/post/2016%E8%85%BE%E8%AE%AF%E5%AE%9E%E4%B9%A0%E7%94%9F%E5%89%8D%E7%AB%AF%E9%9D%A2%E8%AF%95%E7%BB%8F%E5%8E%86%E5%8F%8A%E6%80%BB%E7%BB%93%EF%BC%88%E4%BA%8C%EF%BC%89.md)
移动端是怎么做适配的?
提示:

- meta viewport
- 媒体查询(media query)
- 动态 rem 方案

## 答案
主要是看页面的复杂度
1. 简单页面的做法是，设置高度为固定值，然后宽度为 100%
2. 复杂一点的可以采用百分比布局,即利用百分比设置元素的大小进行适配
3. 再复杂一点的就要用到媒体查询，即 media query, 依据页面的大小来写不同的 css 进行渲染

而目前能做到兼容以上方案的就是动态 rem 方案,该方案需要用到以下几点

- meta viewport

    ```
    <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    ```
    以上代码作用是防止手机页面模拟 980 像素宽度
- 媒体查询

    即 media query, 使用方式类似于
    ```
    @media (条件) {
        // 条件均满足时成立的 css 代码
    }
    或者
    @media (条件1) and (条件2) {
        // 条件均满足时成立的 css 代码
    }
    ```
    示例:
    ```
    <style>
      @media (max-width: 800px) {
        /*若括号中条件成立的 css 代码*/
        body {
          background: red;
        }
      }
    </style>
    ```
- rem

    rem 即 **font size of the root element**，网页根元素的 font-size,
    本质上来说，就是将 rem 应用在 width 和 height 上,然后根据根元素的 font-size 值来改变元素宽高的大小,
    可以利用 sass 的function 功能，在 scss 文件中写入
    ```
    @function px2rem($px) {
        @return $px/$designWidth*10 + rem;
    }
    $designWidth: 640;
    ```
    其中 $px 为传入的需要转换的像素值，$designWidth 为设计稿提供的宽度值

## 问题3
[2017年腾讯前端实习面试题（二面）](https://earthsplitter.github.io/2017/03/31/2017腾讯实习经验总结/)
用过 CSS3 么，实现圆角矩形和阴影怎么做?

## 答案
1. 圆角矩形
使用 `border-radius` 属性

`border-radius` 是一个简写属性,包含 `border-top-left-radius、border-top-right-radius、border-bottom-right-radius，和 border-bottom-left-radius` 

即包含 "左上、右上、右下、左下" 角

示例
```
border-radius: 10px; /*半径为 10px 的圆角*/

/*或者*/
border-top-left-radius: 10px;
border-top-right-radius: 10px;
border-bottom-right-radius: 10px;
border-bottom-left-radius: 10px;
```

2. 阴影
使用 `box-shadow` 属性

`box-shadow` 的语法是 `box-shadow: <inset> <offset-x> <offset-y> <blur-radius> <spread-radius> <color>`

```
inset: 使用这个，则使阴影在边框内; 若不使用，阴影在边框外
offset-x, offset-y: 阴影偏移量
blur-radius: 设置模糊效果，值越大越模糊
spread-radius: 设置阴影扩大和缩小的效果
color: 设置阴影的颜色
```

若设置多重阴影效果，可以用逗号 ',' 隔开

## 问题4
什么是闭包，闭包的用途是什么？

## 答案
1. 什么是闭包

有如下代码
```
function makeFunc1() {
    let name = "Mozilla";
    function displayName() {
        alert(name);
    }
    return displayName;
}

var myFunc = makeFunc1();
myFunc();
```
在 `funtion dispalyName` 中, 函数 `dispalyName` 访问到了它作用域之外的变量 `name` ，这个就叫做 **闭包**, 即

**闭包是由函数以及创建该函数的词法环境组合而成，而这个环境包含了这个闭包创建时所能访问的所有的局部变量**

2. 闭包的用途是什么

有如下代码
```
var myFunc = (function makeFunc2() {
    let num = 0;
    function changeBy(val) {
        num += val;
    }

    return {
        add: function() {
            changeBy(1);
        },
        sub: function() {
            changeBy(-1);
        },
        val: function() {
            return num;
        }
    }
}).call();

console.log(myFunc.val()); // 0
myFunc.add();
console.log(myFunc.val()); // 1
myFunc.sub();
console.log(myFunc.val()); // 0
```
通过以上代码可以看出，闭包的作用就是 **隐藏变量以及封装**, 目的就是不直接对全局变量进行修改，
用局部变量以及能访问这个局部变量的函数来替代，最后将这个函数接口暴露出来使用，即可达到目的

## 问题5
call、apply、bind 的用法是什么?

## 答案

- call 用法
    函数实例调用 `call` 方法，指定函数 `this` 内部的指向, 调用方式为 `func.call(thisValue)`
    ```
    var obj = {};
    var f = function() {
        return this;
    };
    f() === window; // true
    f.call(obj) === obj; // true
    ```
    若有多个参数要传,调用方式为 `func.call(thisValue, arg1, arg2, ...)`
    ```
    var f = function(a, b) {
        return a + b;
    }
    f.call(null, 1, 2); // 3
    ```
    其中 `thisValue` 为 `this` 所要指向的对象
- apply 用法
    与 `call` 方法类似，指定函数 `this` 内部指向，然后再调用该函数, 调用方式为 `func.call(thisValue, [arg1, arg2, ...])`
    ```
    var f = function(a, b) {
        console.log(a + b);
    }
    f.apply(null, [1, 2]); // 这里第二个参数必须以数组形式添加
    ```
- bind 用法
    bind 方法用于将函数体内的 `this` 绑定到某个对象,然后 **返回一个新函数**, 调用方式为 `func.bind(thisValue)`
    ```
    var obj = { x: 3 };
    var f = function() {
        console.log(this.x);
    };
    f(); // undefined
    var f2 = f.bind(obj); // 绑定 this 到 obj 这个对象中
    f2(); // 3
    ```
另外,以上三种方式中，`thisValue` 如果设置为 `null` 或 `undefind`, 那么等同于指定的是全局对象,即 `window`

## 问题6
请说出至少 8 个 HTTP 状态码，并描述各状态码的意义

例如:

状态码 200 表示响应成功

## 答案
状态码类型如下

- 1XX消息 -- 表示请求已经被接收，需要继续处理
    - 100 Continue: 表示服务器已经接收到请求头，并且客户端应继续发送请求主体
    - 101 Switching Protocols: 表示服务器已经理解了客户端的请求，并将通过 `Upgrade` 消息头通知客户端采用不同的协议来完成请求
- 2XX成功 -- 表示请求已经被服务器接收、理解
    - 200 OK: 表示响应成功
    - 202 Accepted: 表示服务器已经接收请求，但尚未处理
- 3XX重定向 -- 表示需要客户端的后续操作才能完成请求
    - 301 Moved Permanently: 表示请求的资源已经被永久地移动到新的位置，并且将来对该资源的任何引用都应该使用本响应返回的若干个 URL 之一
    - 302 Found: 要求客户端执行临时重定向,由于这个重定向是临时的，客户端应继续向原有地址发送以后的请求
    - 304 Not Modified: 表示资源未被修改,由于客户端有之前下载的副本，所有不需要传输资源
- 4XX客户端错误 -- 请求含有词法错误或者无法被执行，通常表示客户端发送请求时出问题的情况
    - 400 Bad Request: 由于明显的客户端错误，服务器不会处理该请求
    - 401 Unauthorized: 表示当前请求需要用户验证,类似于 403 Forbidden
    - 403 Forbidden: 表示服务器已经理解请求，但是拒绝执行它
    - 404 Not Found: 表示请求失败，请求所希望得到的资源未在服务器上发现，但允许用户的后续请求
- 5XX服务器错误 -- 表示服务器无法完成明显有效的请求
    - 500 Internal Server Error: 表示服务器遇到了一个未曾预料的情况，导致无法响应请求
    - 501 Not Implemented: 表示服务器不支持当前请求所需要的某个功能
    - 502 Bad Gateway: 作为网关或代理服务器尝试执行请求时，从上游服务器接收到无效的响应
    - 503 Service Unavilable: 表示服务器当前因正在维护或过载，无法响应请求
    - 504 Gateway Timeout: 作为网关或代理服务器尝试执行请求时,未能即时从上游服务器接收到响应

## 问题7
请写出一个 HTTP post 请求的内容，包括四部分。

其中

第四部分的内容是 username=ff&password=123

第二部分必须含有 Content-Type 字段

请求的路径为 /path

## 答案
```
POST /path HTTP/1.1
Host: www.baidu.com
User-Agent: curl/7.58.0
Accept: */*
Content-Length: 24
Content-Type: application/x-www-form-urlencoded

username=ff&password=123
```

## 问题8
请说出至少三种排序的思路，这三种排序的时间复杂度分别为

1. O(n*n)
2. O(n log2 n)
3. O(n + max)

## 答案
1. O(n*n)

冒泡排序:
```
对于一组数列来说，重复走过需要排序的部分，一次比较两个数列中的元素，依照大小规则交换它们的顺序，直到整个数列中不需要做交换的操作为止
```
2. O(n log2 n)

归并排序
```
先将数列中的这堆数分成一个个长度为 2 的子数列，然后使每个子数列有序，然后两两合并, 继续两两合并剩下的已经弄好序的子数列，直到这些子数列合并成一个有序数列为止
```
3. O(n + max)

计数排序
```
先扫描一遍数列中，得到该数列的最大值 maxValue 和最小值 minValue,然后增加一个长度为 [maxValue - minValue + 1] 的数组, 这个数组的下标 index 对应着数列的值，

然后统计数列的值出现的次数，记录在数组下标对应的值上,然后依据这个次数反向填充进原数列中
```

## 问题9
一个页面从输入 URL 到页面加载显示完成，这个过程中都发生了什么?

## 答案
1. URL 解析
    ```
    从输入 URL 开始，浏览器就开始开启线程解析这个 URL(统一资源定位符)，URL 的格式为: protocol://host:port/path?query#fragment
    其中
    protocol: 代表 URL 协议头，比如说 http 或 ftp 等
    host: 主机名或者 ip 地址
    port: 端口号
    path: 请求资源的目录路径
    query: 查询参数
    fragment: 就是锚点,与网页的定位显示有关
    对于端口号 port 来说，可有可无，一般使用协议的默认端口,如 http 的 80 端口，https 的 443 端口等
    ```

2. 检查 DNS 缓存
    ```
    在 DNS 进行域名解析之前，会先检查 DNS 缓存是否与输入的 URL 有映射关系，若有就调用这个映射，没有查到则返回
    检查顺序依次为: 检查 HOST 文件、检查本地 DNS 缓存、检查浏览器缓存
    HOST 文件: 其实就是 /etc/host
    本地 DNS 缓存: 以 Linux 系统来说，为 /etc/resolv.conf
    浏览器缓存: 即浏览器中保存的 DNS 缓存，比如在 Chrome 浏览器中输入 chrome://dns/ 即可看到
    ```

3. DNS 域名解析
    ```
    DNS 域名解析的本质就是将 DNS 域名解析成 IP 的过程，以解析 www.wh.hb.cn 为例
    在经过 DNS 缓存的检查之后，分别依次向这几个服务器进行迭代查询
    ```
    - 根域名服务器(顶级域)
    - 管理 cn 域的服务器(第二层域)
    - 管理 hb.cn 域的服务器(子域)
    - 管理 wh.hb.cn 域的服务器
    ```
    首先若本地没有缓存中没有对应域名对应 ip，则本地域名服务器会向根域名服务器发送查询请求，若根域名服务器不存在该域名，则本地域名服务器会向管理 cn 域的服务器发送一个查询请求，
    以此类推,最终的解析过程为 `. -> .cn -> hb.cn -> wh.hb.cn -> www.wh.hb.cn`
    ```

4. 建立 TCP/IP 连接
    ```
    IP 地址获取到之后，就开始与目标服务器进行连接，这一过程靠 TCP 协议来完成,需要经过 3 次握手来完成连接
    第 1 次握手: 客户端发送连接请求报文，在这个报文中，设置 SYN=1, seq=x(起始序列号)
    第 2 次握手: 服务器接收到这个连接请求报文后，返回确认报文，设置 SYN=1,ACK=1,seq=y(随机设置),ack=x+1(表示x已经收到，期待收到x+1)
    第 3 次握手: 客户端收到服务器返回的确认报文后，也要返回一个确认报文，设置 ACK=1,seq=x+1,ack=y+1
    ```

    其中

    - SYN=1,ACK=0 表示连接请求报文
    - SYN=1,ACK=1 表示连接接收报文
    - seq 为序号字段，表示报文数据的第一个字节的序号
    - SYN=1 时,报文不能携带数据,并且需要消耗序号,如果不设置 SYN，则不携带数据也不消耗序号
    - ack 为确认号字段，表示期望收到对方下一个报文数据的第一个字节的序号

    3 次握手完成之后，就可以传输数据了

5. 浏览器发起 HTTP 请求
    ```
    发送 HTTP 请求的过程就是构建 HTTP 请求报文并通过 TCP 协议发送到服务器指定端口, 一个完整的 HTTP 请求包括三部分: 请求起始行、请求报头、请求正文
    格式如下:

    1 请求方法 路径 协议/版本
    2 Key1: value1
    2 Key2: value2
    2 Key3: value3
    2 Content-Type: application/x-www-form-urlencoded
    2 Host: www.baidu.com
    2 User-Agent: curl/7.54.0
    3
    4 要上传的数据

    第 1 部分为请求起始行，第 2 部分为请求报头，第 3 部分为回车，第 4 部分为请求正文
    常用的请求方法有: GET POST PUT DELETE OPTIONS HEAD
    常见的请求报头有: Accept, Accept-Charset, Content-Type, Authorization, Cookie 等
    在使用 POST PUT 等方法时，通常就需要客户端向服务器传输数据,请求正文中内容的就是数据了
    ```

6. 服务器接收 HTTP 请求，并返回 HTTP 响应
    ```
    这个过程是，服务器会对 TCP 连接进行处理，会对 HTTP 协议进行解析，并按照报文格式进一步封装成 HTTPRequest 对象
    而一个完整的 HTTP 响应也包括三部分: 响应起始行、响应报头、响应正文
    格式如下:

    1 协议/版本号 状态码 状态解释
    2 Key1: value1
    2 Key2: value2
    2 Content-Length: 17931
    2 Content-Type: text/html
    3
    4 要下载的内容

    第 1 部分为响应起始行，第 2 部分为响应报头，第 3 部分为回车，第 4 部分为响应正文
    常见的响应报头有: Server, Connection 等
    而响应正文就是服务器要返回给浏览器的文本信息，通常返回的就是 HTML CSS JS 图片等资源文件
    ```
7. 渲染页面
    ```
    需要引入两个概念: Reflow(回流) Repaint(重绘)
    Reflow: 若元素的内容、尺寸、位置等发生了变化，浏览器需要重新计算样式和渲染树，这个过程叫做回流
    Repaint: 当盒模型的大小、位置以及其他属性(颜色、字体等)确定下来之后,浏览器开始绘制内容，这个过程叫做重绘

    渲染页面在浏览器这边是个边解析边绘制的过程,浏览器会解析 HTML 文件来构建 DOM 树，然后解析 CSS 文件构建渲染树，这个过程弄好后，浏览器会布局渲染树然后绘制到屏幕上;
    而对于 JS 来说，JS 的相关解析是由浏览器中的 JS 解析引擎完成的。整个过程中，页面加载都会进行回流和重绘，这两个操作都会使得页面变得卡顿。
    ```

8. 关闭 TCP/IP 连接
    ```
    关闭 TCP/IP 连接是通过四次挥手来实现的
    第 1 次挥手: 客户端发送关闭连接请求，发送 FIN=1,seq=u
    第 2 次挥手: 服务器接收到关闭连接请求，返回 ACK=1,seq=v,ack=u+1(服务器关闭读通道，可以发送数据)
    第 3 次挥手: 服务器接着发送关闭连接的回应，发送 FIN=1,ACK=1,seq=w,ack=u+1
    第 4 次挥手: 客户端接收到回应后，发送 ACK=1,seq=u+1,ack=w+1(服务器接收到后会关闭写通道)
    ```

## 问题10
如何实现数组去重？

假设有数组 array = [1,5,2,3,4,2,3,1,3,4]

你要写一个函数 unique，使得

unique(array) 的值为 [1,5,2,3,4]

也就是把重复的值都去掉，只保留不重复的值。

要求：

1. 不要做多重循环，只能遍历一次
2. 请给出两种方案，一种能在 ES 5 环境中运行，一种能在 ES 6 环境中运行（提示 ES 6 环境多了一个 Set 对象）

## 答案
- ES 5 方案
```
function unique(array) {
  let obj = {};
  let ret = [];
  for (let i=0; i < array.length; i++) {
    let key = array[i];
    if (!obj[key]) {
      obj[key] = 1;
      let value = key;
      ret.push(value);
    }
  }
  return ret;
}
let array = [1,5,2, "xxx", 3,4,2,3,1, "xxx" , "", NaN, undefined, NaN, "", null ,3,4, undefined, null];
console.log(unique(array));
```

- ES 6 方案
```
let array = [1,5,2, "xxx", 3,4,2,3,1, "xxx" , "", NaN, undefined, NaN, "", null ,3,4, undefined, null];

const items = new Set(array);
const newArray = Array.from(items);
console.log(newArray);
```

## JS 高级基础知识
### 问题 1
```
var object = {}
object.__proto__ ===  ????填空1????  // 为 true

var fn = function(){}
fn.__proto__ === ????填空2????  // 为 true
fn.__proto__.__proto__ === ????填空3???? // 为 true

var array = []
array.__proto__ === ????填空4???? // 为 true
array.__proto__.__proto__ === ????填空5???? // 为 true

Function.__proto__ === ????填空6???? // 为 true
Array.__proto__ === ????填空7???? // 为 true
Object.__proto__ === ????填空8???? // 为 true

true.__proto__ === ????填空9???? // 为 true

Function.prototype.__proto__ === ????填空10???? // 为 true
```

### 答案
```
var object = {}
object.__proto__ ===  Object.prototype

var fn = function(){}
fn.__proto__ === Function.prototype
fn.__proto__.__proto__ === Object.prototype

var array = []
array.__proto__ === Array.prototype
array.__proto__.__proto__ === Object.prototype

Function.__proto__ === Function.prototype
Array.__proto__ === Function.prototype
Object.__proto__ === Function.prototype

true.__proto__ === Boolean.prototype

Function.prototype.__proto__ === Object.prototype
```

### 问题 2
```
function fn(){
    console.log(this)
}
new fn()
```

new fn() 会执行 fn，并打印出 this，请问这个 this 有哪些属性？这个 this 的原型有哪些属性？

### 答案

1. 这个 this 没有属性，但有 `__proto__` 这个隐藏属性,并指向 `fn.prototype`
2. 这个 this 的原型有 `constructor` 以及 `__proto__` 属性, `constructor` 属性为 `fn`, 而 `__proto__` 属性指向 `Object.prototype`

### 问题 3
JSON 和 JavaScript 是什么关系？

JSON 和 JavaScript 的区别有哪些？

### 答案
1. JSON 是一种"与语言无关的数据格式"，衍生(抄袭)自 JavaScript, 是 JavaScript 的子集
2. JavaScript 与 JSON 的区别有如下

JSON 不支持函数、undefined、变量、引用、单引号字符串、对象的key不支持单引号也不支持不加引号、没有内置的 Date、Math、RegExp 等。
而 JavaScript 全都支持。

### 问题 4
前端 MVC 是什么？

请用代码大概说明 MVC 三个对象分别有哪些重要属性和方法。

### 答案
1. MVC 属于软件的一种设计模式，即 Model View Controller(模型 视图 控制器),将一个应用层软件分为三层。对于前端来说
 - Model 层就是用于封装与业务逻辑相关的数据以及对数据的处理方法，它用来跟服务器打交道
 - View 层就是数据的可视化了，在前端反映的就是页面显示
 - Controller 层就是执行 Model 层以及 View 层间的组织作用，用来控制一个应用总体的逻辑和流程
 总的来说，Model 通过与服务器的请求与响应来获得数据，Controller 通过监听 View 页面的变化，调用 Model 的返回的数据对 View 改变的部分进行可视化的操作，就是其总体流程了

2. 代码如下所示
```
var model = {
    data: null,
    init(){}, // model 初始化
    fetch(){}, // model 的数据获取
    save(){} // model 的数据保存
}
var view = {
    init() {}, // view 初始化
    template: 'html 内容' // view 网页上的内容
}
var controller = {
    view: null,
    model: null,
    init(view, model){
        // 获取到 view 以及 model 的实例
        this.view = view;
        this.model = model;
        // 分别调用 view 以及 model 的初始化函数对其进行初始化操作
        this.view.init.call(this.view);
        this.model.init.call(this.model);
        // 最后绑定事件
        this.bindEvents();
    }
    render(){
        // 用来渲染 view
    },
    bindEvents(){
        // 用来绑定事件
    }
}
```

### 问题 5
在 ES5 中如何用函数模拟一个类？

### 答案
使用 `new` 关键字
1. 定义这个类的构造函数，并将这个类的一些私有属性放到这个构造函数中
2. 将这个类相关的公有属性放到这个构造函数的原型对象中，并将这个构造函数的 `prototype` 指向这个原型对象
3. 使用 `new` 关键字实例化这个构造函数，new 出来的这个对象就可以当作类来使用了
如以下代码
```
function soldier(id) {
    this.id = id;
}
soldier.prototype.walk = function() {
    // walk code
}
soldier.prototype.run = function() {
    // run.code
}

var xxx = new soldier(1);
xxx.walk();
xxx.run();
```

### 问题 6
用过 Promise 吗？举例说明。
如果要你创建一个返回 Promise 对象的函数，你会怎么写？举例说明。
### 答案
1. 用过，在使用 jQuery 或 axios ajax 功能时，这个返回的就是 Promise 对象, 如下代码所示
```
$.ajax({
    url: 'www.baidu.com',
    'method': 'GET'
}).then(success1, error1).then(success2, error2);
```
以上的 success error 相关函数是自己定义的，成功就调用 success, 失败就调用 error
2. 自己实现 Promise,代码如下
```
function myPromise() {
    return new Promise ( function(resolve, reject) {
        var condition = false;
        setTimeout(function() {
            if (condition) {
                resolve.call(undefined, "success");
            } else {
                reject.call(undefined, "error");
            }
        }, 1000);
    });
}
// 使用 myPromise
var xxx = myPromise();
xxx.then(
         (successText)=> {
           console.log(successText);
         }, 
         (errorText)=> {
           console.log(errorText);
         }
);
```

