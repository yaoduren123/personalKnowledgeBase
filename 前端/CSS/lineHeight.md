---
title: lineHeight
date: 2022-03-21 11:22
tags:  css
style_green: "color: red"
---
CSS line-height 属性介绍。行高是指文本行基线间的垂直距离
## 图解 `@red: line-height`
下面是一段 HTML 代码
```HTML
<!DOCTYPE html>
<html>
    <head>
        <title>Test</title>
        <style type="text/css" >
            span
            {
                padding:0px;
                line-height:1.5;
            }
        </style>
    </head>
    <body>
        <div class="test">
            <div style="background-color:#ccc;">
                <span style="font-size:3em;background-color:#999;">中文English</span>
                <span style="font-size:3em;background-color:#999;">English中文</span>
            </div>
        </div>
    </body>
<html>
```
渲染如图:
![](./_image/2022-03-21/2022-03-21-17-16-39@2x.jpg)
### 顶线、中线、基线、底线
从上到下四条线分别是顶线、中线、基线、底线，很像才学英语字母时的四线三格，我们知道vertical-align属性中有top、middle、baseline、bottom，就是和这四条线相关。
###行高、行距与半行距 #lineHeight
`@red: 行高`是指上下文本行的基线间的垂直距离，即图中两条红线间垂直距离。
`@red:行距`是指一行底线到下一行顶线的垂直距离，即第一行粉线和第二行绿线间的垂直距离。
`@red:半行距`是行距的一半，即区域3垂直距离/2，区域1，2，3，4的距离之和为行高，而区域1，2，4距离之和为字体size，所以半行距也可以这么算：（行高-字体size）/2
![](./_image/2022-03-21/2022-03-21-17-28-45@2x.jpg)
### 内容区、行内框、行框
![](./_image/2022-03-21/2022-03-21-17-33-44@2x.jpg)
#### 内容区
 >底线和顶线包裹的区域，即下图深灰色背景区域。
#### 行内框
> 每个行内元素会生成一个行内框，行内框是一个浏览器渲染模型中的一个概念，无法显示出来，在没有其他因素影响的时候（padding等），行内框等于内容区域，而设定行高时行内框高度不变，半行距【（行高-字体size）/2】分别增加/减少到内容区域的上下两边（深蓝色区域）
#### 行框（line box）
> 行框是指本行的一个虚拟的矩形框，是浏览器渲染模式中的一个概念，并没有实际显示。行框高度等于本行内所有元素中行内框最大的值（以行高值最大的行内框为基准，其他行内框采用自己的对齐方式向基准对齐，最终计算行框的高度），当有多行内容时，每行都会有自己的行框。
![](./_image/2022-03-21/2022-03-21-17-52-12@2x.jpg)
## 定义
line-height 属性设置行间的距离（行高），不能使用负值。该属性会影响行框的布局。在应用到一个块级元素时，它定义了该元素中基线之间的最小距离而不是最大距离。line-height 与 font-size 的计算值之差(行距)分为两半，分别加到一个文本行内容的顶部和底部。可以包含这些内容的最小框就是行框

## 取值
- inherit ** do not scale with the relevant font size **
- percentage value — fontSize \* percentage,  ** do not scale with the relevant font size **
- length value (px, em) ** do not scale with the relevant font size **
- normal - 1.2  **scale with the relevant font size **
- number value(unit-less value) 1.0-1.2 inital value, ** relative to their font size**. 

## 备注
-   **number value(unit-less value) best method for setting line-height** as line-heights will then always scale with the relevant font size
- it is safe to assume that **headings can have less relative line-height than paragraphs of text**. For example: heading 1.2 normal content 1.5
- WCAG 2.0: **line spacing is at least space and a half  within paragraphs**. This means that to be  AAA compliant, **paragraphs line-height should be set to 1.5**
- sup sub line-height 0





