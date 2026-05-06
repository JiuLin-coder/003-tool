创建 HTTP 服务器

```javascript
import http from "http";

function server(req, res) {
  // 获取请求方法、URL 和请求头
  const { method, url, headers } = req;

  //书写响应数据请求头
  res.writeHead(200, { "Content-Type": "application/json" }); // 设置响应头//Content-Type必须与前端协商好，不然待会发错类型就报错了
  res.end("{}"); // 结束响应并发送响应数据
}

http.createServer(server).listen(3000);
console.log("服务启动，地址：http://localhost:3000/");
```

1. import http - 导入 Node.js 的 http 模块
2. http.createServer() - 创建一个 HTTP 服务
3. 回调函数 function name (req, res){...} - 处理每个 HTTP 请求，这个服务的所有逻辑交给server函数出处理，请求来的数据和返回的函数会传入server函数
   1. req - 请求对象，包含客户端发来的请求信息
   2. res - 响应对象，用于向客户端发送响应
4. server.listen() - 启动服务监听指定端口,这个函数比较特殊，它不是一次性的，而是listen监听。一直会接收发到3000端口的网络请求。

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
res.writeHead(statusCode, headers); //设置响应状态码（约定俗成）和响应头
res.write(data); //写入响应体,可忽略
res.end([data]); //结束响应，可选发送最后的数据
res.setHeader(name, value); //设置响应头，可忽略
```

---

其他

1. res,req 只能放在请求函数（如这里的server）里，而其他东西就随意，不过遵循自上而下和同步或异步。

```javascript
const server = http.createServer((req, res) => {
  //若是监听到，相当于把这里的代码给执行一次，可以写任何代码，不过req和res的函数必不可少
  //若是请求时没有提供数据则req的所有数据和函数都不能用
});

server.listen(2000);
```

2. 根据响应文件类型，动态修改响应头类型

```js
function server(req, res) {
  const filePath = path.join(__dirname, req.url);
  const ext = path.extname(filePath); //从文件路径中提取文件扩展名

  // 根据扩展名设置Content-Type
  const mimeTypes = {
    ".html": "text/html",
    ".css": "text/css",
    ".js": "application/javascript",
    ".json": "application/json",
  };

  const contentType = mimeTypes[ext] || "application/octet-stream";

  res.writeHead(200, { "Content-Type": contentType });

  let data = fs.readFileSync(filePath);
  res.end(data); //把整个文件发过去
}
```

3. 请求体处理（或者你直接写在url里也是一种选择）

```js
function server(req, res) {
  if (method === "GET") {
  } else if (method === "POST") {
    let body = "";

    // 收集 POST 数据
    req.on("data", (chunk) => {
      body += chunk;
    });

    // 数据接收完毕
    req.on("end", () => {
      console.log("POST 数据:", body);

      res.writeHead(200, { "Content-Type": "text/plain" });
      res.end("数据已接收\n"); //res.end()必须在这里面
    });
  }
}
```

4. 浏览器的同源限制处理
   前端的fetch则要mode:"cors"

```js
res.writeHead(200, {
  "Content-Type": "application/json",
  "Access-Control-Allow-Methods": "*",
  "Access-Control-Allow-Origin": "*",
});
```

---
