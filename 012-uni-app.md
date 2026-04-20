目录结构：
┌─uniCloud              云空间目录，支付宝小程序云为uniCloud-alipay，阿里云为uniCloud-aliyun，腾讯云为uniCloud-tcb（详见uniCloud）
│  ├─cloudfunctions
│  │  │─fun             由自己设置，以一个fun文件夹上传
│  │  └─common          系统默认，不能改
│  └─database           一般不用管
│─components            符合vue组件规范的uni-app组件目录
│  └─comp-a.vue         可复用的a组件
├─pages                 vue页面文件存放的目录
│  ├─index
│  │  └─index.vue       index页面
│  └─list
│     └─list.vue        list页面
├─static                存放应用引用的本地静态资源（如图片、视频等）的目录，注意：静态资源都应存放于此目录
├─uni_modules           存放uni_module 
├─platforms             存放各平台专用页面的目录，
├─nativeplugins         App原生语言插件 
├─nativeResources       App端原生资源目录
│  ├─android            Android原生资源目录 
|  └─ios                iOS原生资源目录 
├─hybrid                App端存放本地html文件的目录，
├─wxcomponents          存放微信小程序、QQ小程序组件的目录，
├─mycomponents          存放支付宝小程序组件的目录，
├─swancomponents        存放百度小程序组件的目录，
├─ttcomponents          存放抖音小程序、飞书小程序组件的目录，
├─kscomponents          存放快手小程序组件的目录，
├─jdcomponents          存放京东小程序组件的目录，
├─unpackage             非工程代码，一般存放运行或发行的编译结果
├─main.js               Vue初始化入口文件
├─App.vue               应用配置，用来配置App全局样式以及监听 应用生命周期
├─pages.json            配置页面路由、导航条、选项卡等页面类信息，（页面路由必须配）
├─manifest.json         配置应用名称、appid、logo、版本等打包信息，
├─AndroidManifest.xml   Android原生应用清单文件 
├─Info.plist            iOS原生应用配置文件 
└─uni.scss              内置的常用样式变量
	
其中重点且必须进行修改。
前端：pages,static,manifest.json,pages.json
后端：uniCloud-aliyun

前端
1.需要写好pages和配置后，需要点击发行就会打开小程序开发者工具，在里面点击上传。就会上传到小程序平台，记得时刻查看小程序后台的信息，会显示是否已上传，而询问是否审核
2.前端的页面是vuejs的写法

后端使用的是UniCloud的后端，提供两家服务商，
1.注册登录

2.在目录里右键点击关联服务空间

3.
fun：默认生成
├─index.js              必要文件的入口函数
├─config.json           系统设置
├─package.json          npm的库的设置，记得npm install
└─package-lock.json     系统设置，不用管
后面就可以在这个文件夹里随便写各种各样的js文件，只需要在index.js引入使用即可

4.每次修改完就上传部署

5.在uniCloud里设置这个fun文件夹的云函数的url地址，并且在小程序后台的开发管理的安全选项里添加uniCloud的request域名和这个云函数的url

6.然后就可以在前端的wx.request里直接请求这个url

7.这个在index.js里return就能把数据直接返回到前端，网络请求什么都不用写。

8.记得注意前端发送给后端的数据是什么对象（event.XX / event.body.XX），后端发送给前端是什么对象（res.data.XX / res.XX）

9.后端的写法是
``` javascript
//index.js
let db = uniCloud.database({
  throwOnNotFound: false,
})
const a = require("./a.js");
exports.main = async (event, context) => {
  try{
        let data = await a(event,context);
    return{
        data:data//你在这里的对象名是什么，返回给前端就是什么。这里return 便可以在返回到客户端。
    }
  }catch(e){
    console.log(e)
  }
}

//a.js //其实一旦配置好index.js，剩下的就自由编写函数了，毕竟这里exports是ES的模块化，async是异步函数
module.exports async (event,context) => {

} 
```

10.还有其他的uniCloud的函数，uni的函数，vue的框架：记住多查文档
```javascript
  //还有一些比如where查询特定条件的
  data = await db.collection("message").get();
  data = await db.collection("message").update();
  data = await db.collection("message").delete();
//
```

11.在前端，纵然是使用uni的语法，但是如果说我是微信小程序，那么微信小程序自带的函数我仍然可以使用