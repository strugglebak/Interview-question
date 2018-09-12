# 手机专用自适应方案

> 非响应式方案

## 12 px 法则
网页的默认的 font-size 是 16px
但是 Chrome 浏览器有个内容字体设置，默认是 12px,
若这个设置不变，那么在页面中设置的字体比这个小的画，
Chrome 渲染出来的字体是不会变的

## REM
px  像素
em  一个 M 字母的宽度(可以理解为一个汉字的宽度)
rem (root em) 根元素(**html**)的 font-size
vh  (viewport height) 100vh == 视口高度
vw  (viewport width)  100vw == 视口宽度

## REM 与 EM 的区别
1em 等于它父元素的 font-size,与根元素关系不大
1rem 等于网页根元素的 font-size

## 动态 REM

本质上来说，就是将 rem 应用在 width 和 height 上,然后根据根元素的 font-size 值来改变自身的值

有几种布局方式
1. 响应式布局
2. 百分比布局
3. 整体缩放

百分比布局高度要变化的时候，就很难调试
所以这里的动态 REM 做的操作就是: 用 JS 使得 html 的 font-size 等于页面宽度

使用 `document.write` 方法可以做到动态设置，如下代码所示
```
var pageWidth = window.innerWidth;
document.write('<style>html{font-size:'+ pageWidth +'px}</style>');
```

Demo(待续)

## px 自动变成 rem
使用 sass
利用 sass 的function 功能，在 scss 文件中写入
```
@function px2rem($px) {
    @return $px/$designWidth*10 + rem;
}
$designWidth: 640;
```

操作步骤(待续)

