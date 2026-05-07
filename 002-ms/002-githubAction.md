### 流程 （不过不是通过阅读文档，而是看别人怎么做的）

1. 在文件目录中，生成一个 .github/workflows/action.yml文件（然后上传到github就会自己识别）

> > yml简单，确实DevOpt的灵魂文件

```yml
name: 尝试使用action
on: push #触发的事件

permissions: {}

jobs:
  job1: #并列执行,可自定义名字
    runs-on: ubuntu-latest
    steps: #依次执行
      - run: pwd #一个 "-" 为一个步骤
      - run: ls
  job2:
    runs-on: windows-latest
    steps:
      - name: node-v
        run: node --version
```

```yml
name: 打包vue项目
on: push

jobs:
  npm-build:
    name: npm-build工作
    runs-on: ubuntu-latest

    steps:
      - name: 检查仓库内容
        uses: actions/checkout@v4

      - name: 库安装
        run: npm install

      - name: 打包
        run: npm run build
```


2. 使用现成的action，通过use来引入，然后直接使用功能

3. 用户名和密码当然不想直接写在文件上：
  setting -> Secrets and variables -> action，可以在里面新增secret,里面就可以通过 自定义名称 来 替换我想隐藏的密码。
  写入好后，可以在github action的yml里${{}}来引用刚才保存的变量