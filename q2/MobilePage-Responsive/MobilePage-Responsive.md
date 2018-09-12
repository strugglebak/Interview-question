# 移动端页面(响应式)

## media query(媒体查询)
媒体查询的逻辑很简单,像如下代码
```
<!DOCTYPE html>
<html lang="zh-Hans">
  <head>
    <meta charset="UTF-8" />
    <title>移动页面 Demo</title>
    <style>
      @media (max-width: 800px) {
        body {
          background: red;
        }
      }
    </style>
  </head>
  <body>
    body
  </body>
</html>
```

其中 `@media` 后面接一个查询条件, 若此查询条件成立，则执行大括号里面的 css 代码
条件还有个 and ，使用方式为如下
```
@media (条件1) and (条件2) {
    // 条件均满足时成立的 css 代码
}
```

## 实例
一个较早做响应式页面的网站[smashingmagazine](https://www.smashingmagazine.com/)
现就模拟其 menu 栏做一个响应式的 Demo
(待续)

## meta viewport

不加 meta viewport 之前，用如下 js 取到的页面宽度为 980 px
```
document.documentElement.clientWidth
```

这里是用 320px 来模拟 980px 的网站,因为一些历史原因，当时的手机只有 300 多像素宽，而 PC 则大概是 900 多像素宽,
所以对页面必须要进行缩放才能正常显示

比如说用 iPhone6 来访问那个页面,需要加入如下 meta
```
<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1, maximum-scale=1.0, minimum-scale=1.0">
```
以上的意思就是"宽度为设备宽度，不允许用户缩放"
用 `document.documentElement.clientWidth` 取到的宽度就是 320px

## 移动端的特点
- 手机端没有 hover
- 手机端有 touch 事件
- 没有 resize
- 没有滚动条

