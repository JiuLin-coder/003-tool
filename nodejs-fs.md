增删查

```javascript
import fs from "fs";

//文件，异步
//创建
fs.open("fileAddress", "a", (data) => {}); //第二个参数是模式，比如"w" "a"
//下面俩是没有该文件就创建文件再写入
fs.writeFile("fileAddress", "1", () => {}); //清空文件再写入第二个参数
fs.appendFile("fileAddress", "12", () => {}); //文件尾部写入第二个参数

//删
fs.unlink("a.js", () => {});

//查
fs.readFile("fileAddress", (err, data) => {
  console.log(data.toString());
}); //data 就是读取的内容

//改
fs.writeFile("fileAddress", "1", () => {}); //清空文件再写入第二个参数
fs.appendFile("fileAddress", "12", () => {}); //文件尾部写入第二个参数

//文件夹，异步
//创建
fs.mkdir("fileAddress", (err) => {});

//读取文件夹名字
fs.readdir("dirAddress", (err, files) => {});

//检查文件或目录是否存在
fs.access("fileAddress", fs.constants.F_OK, (err) => {});

//文件，同步
fs.writeFileSync("fileAddress", "13"); //清空文件再写入第二个参数
fs.appendFileSync("fileAddress", "13"); //清空文件再写入第二个参数
fs.unlinkSync("fileAddress");
//所有的同步都是函数名+Sync，最后一个参数（那个函数）去掉。
//我更多使用同步，因为我可以把直接let data = fs.readFileSync();同步的话若有输出则直接返回
```
