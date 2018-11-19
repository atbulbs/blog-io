---
title: 你想成为风一般的爱(ban)码(zhuan)仕(de)么
date: 2018-11-18 12:15:17
tags: 开发效率
categories: 开发效率
---

# user snippets用户自定义代码片段, 贴上我的
<!-- more -->
```javascript
// vue.json
{
	// Place your snippets for vue here. Each snippet is defined under a snippet name and has a prefix, body and 
	// description. The prefix is what is used to trigger the snippet and the body will be expanded and inserted. Possible variables are:
	// $1, $2 for tab stops, $0 for the final cursor position, and ${1:label}, ${2:another} for placeholders. Placeholders with the 
	// same ids are connected.
	// Example:
	// "Print to console": {
	// 	"prefix": "log",
	// 	"body": [
	// 		"console.log('$1');",
	// 		"$2"
	// 	],
	// 	"description": "Log output to console"
	// }
	"Vue": {
		"prefix": "vue",
		"body": [
			"<template>",
			"\t<div class=\"$1-root\">",
			"\t\t",
  		"\t</div>",
			"</template>",
			"",
			"<script type=\"text/ecmascript-6\">",
			"export default {",
			"\tdata () {",	
			"\t\treturn {",		
			"\t\t\tdata: ''",			
			"\t\t}",	
			"\t}",
			"}",
			"</script>",
			"",
			"<style scoped lang=\"stylus\" rel=\"stylesheet/stylus\">",
			"@import \"~styles/mixin\"",
			"",
			".$1-root",
			"</style>",
		],
		"description": "vue"
	},
	"Print to console": {
		"prefix": "lg",
		"body": [
			"console.log('$1')"
		],
		"description": "Log output to console"
	},
	"Print to warn": {
		"prefix": "cw",
		"body": [
			"console.warn('$1')"
		],
		"description": "Print to warn"
	},
	"Arrow function": {
		"prefix": "af",
		"body": [
			"($1) => {",
			"  $2",
			"}"
		],
		"description": "Arrow function"
	},
	"Let": {
		"prefix": "l",
		"body": [
			"let $1 = $2"
		],
		"description": "Let"
	},
	"Const": {
		"prefix": "c",
		"body": [
			"const $1 = $2"
		],
		"description": "Const"
	},
	"Var": {
		"prefix": "v",
		"body": [
			"var $1 = $2"
		],
		"description": "Var"
	},
	"Import": {
		"prefix": "i",
		"body": [
			"import $1 from '$2'"
		],
		"description": "import"
	},
	"Import object": {
		"prefix": "im",
		"body": [
			"import { $1 } from '$2'"
		],
		"description": "import object"
	},
	"Export": {
		"prefix": "e",
		"body": [
			"export $1"
		],
		"description": "export"
	},
	"Function": {
		"prefix": "fn",
		"body": [
			"function $1 ($2) {",
			"\t$3",
			"}"
		],
		"description": "function"
	},
	"Arrow function": {
		"prefix": "af",
		"body": [
			"($1) => {",
			"\t$2",
			"}"
		],
		"description": "arrow function"
	},
	"Return": {
		"prefix": "r",
		"body": [
			"return $1"
		],
		"description": "return"
	},
	"If": {
		"prefix": "if",
		"body": [
			"if ($1) {",
			"  $2",
			"}"
		],
		"description": "if"
	},
	"Comment": {
		"prefix": "/",
		"body": [
			"/**",
			" * $1",
			" **/"
		],
		"description": "comment"
	}
}
```
# 用法
&#160; &#160; &#160; &#160;新建一个`test.vue`文件输入`vue`, 你会看到提示
![vue](/images/user-snippets/vue.png)
&#160; &#160; &#160; &#160;然后`tab`, 就会duang~, 有了如下代码, 注意$1出会用光标占位
![template](/images/user-snippets/template.png)
&#160; &#160; &#160; &#160;同理, `lg` + `tab`你会得到你经常使用的断点, `af` + `tab`你会得到一个风骚的箭头函数

# emmet / zen coding 
## html简写
```javascript
// '#' 创建一个id特性
// '.' 创建一个类特性
// '[]' 创建一个自定义特性
// '>' 创建一个子元素
// '+' 创建一个兄弟元素
// '^' 提升元素层次
// '*' 相当于乘号,会创建n次相同的东西
// '$' 代替一个自增的数字
// '$$' 用于有填充位的数字比如00,01
// '{}' 创建元素的文本
// 示例
// ul#container>li.item.highlight[data="" role=""]{text$}*5^ol>li*2
// 效果
<ul id="container">
  <li class="item highlight" data="" role="">text1</li>
  <li class="item highlight" data="" role="">text2</li>
  <li class="item highlight" data="" role="">text3</li>
  <li class="item highlight" data="" role="">text4</li>
  <li class="item highlight" data="" role="">text5</li>
</ul>
<ol>
  <li></li>
  <li></li>
</ol>
```
## css简写
```css
/* m0 */
/* p0 */
/* w100p */
/* h100p */
/* w100 */
/* h100 */
/* fl */
/* fr */
/* bgc */
/* l300 */
/* r300 */
/* mb300 */
/* df */
/* jcsb */
/* dt  */
/* duang~ 效果 */
width: 100px;
margin: 0;
padding: 0;
width: 100%;
height: 100%;
width: 100px;
height: 100px;
float: left;
float: right;
background-color: #fff;
left: 300px;
right: 300px;
margin-bottom: 300px;
display: flex;
justify-content: space-between;
display: table;
```