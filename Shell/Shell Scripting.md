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
#### 作为可执行程序
将要执行的语句保存为 .sh 文件，并 cd 到相应的目录
```sh
chmod +x ./<文件名>.sh    # 使脚本具有执行权限
./<文件名>.sh    # 执行脚本
```
**一定要在所要执行的文件名前加 `./` 已告诉系统在该文件目录下寻找，否则系统会去 `PATH` 里寻找该文件，而通常当前目录不在 `PATH` 里，就会导致找不到**

#### 作为解释器参数
直接运行解释器，并将所要执行的 Shell 脚本文件名作为参数
```sh
/bin/sh <文件名>.sh
/bin/php <文件名>.php
```
这种方式运行时，哪怕在第一行指定了解释器，也还是会用直接运行的解释器执行（所以可以不指定）

# Shell 变量
## 定义
定义变量时，变量名不加 $ 符号（PHP语言需要）
```bash
my_name="canvas"
```
**变量名和等号之间绝对不能有空格**，同时，变量名的命名须遵循以下规则：
- 只包含字母、数字和下划线： 变量名可以包含字母（大小写敏感）、数字和下划线 _，不能包含其他特殊字符。
- 不能以数字开头： 变量名不能以数字开头，但可以包含数字。
- 避免使用 Shell 关键字： 不要使用Shell的关键字（例如 if、then、else、fi、for、while 等）作为变量名，以免引起混淆。
- 使用大写字母表示常量。
- 避免使用特殊符号： 尽量避免在变量名中使用特殊符号，因为它们可能与 Shell 的语法产生冲突。
- 避免使用空格： 变量名中不应该包含空格，因为空格通常用于分隔命令和参数。

除了显式地直接复制赋值，还可以通用语句给变量赋值
```bash
for file in 'ls /etc'
for file in $(ls /etc)
# 以上语句将 /etc 下目录的文件名循环出来
```

## 使用
使用变量时，在变量前加 $ 即可：
```bash
my_name="canvas"
echo $my_name
echo ${my_name}
```
花括号为可选项，以便解释器识别变量的边界
```bash
for skill in Ada Coffe Action Java; do
    echo "I am good at ${skill}Script"
done
# 如果不加，会导致解释器认为 $skillscript 为一个变量，而 skillscript未定义
```
重复定义时，第二次赋值时变量前不能加 $ 
```bash
my_name="canvas"
echo $my_name
my_name="astrid"
echo $my_name
```

## 只读变量
使用 readonly 命令可以将变量定义为只读变量，只读变量的值不能被改变，强行更改会报错
```bash
#!/bin/bash
myUrl="https://www.google.com"  
readonly myUrl
```

## 删除变量
```bash
uset <变量名>
```
变量被删除后不能再次使用，unset 命令不能删除只读变量

## 变量类型
以下为一些主要的类型
### 字符串变量
在 Shell 中，变量通常被视为字符串，可以用单引号 `'` 或双引号 `"` 来定义
```bash
my_string='Helloworld!'
my_string="Helloworld!"
```
### 整数变量
可以使用 declare 或 typeset 命令来声明整数变量，这样的变量只包含整数值并且这样的声明告诉 Shell 将 my_integer 视为整数，如将非整数赋值给它，将会尝试将其转换为整数
```bash
declare -i my_integer=17
```
### 数组变量
数组可以是整数索引数组或关联数组
```bash
my_array=(1 2 3 4 5)    # 整数数组
```

```bash
declare -A associative_array
associative_array["name"]="Astrid"
associative_array["age"]=19
```
### 环境变量
由操作系统或用户设置的特殊变量，用于配置  Shell 的行为和影响其执行环境
#### 查看环境变量
```bash
#!/bin/bash
echo "当前用户是：$USER"
echo "当前目录是：$PWD"
```

#### 设置环境变量
```bash
export var2="world"  # 环境变量（会传递给子进程）
```
#### 传递环境变量
```bash
#!/bin/bash
export MY_VAR="FromScript"
./child_script.sh
```
child_script.sh
```bash
#!/bin/bash
echo "MY_VAR is: $MY_VAR"    # 能读取上一级脚本用 export 传下来的变量
```

#### 配置文件
把脚本中的 **配置和逻辑分离**，使脚本更清晰、更易维护、更易复用。
```bash
# env.sh
export SERVER_PORT=8080
export DB_URL="localhost"

# run.sh
#!/bin/bash
source ./env.sh
echo "连接数据库：$DB_URL:$SERVER_PORT"
```
%% 
`source` 命令的本质是在当前 shell 中运行另一个脚本时，不启动子进程
```bash
# 错误方式
./env.sh   # 会启动子shell，变量定义对当前脚本无效
echo $DB_URL  # 空的

# 正确方式
source ./env.sh
echo $DB_URL  # 有效
```
%%

### 特殊变量

