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
    rem 即 **font size of the root element**，网页根元素的 font-size
    本质上来说，就是将 rem 应用在 width 和 height 上,然后根据根元素的 font-size 值来改变元素宽高的大小
    利用 sass 的function 功能，在 scss 文件中写入
    ```
    @function px2rem($px) {
        @return $px/$designWidth*10 + rem;
    }
    $designWidth: 640;
    ```
    其中 $px 为传入的需要转换的像素值，$designWidth 为设计稿提供的宽度值

