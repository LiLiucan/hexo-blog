---
title: git commit message 规范
date: 2020-11-23 12:32:38
tags: 代码规范
issueNumber:
---

## Commit message格式
每条commit message包含如下几部分：header、body和footer。header有一个固定的格式，包含三部分：type、scope和subject。

一条commit message的标准格式如下：
```html
<type>(<scope>): <subject>
 
<BLANK LINE>
 
<body>
 
<BLANK LINE>
 
<footer>
```

header是必须的，header中的scope是可选的。
commit message的每一行文字最好不要超过100个字符，这样的话可以保证易读性。

## Commit message类型

commit message类型也就是上面说的type，有如下几种：

- revert: 回退一个commit，subject中需要写明被回退的commit的hash，例如：revert: This reverts commit <hash>
- **feat**: 新特定
- **fix**：修复bug
- **docs**：文档修改
- **style**：一些对代码实际意义没有影响的修改（例如格式化，增加逗号，空格等等）
- **refactor**: 既不是新特性也不是修复bug
- **perf**：性能优化
- **test**：单元测试或者集成测试相关的修改
- **chore**：构建流程及工程化相关的修改


## Scope
scope为commit的影响范围，如果一个项目有多个模块，这个scope就可以对应一个模块。
以zzb-js-sdk这个项目的commit message为例：
```
chore(zzb-react-components): 打包配置新增externals配置，react和react-dom不打包进最终代码
```

## Subject
subject用于简明扼要的描述修改的内容：

- 使用祈使句，比如：修改了...
- 如果用英文，不要大写首字母
- 结尾不要用标点

## Body
和subject一样，这部分应该尽量简明扼要。写清楚此次修改的动机以及和修改之前的差别。

## Footer
这部分用于说明破坏性的修改（Breaking Changes）以及需要关闭的github issuses。

我觉的不同的项目可以根据自己的实际情况定制这部分。我的做法是在这里放此次修改相关的jira链接（如果有的话）。

## 一个示例
一个包含完整内容的commit message差不多是这个样子：

```

chore(zzb-utils): 优化打包配置
 
1. 升级zzb-react-components，优化打包后的代码体积
2. webpack配置中新增BundleAnalyzerPlugin
3. webpack配置中新增externals配置，解决将库代码打包进模块导致的代码体积过大的问题
 
jira: http://jira.xxxx.com/xxxxxx
```

