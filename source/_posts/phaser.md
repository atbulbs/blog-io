---
title: Phaser
date: 2018-10-22 16:08:17
tags: Phaser
categories: Phaser
---

# 简介
2D开源游戏框架，同时支持Canvas和WebGL渲染引擎

# 版本
1. 最新版本 3.x
2. 社区维护的 2.x版本，Phaser CE —— Community Edition

# Phaser3教程
开发你的第一个Phaser3游戏，官方教程[http://phaser.io/tutorials/making-your-first-phaser-3-game](http://phaser.io/tutorials/making-your-first-phaser-3-game)
<!-- more -->
Phaser3官方API文档[https://github.com/photonstorm/phaser3-docs/tree/master/docs](https://github.com/photonstorm/phaser3-docs/tree/master/docs)

# 我用webpack和vue模块化改版官方的Phaser3入门游戏，带详细注释
在线demo地址[https://atbulbs.github.io/vue-phaser-demo/index.html](https://atbulbs.github.io/vue-phaser-demo/index.html)
git仓库地址[https://github.com/atbulbs/vue-phaser](https://github.com/atbulbs/vue-phaser)

# 坑
* 资源文件被base64压缩后Phaser无法找到， 注意webpack的url-loader配置
* 当game.js文件与game.vue文件重名，引入路由时需加后缀