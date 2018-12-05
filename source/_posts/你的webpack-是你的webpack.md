---
title: 你的webpack,是你的webpack
date: 2018-03-12 09:38:25
tags: webpack
categories: 前端工程
---
# 前言
>同学,你平时用vue-cli构建你的vue项目么,你有研究过vue-cli生成的目录结构和文件里的代码么,你是否看到webpack的官方文档然后从入门到放弃呢,让我们用对新人更友好的姿势学习webpack,进阶webpack配置工程师:joy:,定制属于你自己的webpack吧:sunglasses:,happy hacking!:smile:
ps:如果你是在手机上浏览,横屏体验更好哦~

<!--more-->

# webpack简介
>一图胜百文,如下是webpack官网的示意图,在webpack的世界里,一切资源都是模块,包括代码文件、图片文件、字体文件等，webpack会处理这些模块的依赖关系,大白话就是顺藤:herb:摸:hand:瓜:watermelon:,找到你项目用到的所有的资源文件,然后打包成我们想要的静态资源
![webpack](/images/webpack3/webpack.jpg)

# 核心概念
我们写webapck的config对象主要是在写如下的配置
>## entry--入口
>>配置entry就是告诉webpack那条腾的根在哪里,也就是src/main.js
>## output--出口
>>配置output就是告诉webpack摸到的瓜要放在哪里,一般是dist目录
>## loader--加载器
>>webpack里每种模块都有它对应的loader去加载,我们在config.module.rules数组里面告诉webpack处理不同类型的模块的规则,也就是怎么加工:fork_and_knife:各种各样的瓜:watermelon:,如果一个模块需要好几个loader逐步处理,比如iview组件库推荐我们当test遇到.vue结尾的文件,先用iview-loader处理,再交给vue-loader处理,按照use数组逆序来处理
```javascript
{
  test: /\.vue$/,
  use: [
    {
      // iview-loader处理完成后
      // 再交给vue-loader处理
      loader: 'vue-loader',
      options: createVueLoaderOptions(isDev)
    },
    { 
      // iview-loader 先处理.vue文件
      loader: 'iview-loader',
      options: {
        prefix: false
      }
    }
  ]
}
```
>## plugins-插件
>>webpack里,loader不干的活儿都交给plugins,常用的有
>html-webpack-plugin          提供vue实例挂载的模板
>extract-text-webpack-plugin  生产环境提取css代码为外部样式
>uglifyjs-plugin              压缩js代码,不知道大神为啥给插件取这样的一个丑名字:joy_cat:
>## env--环境
>>webpack有开发环境和生产环境
>>>### development--开发环境
>>>在开发环境里我们配置devserver,设置http代理,让我们愉快的享受热更替,享受爬坑提醒
>>>### production--生产环境
>>>在生产环境里我们提取css为外部样式,分割出vue等类库的源码,压缩代码,生成资源的哈希以便利用长缓存
# 本文源码
>本文的源码在:octocat:[https://github.com/atbulbs/webpack-learning](https://github.com/atbulbs/webpack-learning)
>欢迎在github上为我点星星Star:star:,欢迎转发,也欢迎戳一下文章末尾的点击打赏
# 源码使用说明
>下载代码
```bash
$ git clone https://github.com/atbulbs/webpack-learning.git
```
>进入webpack3文件夹
```bash
$ cd webpack3
```
>安装依赖
```bash
$ npm i
```
>启动开发服务器,请修改自动打开的地址栏url为localhost:8000或者127.0.0.1:8000
>如果你用自己的源代码文件夹替换了src目录,请在webpack.config.js文件里设置你的代理服务器的IP
```bash
$ npm run dev
```
>打包项目代码,根目录下会多出一个dist目录
```bash
$ npm run build
```
>启动部署服务器,请在server.js文件里设置你的代理服务器的IP,在地址栏输入localhost:8001或者127.0.0.1:8001
```bash
$ cd server
$ npm i
$ node server
```
>
# 目录结构:open_file_folder:
```
webpack3  // 项目根目录
│   .babelrc       // babel配置文件
│   .editorconfig  // 编辑器配置文件
│   .eslintrc      // eslint规则配置文件
│   .gitignore     // git忽略配置的文件
│   .postcssrc.js  // css后处理规则配置文件
│   favicon.ico    // 网站的浏览器小图标文件
│   package.json   // 项目依赖和脚本的配置文件
│   README.md      // 项目的说明文件
│ 
├──build  // 项目构建目录
│     template.html           // vue挂载的模板文件
│     vue-loader.config.js    // vue-loader的配置文件
│     webpack.config.base.js  // webpack基础配置文件
│     webpack.config.js       // webpack的开发环境和生产环境配置文件
│
├──read-us // 配置文件的说明 
│      .babelrc.js   // babel的配置说明文件
│      .eslintrc.js  // eslint的配置说明文件
│
├──server  // 项目部署服务器目录
│     package.json  // 部署服务器所需的依赖的配置文件
│     server.js     // 部署服务器的脚本文件
│ 
└──src  // 源码目录
   │ 
   ├──assets // 资源目录
   │  │
   │  ├──fonts   // 字体文件目录
   │  ├──images  // 图片文件目录
   │  ├──styles  // 样式文件目录
   │  └──utils   // 工具代码目录
   │
   ├──base-components  // 基础组件目录
   ├──router           // 前端路由配置目录
   ├──store            // 前端数据仓库配置目录
   └──views            // 业务组件目录
  
```
# 来不及解释了,快上码:red_car:
>## vue-loader的配置
```javascript
/**
 * webpack3/build/vue-loader.config.js
 */

// 向外暴露一个函数,判断是否为开发环境,返回一个vue-loader的配置对象
module.exports = (isDev) => {
  return {
    // 不保留模板里的空格,压缩文件大小,
    // 消除空格引起的页面样式bug
    preserveWhitespace: false,
    // 提取vue组件内的CSS,如果不提取,加载速度快
    extractCSS: !isDev
    // css模块配置
    // cssModules: {
    //   // class的标识名称
    //   localIdentName: isDev ? '[path]-[name]-[hash:base64:5]' : '[hash:base64:5]',
    //   // 是否要驼峰命名
    //   camelCase: true
    // }
    // 组件的热更替,会根据环境变量配置
    // hotReload: true,
    // 预解析loader
    // preLoader: {},
    // 后置loader
    // postLoader: {}
  }
}

```
>## webpack的基本配置
```javascript
/**
 * webpack3/build/webpack.config.base.js
 */

const path = require('path')
const createVueLoaderOptions = require('./vue-loader.config')
const isDev = process.env.NODE_ENV === 'development'

const config = {
  // 'development'会做开发时的优化
  // 'production'会做生产环境的优化,比如压缩
  // 优化会自动配置
  // mode 在4.0一定要加,3.xxx不能加
  // mode: process.env.NODE_ENV || 'production',//只接收 'development' || 'production'
  // 编译目标运行的环境
  target: 'web',
  // 入口文件
  entry: path.join(__dirname, '../src/main.js'),
  // 出口文件
  output: {
    // hash是整个项目的hash
    // dev-server 不能使用chunkhash,不然会报错
    filename: 'bundle.[hash:8].js',
    path: path.join(__dirname, '../dist')
  },
  // 模块的loader规则
  // 声明 appropriate 合适的 loader
  // webpack 只支持原生js,配置loader后可以import非js的模块
  module: {
    rules: [
      // 启用eslint
      // {
      //   // 让eslint-loader为前置loader,在babel-loader之前检查
      //   enforce: 'pre',
      //   test: /.(jsx?|vue)$/,
      //   loader: 'eslint-loader',
      //   include: path.join(__dirname, '../src'),
      //   exclude: /node_modules/
      // },
      {
        test: /\.vue$/,
        use: [
          {
            loader: 'vue-loader',
            options: createVueLoaderOptions(isDev)
          },
          {
            loader: 'iview-loader',
            options: {
              prefix: false
            }
          }
        ]
      },
      {
        test: /\.jsx?$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader'
        },
        include: [
          path.resolve(__dirname, '../src')
          // 限定只在 src 目录下的 js/jsx 文件需要经 babel-loader 处理
          // 通常我们需要 loader 处理的文件都是存放在 src 目录
          // 缩小loader应用的文件范围,提高构建速度
        ]
      },
      {
        test: /\.css$/,
        use: [
          {
            loader: 'style-loader'
          },
          {
            loader: 'css-loader',
            options: {
              modules: true
            }
          }
        ]
      },
      {
        test: /\.styl$/,
        use: [
          // 用了vue-style-loader才支持css热更替
          'vue-style-loader',
          'css-loader',
          {
            loader: 'postcss-loader',
            options: {
              sourceMap: true
            }
          },
          'stylus-loader'
        ]
      },
      {
        test: /\.less$/,
        use: [
          'vue-style-loader',
          'css-loader',
          {
            loader: 'postcss-loader',
            options: {
              sourceMap: true
            }
          },
          'less-loader'
        ]
      },
      {
        test: /\.(jpg|png|jpeg|gif|svg)$/,
        use: [
          {
            // 依赖于file-loader(读取操作文件)
            loader: 'url-loader',
            options: {
              // 文件小于10000b时压缩成base64
              limit: 10000,
              // name: 文件原名
              // ext: 原文件扩展名
              // resources: 自定义文件夹名
              // path: 原路径
              // name: 'resources/[path][name].[hash:8].[ext]'
              name: 'static/imgs/[name].[hash:8].[ext]'
            }
          }
        ]
      },
      {
        test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
        loader: 'url-loader',
        options: {
          limit: 10000,
          // name: 'resources/[path][name].[hash:8].[ext]'
          name: 'static/fonts/[name].[hash:8].[ext]'
        }
      }
    ]
  },
  resolve: {
    // 从前到后自动匹配扩展名
    extensions: ['.js', '.vue', '.json', '.jsx'],
    // 别名,方便引入时使用
    alias: {
      '@': path.join(__dirname, '../src'),
      // 'styles': path.join(__dirname, '../src/assets/styles'),
      'views': path.join(__dirname, '../src/views')
    }
  }
}

module.exports = config

```
>## webpack的开发环境和生产环境的配置
```javascript
/**
 * webpack3/build/webpack.config.js
 */

const path = require('path')
const webpack = require('webpack')
// 合并webpack配置
const merge = require('webpack-merge')
// 提取css代码,生产环境使用外部样式,并可单独缓存
const ExtractTextPlugin = require('extract-text-webpack-plugin')
// 渲染的html模板
const HTMLPlugin = require('html-webpack-plugin')
// 压缩js代码
const UglifyJsPlugin = require('uglifyjs-webpack-plugin')
const baseConfig = require('./webpack.config.base')

const isDev = process.env.NODE_ENV === 'development'

const defaultPlugins = [
  // 注册全局环境变量,提供给webpack,vue和我们自己使用
  new webpack.DefinePlugin({
    'process.env': {
      // 注意双引号,否则webpack转义代码会解析成变量,调用时会报错
      // webpack判断process.env去选择不同版本的源码去打包
      // 比如开发版源码(有错误信息提示),生产版源码
      NODE_ENV: isDev ? '"development"' : '"production"'
    }
  }),
  new HTMLPlugin({
    template: path.join(__dirname, './template.html'),
    minify: { // 压缩 HTML 的配置
      minifyCSS: true, // 压缩 HTML 中出现的 CSS 代码
      minifyJS: true // 压缩 HTML 中出现的 JS 代码
    }
  })
]

const devServer = {
  // 可用localhost,127.0.0.1,以及内网IP访问
  // 但注意不能用0.0.0.0:8000打开
  host: '0.0.0.0',
  port: 8000,
  headers: {
    'Access-Control-Allow-Origins': '*'
  },
  // 代理
  proxy: {
    '/api': {
      target: 'http://yourIPAdresss',
      changeOrigin: true,
      pathRewrite: {
        '^/api': '/'
      }
    }
  },
  overlay: {
    // 出现的错误显示到页面
    errors: true
  },
  // 处理url和页面的映射
  historyApiFallback: {
    index: '/dist/index.html'
  },
  // hotModuleReplacement
  // 修改代码后页面无刷新更新,不会丢失数据
  hot: true,
  // 启动时自动打开浏览器
  // 但是改webpack配置后也会打开新页面
  open: true
}

let config

if (isDev) {
  config = merge(baseConfig, {
    // 映射源码和编译后的代码,方便在浏览器调试代码
    // source-map是最完整的映射代码,但是效率低,文件大,编译和调试慢
    // eval-source-map会导致代码对不齐
    // 官方推荐cheap-module-eval-source-map
    // https://webpack.js.org/configuration/devtool/#development
    devtool: 'cheap-module-eval-source-map',
    devServer,
    plugins: defaultPlugins.concat([
      // 启动hotModuleReplacement功能
      new webpack.HotModuleReplacementPlugin(),
      // 减少不需要的信息展示
      new webpack.NoEmitOnErrorsPlugin()
    ])
  })
} else {
  config = merge(baseConfig, {
    entry: {
      app: path.join(__dirname, '../src/main.js'),
      // 把第三方类库源码单独打包,利用长缓存,提高加载速度
      vendor: [
        'vue', 'vue-router', 'vuex', 'axios', 'iview',
        'echarts', 'js-cookie', 'js-file-download'
      ]
    },
    output: {
      // 使用chunkhash,每个文件会单独生成一个hash
      // 生产模式必须用chunkhash,不然vendor的哈希也会变
      // 就没有vendor的意义了
      filename: '[name].[chunkhash:8].js',
      path: path.join(__dirname, '../dist')
    },
    module: {
      rules: [
        {
          test: /\.styl$/,
          use: ExtractTextPlugin.extract({
            // 会把css-loader编译的代码
            // 用js写成一个style标签
            fallback: 'vue-style-loader',
            use: [
              // css-loader只是处理css文件
              {
                loader: 'css-loader',
                options: {
                  // 压缩css代码
                  minimize: true
                }
              },
              {
                loader: 'postcss-loader',
                options: {
                  sourceMap: true
                  // 用stylus-loader生成的sourceMap
                  // 效率会高,不然postcss会给提醒
                  // 官方文档有说明
                }
              },
              // 一层层往上扔,会生成sourceMap
              'stylus-loader'
            ]
          })
        },
        {
          test: /\.less$/,
          use: ExtractTextPlugin.extract({
            fallback: 'vue-style-loader',
            use: [
              {
                loader: 'css-loader',
                options: {
                  minimize: true
                }
              },
              {
                loader: 'postcss-loader',
                options: {
                  sourceMap: true
                }
              },
              'less-loader'
            ]
          })
        },
        {
          test: /.css$/,
          use: ExtractTextPlugin.extract({
            fallback: 'style-loader',
            use: [
              {
                loader: 'css-loader',
                options: {
                  minimize: true
                }
              }
            ]
          })
        }
      ]
    },
    plugins: defaultPlugins.concat([
      // 通过css文件内容进行哈希生成的文件名
      new ExtractTextPlugin('styles.[contentHash:8].css'),
      // 单独打包
      new webpack.optimize.CommonsChunkPlugin({
        name: 'vendor'
      }),
      // 优化
      // 把与webpack相关的代码单独打包到一个文件
      // 避免有新的模块加入时,webpack会给模块加一个id
      // 插入的顺序在中间时,会导致后面每个模块的id发生变化
      // 会导致打包出的内容的hash会变化,就不能使用长缓存
      // 注意vendor要放在runtime的前面
      new webpack.optimize.CommonsChunkPlugin({
        name: 'runtime'
      }),
      new webpack.NamedChunksPlugin(),
      new UglifyJsPlugin()
    ])
  })
}

module.exports = config

```
>## 部署服务器的脚本文件
```javascript
/**
 * webpack3/server/server.js
 */

const Koa = require('koa')
const path = require('path')
// 处理静态资源
const Static = require('koa-static')
// 设置服务器代理
const proxy = require('http-proxy-middleware')
const send = require('koa-send')

const app = new Koa()

const Public = Static(path.join(__dirname, '../dist'))
app.use(Public)

const favicon = async (ctx, next) => {
  if (ctx.path === '/favicon.ico') {
    await send(ctx, '/favicon.ico', {
      root: path.join(__dirname, '../')
    })
  } else {
    await next()
  }
}
app.use(favicon)

const proxyOptions = {
  target: 'http://yourIPAdresss',
  changeOrigin: true,
  pathRewrite: {
    '^/api': '/'
  }
}
const proxyMiddleware = async (ctx, next) => {
  if (ctx.url.startsWith('/api')) {
    ctx.respond = false
    return proxy(proxyOptions)(ctx.req, ctx.res, next)
  }
  return next()
}
app.use(proxyMiddleware)

const HOST = process.env.HOST || '0.0.0.0'
const PORT = process.env.PORT || 8001

app.listen(PORT, HOST, () => {
  console.log('success!')
  console.log(`your server is listening on ${HOST}:${PORT}`)
})

```
>ps: 本文的配置是基于webpack3.0版本的,后期会出webpack4.0版本的配置,欢迎关注
>本文的源码在:octocat:[https://github.com/atbulbs/webpack-learning](https://github.com/atbulbs/webpack-learning)
>欢迎在github上为我点星星Star:star:,欢迎转发,也欢迎戳一下文章末尾的点击打赏

