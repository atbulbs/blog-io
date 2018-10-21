---
title: random的正确打开方式
date: 2018-06-12 08:52:17
tags: JavaScript
categories: JavaScript
---

### 洗牌
<!-- more -->

```javascript

function shuffle (arr) {
  if (arr.constructor !== Array) {	
    return new TypeError('array is required')
  } else {
    return arr.sort(() => window.Math.random() < 0.5)
  }
} 

const testArr = [1, 2, 3, 4, 5]

const result = shuffle(testArr)

console.log(result)

```