一个基本的 fetch 请求设置起来很简单。

```javascript
fetch("http://example.com/movies.json")
  .then((response) => response.json())
  .then((data) => console.log(data));
```

response里只是http头，需要response.json()得到JSON内容。
response.json()也是个promise，需要再then()或await一次。

---

当然就这些还不够，还需要配置关于http协议请求的，虽然fetch有默认配置，但是默认的是get

POST请求，请求时并把data提供给服务器，服务器接收到后返回请求。最基本的POST的协议配置（第二个参数），只需要两个method和body。

```javascript
fetch("http://example.com/movies.json", {
  method: "POST", // *GET, POST, PUT, DELETE, etc.//用于向服务器说明
  body: JOSN.stringify(data), //data就是向服务器发送的内容，data与headers的Content-Type里的类型一致，默认是json
})
  .then((response) => response.json())
  .then((data) => console.log(data));
```

---

一些协议配置，还有一些没列出来，不过主要的就是前五个，其他我目前不需要  
弄这个纯粹是我有些恐惧未知，毕竟使用这个网络通信简单过头了。

```javascript
fetch("http://example.com/movies.json", {
  method: "POST", // *GET, POST, PUT, DELETE, etc.//用于向服务器说明，指定 HTTP 请求方法
  body: JOSN.stringify(data), //data就是向服务器发送的json内容，data与headers的Content-Type里的类型一致，默认是json请求体，GET,HEAD不能加
  headers: { "Content-Type": "application/json" }, //设置请求头（键值对）,里面我增加的这个参数是请求体的数据类型
  mode: "cors", // no-cors, *cors, same-origin //设置能否跨域，
  credentials: "same-origin", // include, *same-origin, omit      //控制cookie能否跨域 'include'（跨域携带 Cookie）、'same-origin'（同源携带）
  cache: "no-cache", // *default, no-cache, reload, force-cache, only-if-cached //控制 HTTP 缓存行为
  redirect: "follow", // manual, *follow, error  //处理重定向策略
  referrerPolicy: "no-referrer", // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url     //设置来源页面 URL
});
```

---

上传文件

```javascript
let init = { method: "PUT", body: formData }; //formData这个有点复杂，与浏览器获得文件API有关，
```

---

判断是否请求成功
因为fetch 不会自动将 HTTP 错误状态（如 404 或 500）作为拒绝catch的 Promise，仍然需要检查响应状态。

```javascript
fetch("https://api.example.com/data")
  .then((response) => {
    if (response.ok) {
      //请求成功判断
      return response.json();
    }
    throw new Error("Something went wrong");
  })
  .then((data) => console.log(data))
  .catch((error) => console.error("Error:", error));
```

---
