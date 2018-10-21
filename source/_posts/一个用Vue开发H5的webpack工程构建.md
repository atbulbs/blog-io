---
title: 一个用Vue开发H5的webpack工程构建
date: 2018-06-18 17:03:11
tags: webpack
categories: 前端工程
---


# 8bulbs-project webpack-vue
&#160; &#160; &#160; &#160;模块化强迫症:joy:webpack配置工程师:sunglasses:的webpack-vue配置,采用最新的webpack v-4.16和vue-loader v-15.3, 开箱:package:即用, 引入vw适配(建议UI图为375px宽度, 写px单位会自动转为vw)和wechat-js-sdk(方便二次分享以及调用微信其他API),二次封装axios,更高效:rocket:更激爽:kiss:的构建H5项目

## 基于此配置构建的组件/插件库, 自己撸的一些小轮子, welcome npm install :smile:

>npm package  [https://www.npmjs.com/package/jsl-vue-h5](https://www.npmjs.com/package/jsl-vue-h5)

>git仓库  [https://github.com/8bulbs/jsl-vue-h5](https://github.com/8bulbs/jsl-vue-h5)

>在线demo [https://8bulbs.github.io/jsl-vue-h5-demo-online/](https://8bulbs.github.io/jsl-vue-h5-demo-online/)

>demo仓库 [https://github.com/8bulbs/jsl-vue-h5-demo](https://github.com/8bulbs/jsl-vue-h5-demo)

<!--more-->

## build with the latest version of dependencies

``` bash
# clone repository
$ git clone https://github.com/8bulbs/webpack-vue.git

# change directory
$ cd webpack-vue

# install ncu at global
$ npm install -g npm-check-updates

# use ncu
$ ncu -a

# install dependencies
$ npm install

# run the develop server
$ npm start

# build for production with minification
$ npm run build

```
## to deploy your project (proxy and history mode is surported)

```bash
# change directory
$ cd deploy-server

# install dependencies
$ npm install

# install pm2 at global to manager your production process
$ npm install pm2 -g

# run the deploy server with pm2
$ pm2 start server.js --watch

```

## directory path list

```bash
├─build
│  ├─commonConfig
│  └── ... # 开发和生产的通用配置, 模式, 目标, 别名解析
│  ├─commonPlugins
│  └── ... # 抽取出API请求
│  ├─commonRules
│  └── ... # 通用的编译规则
│  ├─devConfig
│  └── ... # 开发环境的配置, 入口, 出口, 开发服务器, 开发工具, 插件, 编译规则
│  ├─prodConfig
│  └── ... # 生产环境的配置, 入口, 出口, 插件, 编译规则, 优化
│  └─utils
│  └── ... # 工具类
├─deploy-server
│  └── ... # 部署服务器, 支持请求代理和路由的history模式
└─src
    ├─api
    │  └── ... # 抽取出API请求, axios的二次封装
    ├─libs
    │  └── ... # 工具类
    ├─assets
    │  ├─images
    │  └── ... # 图片资源
    │  ├─js
    │  └── ... # 常量定义, 微信js开发工具包的调用
    │  └─styles
    │  └── ... # css重置, 变量和mixin的设置
    ├─base-components
    │  └── ... # 基础组件
    ├─components
    │  └── ... # 业务组件
    ├─pages
    │  └── ... # 页面视图
    ├─router
    │  └── ... # 路由配置, 路由守卫, 滚动行为
    └─store
      └── ... # vuex状态管理

```

