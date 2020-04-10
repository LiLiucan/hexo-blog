---
title: eggjs项目搭建问题集锦
date: 2020-04-10 15:57:26
categories: web应用开发
tags: eggjs
issueNumber: 
---


### egg-sequelize 添加 hook 时，如何获取 ctx

> 在每次查询时，我需要将 ctx 中的某个值赋予到查询语句中。
> 但是在 sequelize 的 hook 函数中无法通过 this 获取到 ctx，通过 app.createAnonymousContext() 创建的 ctx 是新的 ctx，没有我所需要的值。
> 目前所能想到的办法，是将 ctx 添加到 options 中，但是这个实现方式有点繁琐。

```javascript
module.exports = app => {
  const { INTEGER, STRING, DATE } = app.Sequelize;

  const User = app.model.define('User', {
    id: { type: INTEGER, autoIncrement: true, primaryKey: true },
    name: { type: STRING(25), allowNull: false, defaultValues: '' },
  });

  User.hook('beforeFind', options => {
    if (!options.where) options.where = {};
    options.where.name = this.ctx.get('name'); // Cannot read property 'get' of undefined
  });

  return User;
}
```
 