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

# 注释
## 单行注释
使用 `#` 开头
```bash
# 这是一个胆寒注释
echo "Helloworld!"    # 这也是注释，Shell只执行 echo 命令
```
## 多行注释
Shell 没有直接支持多行注释的语法，但可以使用其他方法来实现
```bash
:<<EOF
注释内容
不会执行
     |\___/|
    (= o_o =)
    / >💻< \
   (___/ \___) 
EOF
```
`:` 是一个空命令，不会执行 `<<EOF` 之后的内容，`EOF`也可以换成其他标识符，但首尾需一致，所以也可以这样
```bash
: '
注释部分
     |\___/|
    (= o-o =)
    / >💻< \
   (___/ \___) 
两只小猫有一处不同
'
```

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

**注意**
 - `foo = bar` （使用空格隔开）是不能正确工作的，因为解释器会调用 `foo` 并将`=`和 `bar` 作为 参数
	在 Shell 脚本中使用空格会起到分割参数的作用，有时候可能会造成混淆，需要多加检查
 - Shell 中的字符串通过 `'` 和 `"` 分隔符来定义，但是含义不同
	 - 以 `'` 定义的字符串为字面字符串，其中的变量并不会被替换
	 - 以 `"` 定义的字符串会将其中包含的变量值进行替换
	```bash
	foo=bar
        echo "$foo"  # 输出 bar
        echo '$foo'  # 输出 $foo
        
        # 命令替换也会在双引号中执行
        echo "Today is $(date)" # 输出 Today is 07/16/2025 12:15:37
        echo 'Today is $(date)' # 输出 Today is $(date)
	```
### 查看已定义的变量
- `declare -p <varname>`: 显示变量属性和值
- `env`: 显示所有已导出的环境变量
- `set`: 显示所有Shell变量和函数 (输出多)

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
## 局部变量（函数内）
```bash
local my_var="value"
```

## 删除变量
```bash
unset <变量名>
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
my_array=("1" 2 3 4)    # 整数数组
# 数字下标定义数组
array_name[0]=value0
array_name[1]=value1
array_name[2]=value2
```

```bash
declare -A associative_array
associative_array["name"]="Astrid"
associative_array["age"]=19
```
#### 读取数组
##### 整数索引数组
```bash
${array_name[index]}    # 一般格式
```

```bash
# 实例
echo "第一个元素为: ${my_array[0]}"  
echo "第二个元素为: ${my_array[1]}"  
echo "第三个元素为: ${my_array[2]}"  
echo "第四个元素为: ${my_array[3]}"
# 运行结果
第一个元素为: 1
第二个元素为: 2
第三个元素为: 3
第四个元素为: 4
```
##### 关联数组
```bash
array_name["index"]
```
##### 获取数组中的所有元素
使用 @ 或 * 可以获取数组(两种数组都可以)中的所有元素
```bash
echo "数组的元素为: ${my_array[*]}"  
echo "数组的元素为: ${my_array[@]}"
```
在数组名前加一个感叹号 ! 可以获取数组的所有键
```bash
echo "数组的键为: ${!site[*]}"  
echo "数组的键为: ${!site[@]}"
```
##### 获取数组的长度
在·数组名前加一个 # 可以获取数组的长度
```bash
echo "数组元素个数为: ${#my_array[*]}"  
echo "数组元素个数为: ${#my_array[@]}"
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

#### 在脚本中使用默认值
有些环境变量不一定总存在，这时可设默认值：
```bash
#!/bin/bash
echo "PORT=${PORT:-3000}"   # 如果 PORT 未定义，默认是 3000
```
还可以直接赋值（不修改原变量）：
```bash
name=${USER_NAME:-"anonymous"}
echo "Hello $name"
```

### 特殊变量
- `$0`: 脚本名称
- `$1` 至 `$9`: 参数 (超过9个用 `${10}`)
- `$@`: 所有参数，每个为独立字符串
- `$*`: 所有参数，视为单个字符串
- `$#`: 参数数量
- `$?`: 上一个命令的返回码 (0为成功)
- `$$`: 当前脚本的PID
- `!!`: 上一条完整命令
- `$_`: 上一个命令的最后一个参数

# 运算符
[Shell 基本运算符 ](https://www.runoob.com/linux/linux-shell-basic-operators.html
Shell 和其他编程语言一样，支持多种运算符，包括：
- 算数运算符
- 关系运算符
- 布尔运算符
- 字符串运算符
- 文件测试运算符
原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用。
## 算术运算符
**注意**：
 - 使用的是反引号 $`$ 而不是单引号 $'$
 - 条件表达式要放在方括号之间，并且要有空格
 - 乘号(*)前边必须加反斜杠(\)才能实现乘法运算
```bash
# 实例
a=10  
b=20  
  
val=`expr $a + $b`  
echo "a + b : $val"  
  
val=`expr $a - $b`  
echo "a - b : $val"  
  
val=`expr $a \* $b`  
echo "a * b : $val"  
  
val=`expr $b / $a`  
echo "b / a : $val"  
  
val=`expr $b % $a`  
echo "b % a : $val"  
  
if [ $a == $b ]  
then  
   echo "a 等于 b"  
fi  
if [ $a != $b ]  
then  
   echo "a 不等于 b"  
fi

# 结果
a + b : 30
a - b : -10
a * b : 200
b / a : 2
b % a : 0
a 不等于 b
```

## 关系运算符
关系运算符只支持数字，不支持字符串，除非字符串的值是数字

| 运算符 | 说明                           |
| --- | ---------------------------- |
| -eq | 检测两个数是否相等，相等返回 true          |
| -ne | 检测两个数是否不相等，不相等返回 true        |
| -gt | 检测左边的数是否大于右边的，如果是，则返回 true   |
| -lt | 检测左边的数是否小于右边的，如果是，则返回 true   |
| -ge | 检测左边的数是否大于等于右边的，如果是，则返回 true |
| -le | 检测左边的数是否小于等于右边的，如果是，则返回 true |

## 布尔运算符

| 运算符 | 说明                                |
| --- | --------------------------------- |
| ！   | 非运算，表达式为 true 则返回 false，否则返回 true |
| -o  | 或运算，有一个表达式为 true 则返回 true         |
| -a  | 与运算，两个表达式都为 true 才返回 true         |

## 逻辑运算符
`&&` 逻辑的 AND
`||` 逻辑的 OR

## 字符串运算符

| 运算符 | 说明                         |
| --- | -------------------------- |
| =   | 检测两个字符串是否相等，相等返回 true      |
| !=  | 检测两个字符串是否不相等，不相等返回 true    |
| -z  | 检测字符串长度是否为0，为0返回 true      |
| -n  | 检测字符串长度是否不为 0，不为 0 返回 true |
| $   | 检测字符串是否不为空，不为空返回 true      |

## 文件测试运算符
| 操作符     | 说明                                      |
| ------- | --------------------------------------- |
| -b file | 检测文件是否是块设备文件，如果是，则返回 true               |
| -c file | 检测文件是否是字符设备文件，如果是，则返回 true              |
| -d file | 检测文件是否是目录，如果是，则返回 true                  |
| -f file | 检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true |
| -g file | 检测文件是否设置了 SGID 位，如果是，则返回 true           |
| -k file | 检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true   |
| -p file | 检测文件是否是有名管道，如果是，则返回 true                |
| -u file | 检测文件是否设置了 SUID 位，如果是，则返回 true           |
| -r file | 检测文件是否可读，如果是，则返回 true                   |
| -w file | 检测文件是否可写，如果是，则返回 true                   |
| -x file | 检测文件是否可执行，如果是，则返回 true                  |
| -s file | 检测文件是否为空（文件大小是否大于0），不为空返回 true          |
| -e file | 检测文件（包括目录）是否存在，如果是，则返回 true             |

## 自增自减操作符
Shell 本身没有类似 ++ 和 -- 的操作符，但可以通过其他方式实现相同的功能
```bash
# 实例
# 初始化变量  
num=5  
  
echo "初始值: $num"  
  
# 自增  
let num++  
echo "自增后: $num"  
  
# 自减  
let num--  
echo "自减后: $num"  
  
# 使用 $(( ))  
num=$((num + 1))  
echo "使用 $(( )) 自增后: $num"  
  
num=$((num - 1))  
echo "使用 $(( )) 自减后: $num"  
  
# 使用 expr  
num=$(expr $num + 1)  
echo "使用 expr 自增后: $num"  
  
num=$(expr $num - 1)  
echo "使用 expr 自减后: $num"  
  
# 使用 (( ))  
((num++))  
echo "使用 (( )) 自增后: $num"  
  
((num--))  
echo "使用 (( )) 自减后: $num"
```

# printf 命令
**实际应用示例：**

创建表格输出
```bash
#!/bin/bash
# 表头
printf "%-15s %10s %10s %10s\n" "Item" "Quantity" "Price" "Total"
# 分隔线
printf "%-15s %10s %10s %10s\n" "---------------" "----------" "----------" "----------"
# 数据行
printf "%-15s %10d %10.2f %10.2f\n" "Notebook" 3 2.50 7.50
printf "%-15s %10d %10.2f %10.2f\n" "Pen" 5 1.20 6.00
printf "%-15s %10d %10.2f %10.2f\n" "Eraser" 2 0.50 1.00
# 总计行
printf "%-15s %10s %10s %10.2f\n" "" "" "Total:" 14.50
```
进度条实现
```bash
#!/bin/bash
for i in {1..20}; do
    printf "\rProgress: [%-20s] %d%%" $(printf "%${i}s" | tr ' ' '#') $((i*5))
    sleep 0.1
done
printf "\n"
```
颜色输出
```bash
#!/bin/bash
RED='\033[0;31m'
GREEN='\033[0;32m'
NC='\033[0m' # No Color
printf "${RED}Error:${NC} Something went wrong\n"
printf "${GREEN}Success:${NC} Operation completed\n"
```

`%-10s`指一个宽度为10个字符（`-` 表示左对齐，没有表示右对齐）
# 控制流
bash 支持**控制流**技术，包括 `if`、`case`、`while` 和 `for`。类似地，bash 有接收参数并能对其进行操作的**函数**。
#### if else
##### if 
```bash
if condition
then
    command1 
    command2
    ...
    commandN 
fi
```
##### if else
```bash
if condition
then
    command1 
    command2
    ...
    commandN
else
    command
fi
```
##### if else-if else
```bash
if condition1
then
    command1
elif condition2 
then 
    command2
else
    commandN
fi
```

#### for 循环
```bash
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```

#### while 语句
```bash
while condition
do
    command
done
```

#### until 循环
 循环执行一系列命令直至条件为 true 时停止，与 while 循环在处理方式上刚好相反
```bash
until condition
do
    command
done
```

#### case ··· esac
多选择语句，与其他语言中的 switch ... case 语句类似，是一种多分支选择结构，每个 case 分支用右圆括号开始，用两个分号 ;; 表示 break，即执行结束，跳出整个 case ... esac 语句，esac（就是 case 反过来）作为结束标记，可以用 case 语句匹配一个值与一个模式，如果匹配成功，执行相匹配的命令
```bash
case 值 in
模式1)
    command1
    command2
    ...
    commandN
    ;;
模式2)
    command1
    command2
    ...
    commandN
    ;;
esac
```
取值后面必须为单词 **in**，每一模式必须以右括号结束。取值可以为变量或常数，匹配发现取值符合某一模式后，其间所有命令开始执行直至 ;;
取值将检测匹配的每一个模式。一旦模式匹配，则执行完匹配模式相应命令后不再继续其他模式。如果无一匹配模式，使用星号 * 捕获该值，再执行后面的命令
```bash
# 实例
echo '输入 1 到 4 之间的数字:'
echo '你输入的数字为:'
read aNum
case $aNum in
    1)  echo '你选择了 1'
    ;;
    2)  echo '你选择了 2'
    ;;
    3)  echo '你选择了 3'
    ;;
    4)  echo '你选择了 4'
    ;;
    *)  echo '你没有输入 1 到 4 之间的数字'
    ;;
esac
```

#### 跳出循环
还是老朋友 break（终止所有循环） 和 continue（跳出当前循环）
# Shell Globbing
启动脚本时，通常会希望提供相似的参数。Bash有简化此操作的方法，通过执行**文件名展开**来展开表达式。这些技术通常被称为**Shell Globbing**。
## 通配符 (Wildcards)
当想要执行某种通配符匹配时，可以使用：
- `?` 匹配单个字符
- `*` 匹配任意数量的字符
例如，给定文件 foo、foo1、foo2、foo10 和 bar：
```bash
# 删除 foo1 和 foo2（匹配单个字符）
rm foo?

# 删除所有以 foo 开头的文件（除了 bar）
rm foo*
```
## 花括号展开 {}
当一系列命令中有共同的子串时，可以使用花括号让bash自动展开：
```bash
# 将展开为 convert image.png image.jpg
convert image.{png,jpg}

# 将展开为三个文件的复制
cp /path/to/project/{foo,bar,baz}.sh /newpath

# Globbing技术可以组合使用
mv *.{py,sh} folder

# 序列展开
# 创建 foo/a, foo/b, ..., foo/h, bar/a, bar/b, ..., bar/h
mkdir foo bar
touch {foo,bar}/{a..h}

# 显示两个目录文件的差异
touch foo/x bar/y
diff <(ls foo) <(ls bar)
```

# 命令输出复用
Shell脚本中常见的需求是将命令的输出作为变量保存或用于其他命令的参数
## 命令替换 (Command Substitution)
使用 `$( CMD )` 语法将命令的输出捕获并替换：
```bash
# 捕获日期命令的输出
current_date=$(date)
echo "Today is $current_date"

# 在for循环中使用命令输出（虽然不推荐用于文件名）
for file in $(ls); do
    echo "Found file: $file"
done

# 推荐的现代语法是 $(command)
# 旧语法是反引号 `command`，但嵌套使用困难且有解析问题
```
## 进程替换 (Process Substitution)
`<( CMD )` 会执行CMD并将其输出放在一个临时文件中，并将命令替换为该文件的名称：
```bash
# 比较两个目录的内容
diff <(ls dir1) <(ls dir2)

# 同时处理两个命令的输出
comm <(sort file1) <(sort file2)

# 将多个输出合并为一个输入
cat <(grep 'error' logfile) <(grep 'warning' logfile) > filtered_logs.txt
```
这在命令期望通过文件而不是标准输入传递值时非常有用
## 逻辑操作符
退出码可用于条件性执行命令，使用 `&&` (与操作符) 和 `||` (或操作符)：
```bash
# 仅当命令成功时运行后续命令
mkdir /tmp/test && cd /tmp/test

# 当命令失败时提供后备命令
grep 'pattern' file.txt || echo "Pattern not found"

# 命令可以用分号分隔，无条件执行
echo "First"; echo "Second"; echo "Third"

# 实际例子
false || echo "Oops, fail"  # 输出: Oops, fail
true || echo "Won't be printed"  # 无输出
true && echo "Things went well"  # 输出: Things went well
false && echo "Won't be printed"  # 无输出
```
