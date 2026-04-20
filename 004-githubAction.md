### 流程 （不过不是通过阅读文档，而是看别人怎么做的）

1.在文件目录中，生成一个 .github/workflow/action.yml文件（然后上传到github就会自己识别）

> > yml简单，确实DevOpt的灵魂文件

```yml
name: 尝试使用action
on: push #触发的事件

permissions: {}

jobs:
  job1: #并列执行
    runs-on: ubuntu-latest
    steps: #依次执行
      - run: pwd
      - run: ls
  job2:
    runs-on: windows-latest
    steps:
      - name: node-v
        run: node --version
```
