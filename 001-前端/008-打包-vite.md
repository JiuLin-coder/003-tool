```js
// 默认配置 (vite.config.js)
export default {
  build: {
    outDir: "dist", // 输出目录
    assetsDir: "assets", // 静态资源目录
    emptyOutDir: true, // 构建前清空目录
  },
};
```

为了让vite适配环境，（最终适配浏览器，不过这个是环境要考虑的事情），必须修改

1.这个base在不用环境的设置不同，
比如在github pages里 https://<USERNAME>.github.io/<REPO>/（例如你的仓库地址为 https://github.com/<USERNAME>/<REPO>），
那么请将 base 设置为 '/<REPO>/'

```js
// vite.config.js
import { defineConfig } from "vite";
import vue from "@vitejs/plugin-vue";

export default defineConfig({
  plugins: [vue()],
  base: "./", // 关键！改为相对路径，这里影响着打包后的文件的url地址，具体修改与你怎么通过前端url网络请求后拿到url再进行查找后端对应文件。
  build: {
    outDir: "dist",
    assetsDir: "assets",
    rollupOptions: {
      output: {
        assetFileNames: "assets/[name]-[hash][extname]", // 确保路径一致性
      },
    },
  },
});
```

---

下面这个做个参考

```js
import { resolve } from "path";
import { defineConfig } from "electron-vite";
import vue from "@vitejs/plugin-vue";
import vueDevTools from "vite-plugin-vue-devtools";
import AutoImport from "unplugin-auto-import/vite";
import Components from "unplugin-vue-components/vite";
import { TDesignResolver } from "@tdesign-vue-next/auto-import-resolver";
import wasm from "vite-plugin-wasm";
import { NaiveUiResolver } from "unplugin-vue-components/resolvers";
import topLevelAwait from "vite-plugin-top-level-await";
export default defineConfig({
  main: {
    resolve: {
      alias: {
        "@common": resolve("src/common"),
      },
    },
    build: {
      rollupOptions: {
        input: {
          index: resolve(__dirname, "src/main/index.ts"),
          downloadWorker: resolve(
            __dirname,
            "src/main/workers/downloadWorker.ts",
          ),
          pluginWorker: resolve(
            __dirname,
            "src/main/services/plugin/manager/pluginWorker.ts",
          ),
        },
        output: {
          entryFileNames: "[name].js",
          chunkFileNames: "script/[name]-[hash].js",
          assetFileNames(chunkInfo) {
            if (chunkInfo.names[0].endsWith(".css"))
              return "style/[name]-[hash].css";
            // 图片资源
            const imgReg = /\.(png|jpg|jpeg|gif|svg|webp)$/;
            if (imgReg.test(chunkInfo.names[0]))
              return "images/[name]-[hash].[ext]";
            // 其他资源
            return "assets/[name]-[hash].[ext]";
          },
        },
      },
      minify: "terser",
    },
  },
  preload: {
    resolve: {
      alias: {
        "@common": resolve("src/common"),
      },
    },
  },
  renderer: {
    build: {
      chunkSizeWarningLimit: 1000,
      minify: "terser",
      terserOptions: {
        compress: {
          drop_console: true,
          drop_debugger: true,
        },
      },
      rollupOptions: {
        output: {
          entryFileNames: "script/[name]-[hash].js",
          chunkFileNames: "script/[name]-[hash].js",
          assetFileNames(chunkInfo) {
            if (chunkInfo.names[0].endsWith(".css"))
              return "style/[name]-[hash].css";
            // 图片资源
            const imgReg = /\.(png|jpg|jpeg|gif|svg|webp)$/;
            if (imgReg.test(chunkInfo.names[0]))
              return "images/[name]-[hash].[ext]";
            // 其他资源
            return "assets/[name]-[hash].[ext]";
          },
        },
      },
    },
    plugins: [
      vue(),
      vueDevTools(),
      wasm(),
      topLevelAwait(),
      AutoImport({
        resolvers: [
          TDesignResolver({
            library: "vue-next",
          }),
        ],
        imports: [
          "vue",
          {
            "naive-ui": [
              "useDialog",
              "useMessage",
              "useNotification",
              "useLoadingBar",
            ],
          },
        ],
        dts: true,
      }),
      Components({
        resolvers: [
          TDesignResolver({
            library: "vue-next",
          }),
          NaiveUiResolver(),
        ],
        dts: true,
      }),
    ],
    base: "./",
    resolve: {
      alias: {
        "@renderer": resolve("src/renderer/src"),
        "@assets": resolve("src/renderer/src/assets"),
        "@components": resolve("src/renderer/src/components"),
        "@services": resolve("src/renderer/src/services"),
        "@types": resolve("src/renderer/src/types"),
        "@store": resolve("src/renderer/src/store"),
        "@common": resolve("src/common"),
      },
    },
  },
});
```
