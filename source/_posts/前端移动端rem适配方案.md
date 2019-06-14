---
title: 前端移动端rem适配方案
date: 2019-06-14 10:17:57
categories: 前端
tags: 移动端适配
---

参考网易移动端适配方案,使用rem方案做页面适配，以750px设计稿为基准适配：


```
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

淘宝和网易移动端rem适配方案对比：

https://juejin.im/post/5b346e8f5188251e1d39bd09?utm_source=gold_browser_extension

