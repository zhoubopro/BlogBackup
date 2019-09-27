---
title: CSS垂直水平居中
top: true
cover: false
date: 2019-09-18 17:23:02
password:
summary:
tags:
- css
categories:
- css
---
#### 概述

>垂直水平居中，在 CSS 中是一个老生常谈的问题，有常见的 ```flex、transform、absolute``` 等等。
>也有 CSS3 的网格布局。还有伪元素的方法，是的，你没有看错，```::after``` 和 ```::before``` 也可以实现居中。

#### No.1 flex方法 [传送门](https://codepen.io/zhoubopro/pen/pozxQYo)
>现在通用的方法也就是这个啦，因为它的写法够简单直观，兼容性也没什么问题，也是移动端居中方式的首选。
>利用到了 2 个关键属性：```justify-content``` 和 ```align-items```，都设置为 ```center```，即可实现居中。
>需要注意的是，一定要把这里的 ```flex-center``` 挂在父级元素，才能使得其中 唯一的 子元素居中。

```html
<div class="wrapper flex-center">
  <p>flex horizontal and vertical</p>
</div>
```

```css
.wrapper {
  width: 300px;
  height: 300px;
  border: 1px solid #ccc;
}

.flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

#### No.2 flex + margin [传送门](https://codepen.io/zhoubopro/pen/QWLZJYX)
>父级元素设置 ```flex```，子元素设置 ```margin: auto```。

```html
<div class="wrapper">
  <p>flex + margin</p>
</div>
```

```css
.wrapper {
  display: flex;
  width: 300px;
  height: 300px;
  border: 1px solid #ccc;
}

.wrapper p {
  margin: auto;
}
```

#### No.3 transform + absolute [传送门](https://codepen.io/zhoubopro/pen/XWrxoJN)
>这个方法，用于图片的居中显示。

```html
<div class="wrapper">
  <img src="http://img2.imgtn.bdimg.com/it/u=1405109477,1383249963&fm=11&gp=0.jpg">
</div>
```

```css
.wrapper {
  width: 300px;
  height: 300px;
  border: 1px solid #ccc;
  position: relative;
}

.wrapper img {
  position: absolute;
  /*position: relative; 替换掉绝对定位，效果是一样的 */
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
```

#### No.4 table-cell [传送门](https://codepen.io/zhoubopro/pen/WNeaLwe)
>利用 ```table``` 的单元格居中效果展示，与 ```flex``` 一样，需要写在父级元素上。

```html
<div class="wrapper">
  <p>table-cell</p>
</div>
```

```css
.wrapper {
  width: 300px;
  height: 300px;
  border: 1px solid #ccc;
  display: table-cell;
  text-align: center;
  vertical-align: middle;
}
```

#### No.5 absolute + 四个方向的值相等 [传送门](https://codepen.io/zhoubopro/pen/bGbmOrG)
>使用绝对定位布局，设置 ```margin:auto```，并设置 ```top、left、right、bottom``` 的值相等即可（不一定要都是 0）。
>这种方法一般用于弹出层，需要设置弹出层的宽高。

```html
<div class="wrapper">
  <p>absolute + 四个方向的值相等</p>
</div>
```

```css
.wrapper {
  width: 300px;
  height: 300px;
  border: 1px solid #ccc;
  position: relative;
}

.wrapper p {
  width: 170px;
  height: 20px;
  margin: auto;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}
```

#### No.6 grid [传送门](https://codepen.io/zhoubopro/pen/jONeXLW)
>代码量也很少，但兼容性不如flex，不推荐使用。

```html
<div class="wrapper">
  <p>grid</p>
</div>
```

```css
.wrapper {
  width: 300px;
  height: 300px;
  border: 1px solid #ccc;
  display: grid;
}

.wrapper p {
  align-self: center;
  justify-self: center;
}
```

#### No.7 ::after [传送门](https://codepen.io/zhoubopro/pen/zYOmypE)
>::after也可以

```html
<div class="wrapper">
  <img src="http://img2.imgtn.bdimg.com/it/u=1405109477,1383249963&fm=11&gp=0.jpg">
</div>
```

```css
.wrapper {
  width: 300px;
  height: 300px;
  border: 1px solid #ccc;
  text-align: center;
}

.wrapper::after {
  content: '';
  display: inline-block;
  vertical-align: middle;
  height: 100%;
}

.wrapper > img {
  vertical-align: middle;
}
```

#### No.8 ::before [传送门](https://codepen.io/zhoubopro/pen/qBWJLxK)
>另一种是配合 ```font-size: 0``` 共同施展的魔法。

```html
<div class="wrapper">
  <img src="http://img2.imgtn.bdimg.com/it/u=1405109477,1383249963&fm=11&gp=0.jpg">
</div>
```

```css
.wrapper {
  width: 300px;
  height: 300px;
  border: 1px solid #ccc;
  text-align: center;
  font-size: 0;
}

.wrapper::before {
  display: inline-block;
  vertical-align: middle;
  font-size: 14px;
  content: '';
  height: 100%;
}

.wrapper > img {
  vertical-align: middle;
  font-size: 14px;
}
```

```*ps：``` ```font-size: 0``` 的神秘之处在于，可以消除标签之间的间隙。
另外，因为伪元素搭配的，都是最基础的 CSS 写法，所以不存在兼容性的风险。
