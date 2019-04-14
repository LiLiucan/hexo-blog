---
title: hexo第三方功能组件添加及配置
date: 2019-04-14 12:56:55
categories: 搭建博客
tags: hexo
---


### 访客统计
搜了下，大家普遍使用最多的是[不蒜子](https://busuanzi.ibruce.info/)，这是个第三方的js sdk，接入方便简单。
将如下代码copy至/themes/next/layout/_partials/footer.swig文件的底部就行

    <script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
    <span id="busuanzi_container_site_pv">本站总访问量<span id="busuanzi_value_site_pv"></span>次</span>




