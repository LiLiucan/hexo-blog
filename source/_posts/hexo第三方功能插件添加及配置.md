---
title: hexo第三方功能插件添加及配置
date: 2019-04-14 12:56:55
categories: 搭建博客
tags: hexo
issueNumber: 3
---

本博客使用的是bmw主题，所以插件引入的时候，目录结构和文件会与其他主题有区别。
插件本身是一个独立的功能模块，一般需要引入一个js文件，同时指定一个该插件挂载的html标签。

### 访客统计
访客统计可以使用[不蒜子](https://busuanzi.ibruce.info/)。
编辑themes/bmw/layout/_partials/footer.ejs文件，将footer标签修改为如下内容：

```javascript
    <footer>
        <p class="site-info">
            博客已萌萌哒运行
            <span id="time-to-now"></span>
            <% if(theme.footer.more){ %>
            <%- theme.footer.more %>
            <% } %>
        </p>

        <div>
        <script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
        本站总访问量 <span id="busuanzi_value_site_pv"></span> 次&nbsp&nbsp&nbsp
        本站访客数<span id="busuanzi_value_site_uv"></span>人次
        </div>
    </footer>
```

### 评论
现在使用最多的应该是[gittalk](https://github.com/gitalk/gitalk)了。
首先要[在github上注册一个OAuth应用](https://github.com/settings/applications/new)，注意：注册应用的时候，Authorization callback URL一定要填的是你博客的地址。
注册成功后就可以拿到使用gitalk需要的CientID和Client Secret等信息，下面就可以在博客中引入gitalk插件了。

#### gitalk插件的引入
1. 在文章信息中添加gitalk iusse id信息，方法如下：
编辑themes/bmw/layout/component/passage-viewer.ejs文件,找到class为article-top-meta的div，在标签内追加如下内容：
```javascript
    <span>
      评论编号 : 
      <span id="issueNumber"><%- page.issueNumber %></span>
    </span>
```

2. 新建文件themes/bmw/layout/_partial/comment-gitalk.ejs,文件内容如下：
```javascript
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/gitalk@1/dist/gitalk.css">
    <script src="https://cdn.jsdelivr.net/npm/gitalk@1/dist/gitalk.min.js"></script>
    <div id="gitalk-container"></div>

    <script defer>
        var issueNumber = document.querySelector('#issueNumber').innerHTML;
        var gitalk = new Gitalk({
            clientID: '你的Clinet ID',
            clientSecret: '你的Clinet Secret',
            repo: 'liliucan.github.io',
            owner: 'Liliucan',
            admin: ['Liliucan,'],
            // id: decodeURIComponent(window.document.title),      // Ensure uniqueness and length less than 50
            number: Number(issueNumber),
            distractionFreeMode: false,  // Facebook-like distraction free mode
            createIssueManually: false
        });
        gitalk.render('gitalk-container')
    </script>
```


3. 在themes/bmw/layout/post.ejs文件合适的位置引入步骤2中的文件：
```javascript
 <%- partial("_partial/comment-gitalk") %>
```

4. 新建博客的时候要注意： 

a) 在GitHub对应的repo中新建一个issue

b) 博客header中要新增issueNumber字段，issueNumber为上述issue的id

```
---
title: Ubuntu安装postresql
date: 2019-09-09 22:55:57
tags:
issueNumber: 4
---
```

我使用了issue id作为查询gitalk记录的条件，步骤1是将步骤4中的issueNumber字段作为文章的‘评论编号’显示，步骤2将文章的‘评论编号’信息作为number字段提交给gitalk，用户获取相应的issue评论。





