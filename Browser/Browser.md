# Browser

## 实现原理

浏览器的功能，可以理解为把一个 URL 变为屏幕上显示的网页。
从 server 返回的数据并非想象的一步做完才能接着下一步，而是一条流水线，即不需要前一步完全完成，下一步就开始处理上一步的输出，这也是我们浏览页面时页面逐步出现的原因。

### HTTP 协议

![http](./images/http.jpg)

#### HTTP 方法

- GET
- POST
- HEAD
- PUT
- DELETE
- CONNECT
- OPTIONS
- TRACE

#### HTTP Status Code 和 Status Text

- 1xx: 临时回应，表示客户端请继续。一般浏览器 http 库会直接处理，所以平时见不到
- 2xx: 请求成功
  - 200: 请求成功
- 3xx: 请求目标资源有变化
  - 301: 目标资源永久性转移
  - 302: 目标资源临时性转移
  - 304: 客户端缓存没有更新
- 4xx: 客户端请求错误
  - 403: 无权限
  - 404: 页面不存在
- 5xx: 服务器请求错误
  - 500: 服务器端错误
  - 503: 服务器端暂时错误，可以一会再试

[在线模拟 HTTP 状态码](https://httpstat.us/)

#### HTTP Head

#### HTTP Request Body

### 解析

### 构建 DOM

### 计算 CSS

### 渲染/合成/绘制

## API

### DOM

### CSSOM

### 事件

### API 总集合
