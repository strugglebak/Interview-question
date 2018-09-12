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

