大多数的 shell 都有自己的一套脚本语言，包括变量、控制流和自己的语法。而 shell scripting 与其他[[脚本语言]]的不同之处在于它只为  shell 环境（即 CLI ），对 shell 所从事的相关工作进行了优化，用于自动化系统管理任务。  

本节以最常见的bash脚本为讲述重点

---
# 第一个 Shell
```sh
#!/bin/bash
echo "HelloWorld！"
```
`#!` 告诉系统解释此脚本文件的 Shell 程序 `/bin/bash`，即指定使用的是哪种shell
`echo`在终端打印出内容

# 运行 Shell
## 作为可执行程序
