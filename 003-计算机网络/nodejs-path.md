```js
// Node.js 10.12+ 通用方案
import { fileURLToPath } from "node:url";
import path from "node:path";
const __filename = fileURLToPath(import.meta.url); //当前文件路径
const __dirname = path.dirname(__filename); //当前文件目录路径



```
