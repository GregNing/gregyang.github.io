---
layout: post
title: 'CheckBox Style'
date: 2017-09-20 09:26
comments: true
categories: CSS
tags: CSS
reference:
  name:
    - checkbox codepen style
    - style-checkboxes-with-css
  link:
    - https://codepen.io/bbodine1/pen/novBm
    - https://paulund.co.uk/style-checkboxes-with-css
---
##### 我選用的是squaredFour
```HTML
<div class="squaredFour">
<input type="checkbox" value="None" id="squaredFour" name="check" checked />
<label for="squaredFour"></label>
</div>
```
##### CSS
```css
.squaredFour {
  width: 20px;
  position: relative;
  margin: 20px auto;
}

.squaredFour input[type=checkbox] {
  visibility: hidden;
}

.squaredFour input[type=checkbox]:checked + label:after {
  opacity: 1;
}

.squaredFour label {
  width: 20px;
  height: 20px;
  cursor: pointer;
  position: absolute;
  top: 0;
  left: 0;
  background: #fcfff4;
  background: linear-gradient(top, #fcfff4 0%, #dfe5d7 40%, #b3bead 100%);
  border-radius: 4px;
  box-shadow: inset 0px 1px 1px white, 0px 1px 3px rgba(0,0,0,0.5);
}

.squaredFour label:after {
  content: '';
  width: 9px;
  height: 5px;
  position: absolute;
  top: 4px;
  left: 4px;
  border: 3px solid #333;
  border-top: none;
  border-right: none;
  background: transparent;
  opacity: 0;
  transform: rotate(-45deg);
}

.squaredFour label:hover::after {
  opacity: 0.5;
}
```