1. jwt
```javascript
let jwt = require("jsonwebtoken");
let jwtSecret = "你的jwt密码";

let token = "Bearer " + jwt.sign({ userId }, jwtSecret); //加密
//剩下还有给用户表添加用户，或者返回用户数据

let auth = jwt.verify(event.token.replace("Bearer ", ""), jwtSecret); //解密
let userId = auth.userId;
//若是成功,则返回userId
```

2. 这里面的函数就可以直接使用，
![第三方微信的登录](./static/013-小程序获取openId流程.jpg)

3. 正如图里的api请求一样，我们还需要网络请求函数（uni,给打包好了，直接调用函数，输入参数即可）


