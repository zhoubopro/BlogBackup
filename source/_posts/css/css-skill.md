---
title: 一些常被忽略的CSS技巧
top: true
cover: false
date: 2019-09-27 09:49:13
password:
summary:
tags:
- css
categories:
- css
---

#### 1.使用CSS复位
CSS复位可以在不同的浏览器上保持一致的样式风格。您可以使用CSS reset 库[Normalize](http://necolas.github.io/normalize.css/)等，也可以使用一个更简化的复位方法：
```css
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
}
```

#### 2.CSS禁用鼠标事件
```css
.disabled {
  pointer-events: none;
  cursor: default;
  opacity: 0.7;
}
```

#### 3.制造模糊文本
```css
.blurry-text {
  color: transparent;
  text-shadow: 0 0 5px rgba(0,0,0,0.5);
}
```

#### 4.跨浏览器的透明
```css
.transparent {
  filter: alpha(opacity=50); /* internet explorer */
  -khtml-opacity: 0.5;       /* khtml, old safari */
  -moz-opacity: 0.5;         /* mozilla, netscape */
  opacity: 0.5;              /* fx, safari, opera */
}          
```

#### 5.CSS3：全屏背景
```css
body { 
  background: url('images/bg.jpg') no-repeat center center fixed; 
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
}
```

#### 6.强制出现垂直滚动条
```css
html { 
  height: 101% 
}
```

#### 7.强制换行
```css
pre {
  white-space: pre-wrap;       /* css-3 */
  white-space: -moz-pre-wrap;  /* Mozilla, since 1999 */
  white-space: -pre-wrap;      /* Opera 4-6 */
  white-space: -o-pre-wrap;    /* Opera 7 */
  word-wrap: break-word;       /* Internet Explorer 5.5+ */
}
```
