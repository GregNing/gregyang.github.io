---
layout: post
title: 'BootstrapDialog 使用 以及文件引用'
date: 2017-09-02 16:16
comments: true
categories: Bootstrap
tags: Bootstrap JS
reference:
  name:
    - bootstrap3-dialog
  link:
    - https://github.com/nakupanda/bootstrap3-dialog
---

使用前請先去[bootstrap3-dialog](https://github.com/nakupanda/bootstrap3-dialog)這網址下載以下文件:<br>
`css : bootstrap.min.css ,  bootstrap-dialog.css`<br>
`js : jquery-1.11.1.min.js, bootstrap.min.js,  bootstrap-dialog.js`

```HTML
<!-- Modal 彈出框的結構 -->
<div class="modal fade" id="myModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
    <div class="modal-header">
      <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-   							hidden="true">×</span></button>
      <h4 class="modal-title" id="myModalLabel">Modal title</h4>
    </div>
    <div class="modal-body">
    //(Message訊息內容)
    </div>
    <div class="modal-footer">
      <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
      <button type="button" class="btn btn-primary">Save changes</button>
    </div>
  </div>
  </div>
</div>
```
#### 使用JS
```js
BootstrapDialog.show({
  title: '文章Title',
  message: '請輸入您所要的內容!',
  type: BootstrapDialog.TYPE_INFO, (跟隨Title的顏色)
  size: BootstrapDialog.SIZE_SMALL,(bootstrap的大小)
  cssClass: 'bootstraplogin',可以任意套用你所要的 class樣式(css))
  buttons: [{ (按鈕)
  cssClass: 'btn-warning',
  label: 'Close', (按鈕的名稱)
  action: function (dialog) {
    dialog.close();
  }
  }]
});
```