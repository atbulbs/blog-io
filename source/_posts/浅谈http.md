---
title: 浅谈http
date: 2018-13-08 13:56:25
tags: http
categories: http
---

### 网络协议分层(经典五层模型)
![经典五层模型](/images/http/five.jpg)
1.  应用层(http, 只有请求和响应, 没有连接, tcp提供连接)
2.  传输层(tcp/udp提供端对端的服务, 分包和组装, 一个tcp里可以有多个http)
3.  网络层(寻找网络设备的逻辑)
4.  数据链路层(电路的连接)
5.  物理层(硬件设备)

### 发展历史
1. http/0.9(只有get命令, 没有header)
2. http/1.0(更多命令, 有header)
3. http/1.1(持久连接, pipeline, 虚拟host)
4. http2(数据以二进制传输, 同一个连接里发送多个请求不再需要按顺序, 头信息压缩, 服务端推送)

### http三次握手
![http三次握手](/images/http/three.jpg)
目的: 防止因为延迟或其他网络故障创建无用的连接, 浪费服务器开销
SYN: 标志位
Seq: 序列, 每一次都有一个新序列
ACK: 序列 + 1

### URI URL URN
1.  URI Uniform Resource Identifier 统一资源标识符, 包含URL 和 URN
2.  URL Uniform Resource Locator 统一资源定位器
http://user:pwd@host.com:80/path?query=string#hash
3. URN 永久统一资源定位符(在资源移动后也能被找到)

### 报文
![报文](/images/http/post.jpg)
请求起始行: 请求方法method + 资源地址url的路由path + 协议版本
响应起始行: 协议版本 + code + code的含义
头部header
空行: 分格头部和主体
主体body

### http客户端client
1. browser
2. curl
```bash
curl -v www.baidu.com
```

### CORS跨域请求
```javascript
res.writeHead(200, { 'Access-Control-Allow-Origin':  '*' }, { 'Access-Control-Allow-Headers': 'X-Test-Cors' }, { 'Access-Control-Allow-Methods': 'get, post, head, delete, put' })
```
1. 跨域请求其实是被服务器接收到了的, 只是响应被浏览器拦截了, 浏览器允许标签的跨域 
2. cors预请求(先发送一个options请求), 默认允许的方法包括 get post head

### Cache-Control
1.  可缓存 public(任何代理可缓存) private（发起请求的浏览器可缓存） no-cache（任何节点可在本地存储缓存，但需要到服务器验证才能使用缓存）
2.  到期 浏览器读取max-age=<seconds> 代理服务器读取s-maxage=<seconds> 发起端设置可使用过期缓存max-stale=<seconds>
3.  重新验证 must-revalidate proxy-revalidate 向服务器验证是否真正过期了
4.  no-store 本地和代理服务器不能存储缓存
5.  no-transform 禁止格式转换

### 资源验证
1.  Last-Modified 上次修改时间 If-Modified-since / If-Unmodified-Since
2.  Etag 数据签名 资源内容的hash计算 If-Match / If-Non-Match