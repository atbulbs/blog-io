---
title: Phaser TS项目对低版本iOS的兼容
date: 2019-05-05
tags: Phaser
categories: Phaser
---

# 引入 TS-Polyfill
```bash
$ npm i -S ts-polyfill
```

```javascript
import 'ts-polyfill'

// iOS 8, 9不能使用AudioContext播放音频
let isDisableWebAudio: boolean = false
const iOS: boolean = navigator.userAgent.match(/iPhone|iPad/i)
if (iOS) {
  let versionNum: number = parseInt((navigator.appVersion).match(/OS (\d+)_(\d+)_?(\d+)?/)[1], 10)
  if (versionNum <= 9) {
    isDisableWebAudio = true
  }
}

const phaserGameConfigObject = {
  // ...
  audio: {
    disableWebAudio: isDisableWebAudio
  }, 
}

```
