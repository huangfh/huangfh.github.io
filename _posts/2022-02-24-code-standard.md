---
layout: post
read_time: true
show_date: true
title: "代码规范"
date: 2022-02-24
# img: posts/20210324/Npm_logo.png
tags: [Eslint, Prettier]
author: huangfh
description: ""
toc: yes
---
## eslint配置

```javascript
{
 //执行环境
 env: {
    browser: true,
    commonjs: true,
    es6: true,
    node: true,
    jquery: true,
    worker: true,
  },
  parserOptions: {
    sourceType: 'module',
  },
  // 预设好的规则集
  extends: ['plugin:react/recommended'],
  settings: {
    react: {
      version: 'detect',
    },
  },
  // 为eslint增加新的检查规则集
  plugins: ['react'],
  globals: {
    // 全局变量 global 不允许被重新赋值
    global: false,
    // 全局变量 window 不允许被重新赋值
    window: false,
    // 单元测试
    jest: false,
    describe: false,
    test: false,
    it: false,
    expect: false,
  },
  //分别配置每个规则
  rules: {
    'no-useless-constructor': 'off',
  }
};
```
eslint，prettier区别

eslint（包括其他一些 lint 工具）的主要功能包含代码格式的校验，代码质量的校验。
prettier 只是代码格式的校验（并格式化代码），不会对代码质量进行校验。
说明
代码格式问题通常指的是：单行代码长度、tab长度、空格、逗号表达式等问题。
代码质量问题指的是：未使用变量、三等号、全局变量声明等问题。

husky ，lintstage
静态代码检查 如果放到远程hook，流程太长，所以放到本地，使用husky。结合lint-stage使用，stage就是待提交区，作用是让检查只检查本次修改的代码。

```javascript
{
    "husky": {
        "hooks": { "pre-commit": "lint-staged" }
    },
    // 文件过滤。格式化代码，并对格式化之后的代码重新提交。
    "lint-staged": { "src/**/*.js": ["eslint --fix", "git add"] }
}

```




