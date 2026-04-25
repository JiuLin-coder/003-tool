创建 HTTP 服务器

```javascript
const http = require("http");

// 创建 HTTP 服务器
const server = http.createServer((req, res) => {
  // 设置响应头
  res.writeHead(200, { "Content-Type": "text/plain" }); //Content-Type必须与前端协商好，不然待会发错类型就报错了

  // 发送响应数据
  res.end("Hello, World!\n");
});

// 监听 3000 端口
server.listen(3000, () => {
  console.log("Server running at http://localhost:3000/");
});
```

1. require('http') - 导入 Node.js 的 http 模块
2. http.createServer() - 创建一个 HTTP 服务器实例
3. 回调函数 (req, res) => {...} - 处理每个 HTTP 请求
   1. req - 请求对象，包含客户端发来的请求信息
   2. res - 响应对象，用于向客户端发送响应
4. res.writeHead() - 设置响应头
5. res.end() - 结束响应并发送数据
6. server.listen() - 启动服务器监听指定端口

**（记得在操作系统使用这个文件,listen()比较特殊，使用后这个文件listen会持续监听该端口，不会使用后释放整个文件）**

---

处理 HTTP 请求：个人是感觉，就是依靠req获取数据，res响应数据

下面是提供的一些特殊的数据和函数

```javascript
//请求对象 (req) 常用属性
req.method; //HTTP 请求方法 (GET, POST, PUT, DELETE 等)
req.url; //请求的 URL 路径
req.headers; //请求头对象
req.httpVersion; //HTTP 协议版本
//响应对象 (res) 常用方法
res.writeHead(statusCode, headers); //设置响应状态码（约定俗成）和头
res.write(data); //写入响应体
res.end([data]); //结束响应，可选发送最后的数据
res.setHeader(name, value); //设置响应头
```

处理HTTP请求的数据

```javascript
const http = require("http");

const server = http.createServer((req, res) => {
  // 获取请求方法、URL 和请求头
  const { method, url, headers } = req;

  // 记录请求信息
  console.log(`收到 ${method} 请求，路径: ${url}`);
  console.log("请求头:", headers);

  // 根据请求方法处理
  if (method === "GET") {
    res.writeHead(200, { "Content-Type": "text/plain" });
    res.end("这是一个 GET 请求\n");
  } else if (method === "POST") {
    ///////////////////////////////////////////////////////////////////

    let body = "";

    // 收集 POST 数据
    req.on("data", (chunk) => {
      body += chunk;
    });

    // 数据接收完毕
    req.on("end", () => {
      console.log("POST 数据:", body);
      res.writeHead(200, { "Content-Type": "text/plain" });
      res.end("数据已接收\n");
    });

    ///////////////////////////////////////////////////////////////////
  } else {
    res.writeHead(405, { "Content-Type": "text/plain" });
    res.end("方法不允许\n");
  }
});

server.listen(3000);
```

---

其他

```javascript
const server = http.createServer((req, res) => {
  //若是监听到，相当于把这里的代码给执行一次，可以写任何代码，不过req和res的函数必不可少
  //若是请求时没有提供数据则req的所有数据和函数都不能用
});

server.listen(2000);
```
