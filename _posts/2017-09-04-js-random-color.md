---
layout: post
title: '使用javascript 自動random顏色'
date: 2017-09-04 03:18
comments: true
categories: Jquery
tags: Jquery CSS
reference:
  name:
    - Random-color-byjquery
  link:
    - https://stackoverflow.com/questions/18508293/random-color-for-animate-in-jquery
---

```js
var newColor = '#'+(0x1000000+(Math.random())*0xffffff).toString(16).substr(1,6);
jQuery(".font-style").animate({color: newColor}, 2000); // animate
```