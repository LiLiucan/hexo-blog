---
layout: blog
title: git使用问题记录
date: 2020-03-31 11:17:20
categories: 版本控制系统
tags: git
issueNumber:
---

## 在.gitignore 中配置已经被添加到 git 追踪的文件，该文件依然被 git 追踪

- 问题原因：
  ![问题原因](git.webp)
- 解决方法：

  > 1. 文件
  >    \$ git rm --cache file/path/to/ignore
  > 2. 目录
  >    \$ git rm -r --cache dir/path/to/ignore
