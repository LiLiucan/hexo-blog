---
title: 前端移动端rem适配方案
date: 2019-06-14 10:17:57
categories: 前端
tags: 移动端适配
---

### 适配方案选择
参考网易移动端适配方案,使用rem方案做页面适配，以750px设计稿为基准适配：


### 适配代码
```javascript
<script>
  (function () {
    function resize() {
      var deviceWidth = document.documentElement.clientWidth;
      document.documentElement.style.fontSize = (deviceWidth / 7.5) +'px';
 
      var documentElement = document.documentElement;
      var docElFontSize = documentElement.style.fontSize.replace(/px/gi, '')
      var computedFontSize = window.getComputedStyle(documentElement)['font-size'].replace(/px/gi, '')
      docElFontSize != computedFontSize && (documentElement.style.fontSize = docElFontSize * docElFontSize / computedFontSize + 'px')
  }
    resize();
    window.onresize = resize;
  })()
</script>
```

### 示例代码
750px设计稿px单位可以直接换算为rem(1rem=100px)
```css
.test {
  width: 100%;
  height: 1rem; 
}
```

### 其他类似适配方案对比
淘宝和网易移动端rem适配方案对比：

https://juejin.im/post/5b346e8f5188251e1d39bd09?utm_source=gold_browser_extension

