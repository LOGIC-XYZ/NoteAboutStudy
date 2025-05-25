# 控制台、终端、命令行（CLI）和Shell
在**终端**（或控制台）的**命令行界面**中输入命令，然后**Shell**解释并执行这些命令，并将结果显示在终端中。

## 控制台
console 最初指物理设备，包括键盘、鼠标与显示器，而用户通过控制台与计算机**进行交互**。
在现代计算机中，尤其是在服务器领域，物理控制台仍然存在。但在个人电脑上，"控制台"的概念更多地被“终端”软件所取代，或特指直接连接到计算机的“主”终端。

## 终端
Terminal 是一个进行文本交互的**程序**，它提供了一个输入输出环境（窗口），可以在其中输入命令并查看输出。
作为一个程序，可以随意安装卸载，可以安装多个，并运行想运行的，而所有操作系统都安装了一个默认的终端：win 的 Windows Terminal， mac 的 Terminal 应用，而 Linux 发行版不同默认的终端也不同）。

## 命令行
Command Line Interface 是一种通过文本命令与计算机系统交互的用户界面。
大多数操作系统都有两种不同类型的界面， CLI 与 GUI （图形用户界面），前者通过**键入命令**执行任务，后者通过**图标、菜单和点击**执行任务。
终端或者说控制台提供了 CLI 的环境，而 CLI 是与计算机进行文本交互的通用概念。 Shell 是 CLI 的一个具体实现。

##  Shell
Shell 是是位于用户和操作系统内核之间的一层软件。它是一个**命令解释器**，负责解析用户输入的命令（通过 CLI ），并将其传递给操作系统内核执行。Shell 还负责处理命令的输出，并将其显示给用户。
与 Terminal 一样， Shell 是所有操作系统中的默认程序，也可以自主安装卸载，可以安装多个，并运行想运行的。而不同的 Shell 有不同的语法和特点，不同的操作系统可能默认安装的 Shell 也不同：win 的 PowerShell 还有 cmd.exe ， mac 的 Zsh 或者 Bash （旧版），而大多数 Linux 发行版用的是 Bash。
Shell 是真正“理解”并“执行”你输入的命令的程序，它提供了多种功能。
Shell 脚本语言是一种解释型语言，可将一系列命令（ CLI 中输入的）写入一个文本文件中，然后让 Shell 程序按照脚本内容执行，这就是 [[Shell 脚本]]。

# Using the shell
## 理解提示符（Prompt）
当启动终端时，会看到大致如下提示符：
```sh
missing@MiZoRe:~$
```
 - 通常格式是`username@hostname:current_director$`
 - `~`代表用户主目录
 - `$`代表普通用户，`#`代表 root 用户

## 引号的使用
 - **单引号（`'`）**：强引用，所有字符都**字面处理**，被当作普通文本。
 - **双引号（`"`）**：弱引用，变量/命令替换会被**解释**，算术会扩展为结果。
 - **反斜杠（`\`）**：用于**转义紧随其后的单个字符**（无论是否在引号内），使其失去特殊含义，被当作字面字符处理。

## $PATH 环境变量
### 1. 当 Shell 执行某个指令，但是该指令并不是 Shell 所了解的编程关键字时，它会去查询**环境变量 `$PATH`**。`$PATH` 列出了当 Shell 接到某条指令时，进行程序搜索的路径，这些路径由 `:` 分割。
```sh
missing:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
missing:~$ which echo
/bin/echo
missing:~$ /bin/echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```
1. echo $PATH -- 查询 $PATH 环境变量中包含哪些目录。
2. which echo -- 在 $PATH 环境变量中查找指定的程序。并输出第一个匹配程序的完整路径。
3. /bin/echo $PATH -- 直接指定程序的完整路径来绕过 $PATH 的搜索机制，从而精确控制要执行的程序版本或位置。
---
### 2. 为什么需要绕过`$PATH`？
- **执行当前目录的脚本或程序：** 出于安全考虑，当前目录通常不在 `$PATH` 中。因此，如果想执行当前目录下的脚本或程序，就必须使用 `./` 前缀。
- **执行特定版本的程序：** 如果系统上安装了同一个程序的多个版本（例如，不同的 Python 版本），可以通过指定完整路径来确保执行的是想要执行的版本。
- **执行未添加到 `$PATH` 的程序：** 有些程序可能不会自动将其可执行文件添加到 `$PATH` 中的目录，此时就需要手动指定它们的路径来运行。
---
- 临时添加: `export PATH="/new/path:$PATH"`
- 永久修改: 编辑 `~/.bashrc` 或 `~/.zshrc`
### 3. 其他特殊==变量==
- `$?`: 显示上一个命令的退出状态（0 表示成功，非 0 表示失败）。
- `$1`, `$2`, ... , `$9`: 用于引用脚本或函数中的参数。
- `$0`: 脚本名。
- `$$`: 显示当前进程的PID（进程ID）。
- `$@`: 所有参数。
- `$#`: 参数个数。
- `!!`: 完整的上一条命令，包括参数（当你因为权限不足执行命令失败时，可以使用 `sudo !!` 再尝试一次）。

## `type`命令
`type` 命令比 `which` 命令**更全面**，因为它不仅仅查找可执行文件的路径，还会说明 Shell 如何解析和执行提供的命令，包括它是否是一个**别名、内建命令、函数或外部文件**。
- **语法：** `type [options] name [name ...]`
- **作用：** 显示 Shell 如何解释和查找指定的命令。
---
`type` 命令的输出类型
- **alias (别名)：** 用户为另一个命令或命令序列创建的快捷方式。
- **builtin (内建命令)：** Shell 自身提供的命令，不需要启动外部程序。这些命令直接由 Shell 解释执行，通常运行速度更快。
- **function (函数)：** 用户在 Shell 配置文件或脚本中定义的 Shell 函数。
- **file (文件/外部命令)：** 通过 `$PATH` 找到的可执行文件（二进制程序或脚本）。这是 `which` 命令主要查找的类型。
- **keyword (关键字)：** Shell 语言中的保留字，例如 `if`, `for`, `while` 等。

## `echo`命令
用于输出字符串。Shell 会基于空格分割命令并解析，然后执行第一个单词代表的程序，并将后续单词作为参数。如果参数中包含空格，需要使用单引号 (`'`)、双引号 (`"`) 包裹，或使用反斜杠 (`\`) 进行转义。
```sh
echo "Hello world" # 输出 Hello world
echo Hello\ world  # 输出 Hello world
```

# Navigating in the shell
shell 中的路径是一组被分割的目录，在 Linux 和 macOS 上使用 `/` 分割，而在 Windows 上是 `\`。
以`/`（或Windows盘符如`C:\`）开头的为*绝对路径*，否则为*相对路径*。
在路径中，`.` 表示的是当前目录，而 `..` 表示上级目录。
## `cd`
```sh
missing:~$ pwd                           # 显示当前工作目录
/home/missing
missing:~$ cd /home                      # 切换目录
missing:/home$ pwd
/home
missing:/home$ cd ..                     # 切换到上级目录
missing:/$ pwd
/
missing:/$ cd ./home
missing:/home$ pwd
/home
missing:/home$ cd missing                # cd ## 切换到主目录（~）
missing:~$ pwd
/home/missing
missing:~$ ../../bin/echo hello
hello
```
## `ls`
```sh
ls               # 列出当前目录内容
ls /home         # 列出 /home 目录内容
ls --help        # 查看 ls 命令的帮助
```
### `ls -l`
```sh
missing:~$ ls -l /home
total 20                   # 当前目录下所有文件（不包括子目录内容）的总大小
drwxr-xr-x 1 missing  users  4096 Jun 15  2019 missing
```
#### 1. 权限（Permissions） - 10个字符
这是最左边的第一列，由10个字符组成，用于描述文件类型和访问权限。
- **第一个字符：文件类型**
    - `-`：普通文件 (Regular file)
    - `d`：目录 (Directory)
    - `l`：符号链接 (Symbolic link)
    - `b`：块设备文件 (Block device file，如硬盘)
    - `c`：字符设备文件 (Character device file，如终端、打印机)
    - `p`：命名管道 (Named pipe / FIFO)
    - `s`：套接字文件 (Socket file)
- **接下来的9个字符：文件权限** 这9个字符分为三组，每组3个字符，分别代表**所有者 (owner)**、**所属组 (group)** 和**其他用户 (others)** 的权限：
    - **第一组 (字符 2-4)：所有者的权限**
        - `r`：读 (Read)
        - `w`：写 (Write)
        - `x`：执行 (Execute)。对于目录，`x` 表示可以进入该目录。
        - `-`：表示没有该权限
    - **第二组 (字符 5-7)：所属组的权限**
        - `r`：读
        - `w`：写
        - `x`：执行
        - `-`：表示没有该权限
    - **第三组 (字符 8-10)：其他用户的权限**
        - `r`：读
        - `w`：写
        - `x`：执行
        - `-`：表示没有该权限
    **特殊权限位：** 在 `x` 的位置有时可能会出现 `s` 或 `t`：
    - `s`：当出现在所有者或组的 `x` 位置时，表示 **SUID (Set User ID)** 或 **SGID (Set Group ID)**。
        - SUID：当执行一个具有 SUID 权限的文件时，它的执行权限将临时变为文件所有者的权限，而不是执行者的权限。
        - SGID：当执行一个具有 SGID 权限的文件时，它的执行权限将临时变为文件所属组的权限。对于目录，在该目录下创建的新文件或目录将继承父目录的组ID，而不是创建者的主组ID。
    - `t`：当出现在其他用户的 `x` 位置时，表示 **Sticky Bit**。
        - Sticky Bit (粘滞位)：通常用于共享目录（如 `/tmp`）。它意味着只有文件的所有者、目录的所有者或超级用户才能删除或重命名该目录下的文件，即使其他用户对该目录有写权限。
#### 2. 硬链接数（Number of Hard Links）
这一列显示了指向该文件（或目录）的**硬链接数量**。
- 对于文件，硬链接数表示有多少个文件名指向同一个 inode（文件内容）。
- 对于目录，硬链接数是 2 + 该目录下子目录的数量（因为每个子目录都有一个 `.` 和 `..` 硬链接）。
#### 3. 所有者（Owner）
显示文件的**所有者用户名**。
#### 4. 所属组（Group）
显示文件的**所属组名**。
#### 5. 文件大小（Size）
显示文件的大小，单位是**字节（bytes）**。
- 对于目录，这里显示的是目录元数据的大小，通常是 4096 字节。
#### 6. 最后修改时间（Last Modified Timestamp）
显示文件的**最后修改日期和时间**。
#### 7. 文件名（Filename）
显示文件或目录的**名称**。
- 如果文件是符号链接，通常会显示 `link_name -> target_file` 的格式。

### 常用选项
- `-a`: 显示所有 (包括隐藏)
- `-l`: 长列表格式
- `-h`: 人类可读大小 (需配合 -l)
- `-t`: 按时间排序
- `-r`: 反向排序
- `-R`: 递归列出

# 文件操作
## `mv`
**基本语法：**
- `mv [选项] 源文件或目录 目标文件或目录`

### 基本用法
1. **移动文件/目录到另一个位置**：
```sh
mv file.txt /path/to/new_directory/  # 将 file.txt 移动到 new_directory 目录
mv my_dir /another/path/             # 将 my_dir 目录移动到 /another/path/
```
2. **重命名文件/目录**：
```sh
mv old_name.txt new_name.txt         # 将 old_name.txt 重命名为 new_name.txt
mv my_old_dir my_new_dir             # 将 my_old_dir 重命名为 my_new_dir
```

### 常用选项
- `-i` (interactive)：在覆盖现有文件之前进行提示确认。
- `-u` (update)：只在源文件比目标文件新时才进行移动或覆盖。
- `-v` (verbose)：显示移动过程的详细信息。

## `cp
**基本语法：**
- `cp [选项] 源文件或目录 目标文件或目录`

### 基本用法
1. **复制文件**：
```sh
cp file.txt new_file.txt             # 复制 file.txt 并重命名为 new_file.txt
cp file.txt /path/to/directory/      # 复制 file.txt 到指定目录 (保持原名)
```
2. **复制目录：** 复制目录时必须使用 `-r` 或 `-R` 选项（递归复制），否则 `cp` 命令无法复制目录。
```sh
cp -r my_directory new_directory_copy # 递归复制 my_directory 及其所有内容到 new_directory_copy
```

### 常用选项
- `-r` 或 `-R` (recursive)：递归复制目录及其内容。**复制目录时必用。**
- `-i` (interactive)：在覆盖现有文件之前进行提示确认。
- `-u` (update)：只在源文件比目标文件新时才进行复制。
- `-v` (verbose)：显示复制过程的详细信息。
- `-a` (archive)：保留文件的所有属性（权限、时间戳、所有者、组等），并递归复制目录。常用于备份。

## `rm`
**基本语法：**
- `rm [选项] 文件或目录...`

### 基本用法
1. **删除文件**：
```sh
rm file.txt                     # 删除 file.txt
rm another_file.log temp_file.tmp # 同时删除多个文件
```
2. **删除目录：** 删除非空目录时必须使用 `-r` 或 `-R` 选项（递归删除）。
```sh
rm -r empty_directory           # 删除空目录（虽然也可以直接用 rm -d）
rm -r my_old_directory          # 递归删除 my_old_directory 及其所有内容
```

### 常用选项
- `-r` 或 `-R` (recursive)：递归删除目录及其内容。**删除目录时必用。**
- `-f` (force)：强制删除，不提示确认。**请谨慎使用此选项，因为它具有破坏性。**
- `-i` (interactive)：在删除每个文件或目录之前进行提示确认。**推荐在不确定时使用。**
- `-v` (verbose)：显示删除过程的详细信息。
（ps：远离`rm -rf`）
## `mkdir`
**基本语法：**
- `mkdir [选项] 目录名...`

### 基本用法
**创建目录**：
```sh
mkdir my_new_directory          # 在当前目录下创建 my_new_directory
mkdir dir1 dir2 dir3            # 创建多个目录
mkdir /home/user/documents/reports # 创建指定路径的目录 (前提是 /home/user/documents 已存在)
mkdir -p /home/user/documents/reports # 递归创建多级目录, 如果上级目录不存在, 则会自动创建
```
### 常用选项
- `-p` (parents)：递归创建目录。如果父目录不存在，则一并创建。
- `-v` (verbose)：显示创建过程的详细信息。
- `-m` (mode)：在创建目录时直接设置其权限模式（例如 `mkdir -m 755 new_dir`）。

## `touch`
**基本语法**：
- `touch [选项] 文件名...`

### 基本用法
1. **创建新的空文件**
```sh
touch new_empty_file.txt        # 在当前目录下创建一个名为 new_empty_file.txt 的空文件
touch /home/user/logs/daily.log # 在指定路径下创建空文件 (前提是 /home/user/logs 目录已存在)
```
2. **更新文件或目录的时间戳**
```sh
touch my_file.txt # 更新 my_file.txt 的时间戳到当前时间
```

### 常用选项
- `-a` (access time)：只更新文件的**访问时间 (atime)**。
- `-m` (modification time)：只更新文件的**修改时间 (mtime)**。
- `-c` (no create)：如果文件不存在，**不创建新文件**。此选项通常与 `-a` 或 `-m` 结合使用。
- `-r 参照文件` (reference)：使用**参照文件的时间戳**来更新目标文件的时间戳。目标文件的时间戳将与参照文件的时间戳相同。
```sh
        touch -r source_file.txt target_file.txt
        # target_file.txt 的时间戳会变成 source_file.txt 的时间戳
```
- `-t STAMP` (timestamp)：使用指定的时间戳 `STAMP` 来更新文件的时间戳，而不是当前系统时间。`STAMP` 的格式通常是 `[[CC]YY]MMDDhhmm[.ss]`。
        - `CC`：世纪（可选，默认为当前世纪）
        - `YY`：年份的最后两位（可选，默认为当前年份）
        - `MM`：月份（01-12）
        - `DD`：日期（01-31）
        - `hh`：小时（00-23）
        - `mm`：分钟（00-59）
        - `.ss`：秒（可选，00-61，包括闰秒）
    - **示例：**
```sh
        touch -t 202401011200.00 old_file.txt
        # 将 old_file.txt 的时间戳设置为 2024 年 1 月 1 日 12:00:00
        touch -t 05241600 current_time.txt
        # 将 current_time.txt 的时间戳设置为 5月24日 16:00 (使用当前年份)
```
- `-d 日期时间字符串` (date string)：与 `-t` 类似，但允许更灵活的日期时间格式，例如 `YYYY-MM-DD HH:MM:SS`。
    - **示例：**
```sh
        touch -d "2023-12-25 10:30:00" holiday_plan.txt
        # 将 holiday_plan.txt 的时间戳设置为 2023 年 12 月 25 日 10:30:00
```

## `cat`
**基本语法**：
- `cat [选项] [文件名...]`

### 基本用法
1. **显示文件内容**
```sh
		cat myfile.txt           # 显示 myfile.txt 的内容
		cat file1.txt file2.txt  # 依次显示 file1.txt 和 file2.txt 的内容
 ```
 2. **连接文件**：将多个文件的内容按顺序连接起来，然后输出到标准输出。
```sh
		cat part1.txt part2.txt > combined.txt
		# 将 part1.txt 和 part2.txt 的内容连接起来，并重定向到 combined.txt 文件中
```
3. **创建文件并输入内容**：
```sh
		cat > newfile.txt        # 后输入内容，按`Ctrl + D` 结束
```
### 常用选项
- `-n` (number)：对所有输出行进行编号，从 1 开始。
- `-b` (number nonblank)：与 `-n` 类似，但只对非空行进行编号。空行不编号。
- `-s` (squeeze blank)：将连续的多个空行压缩成一个空行。
- `-E` (show ends)： 在每行的末尾显示 `$` 符号，用于指示行的结束。这对于调试文件中的隐藏换行符或行尾空格非常有用。
- `-T` (show tabs)：将制表符 (Tab) 显示为 `^I`。
- `-v` (show non-printing)： 显示非打印字符。通常与其他选项结合使用，用于调试文件编码或隐藏字符。

## `tr`
**基本语法**：
 - `tr [选项] <字符串1> [字符串2]`
 - **`<字符串1>`**: 这是一个字符集，表示需要被转换或删除的字符。
 - **`[字符串2]`**: 这是一个可选的字符集，当进行字符转换时，它指定了 `<字符串1>` 中对应的字符将被转换成的目标字符。如果省略，`tr` 命令将执行删除或压缩操作。
- 其实两个字符串也可以是单一字符或正则表达式
---
**重要提示：** `tr` 命令通常从标准输入读取数据，并将结果输出到标准输出。这意味着它经常与管道符 `|` 结合使用。

### 基本用法
1. **字符转换**: 将输入流中 `<字符串1>` 中的字符一对一地映射并转换为 `<字符串2>` 中对应的字符。
```sh
    echo "hello world" | tr 'a-z' 'A-Z'   # 将小写字母转换为大写
```
2. **字符删除**: 当只提供 `<字符串1>` 并使用 `-d` 选项时，`tr` 会删除输入流中所有匹配 `<字符串1>` 中任何字符的字符。
```sh
    echo "abc123def" | tr -d '0-9'        # 删除所有数字
```
3. **字符压缩**: 当使用 `-s` 选项时，`tr` 会将输入流中连续重复出现的 `<字符串1>` 中的字符压缩成一个单一的字符。
```sh
    echo "This   is   a   test" | tr -s ' ' # 将多个空格压缩成一个空格
```
4. **字符补集**：当使用 `-c` 选项时，`tr` 会处理不在 `<字符串1>` 中的字符，即对`<字符串1>` 中的字符进行 **补集** 操作。
```sh
	echo "123abc456" | tr -cd '0-9' # 只保留数字（删除所有非数字字符） 
	echo "hello world" | tr -c 'a-z' '*' # 将所有非小写字母的字符替换为 '*'
```
5. **截断字符**：当 `<字符串1>` 比 `<字符串2>` 长时，使用 `-t` 选项，会**截断** `<字符串1>` 使其与 `<字符串2>` 长度相同。
```sh
	echo "abcdef" | tr 'abc' 'xy' # 输出 xyydef
	echo "abcdef" | tr -t 'abc' 'xy' # 输出 xycdef
```
### 特殊字符表示
- `[:lower:]`: 所有小写字母。
- `[:upper:]`: 所有大写字母。
- `[:digit:]`: 所有数字 (0-9)。
- `[:alnum:]`: 所有字母和数字。
- `[:space:]`: 所有空白字符（空格、制表符、换行符等）。
- `\n`: 换行符。
- `\t`: 制表符。
- `a-z`: 字符范围表示，例如 'a' 到 'z'。
```sh
echo "MixED cAsE 123" | tr '[:upper:]' '[:lower:]' # 将大写转小写
echo "line1\n\nline2" | tr -s '\n' # 压缩连续的换行符
```

## `grep`
**基本语法**：
- `grep [选项] 模式 [文件...]`

### 基本用法
1. **在文件中搜索指定模式的文本**： `grep` 最核心的功能是从一个或多个文件中查找匹配指定“模式”（通常是正则表达式）的行，并将这些行打印到标准输出。
```sh
    grep "error" /var/log/syslog       # 在 syslog 文件中查找包含 "error" 的行
    grep "^start" my_config.conf        # 查找以 "start" 开头的行
    grep "PATTERN" file1.txt file2.txt  # 在多个文件中搜索模式
```
2. **通过管道输入进行搜索**： `grep` 可以从标准输入读取数据，这使得它能与其它命令通过管道 `|` 结合使用，对前一个命令的输出结果进行过滤。
```sh
    ls -l | grep ".txt$"               # 列出当前目录下所有以 ".txt" 结尾的文件/目录
    ps aux | grep "apache2"            # 查找所有与 apache2 相关的进程
```
3. **统计匹配行数**： 当使用 `-c` 选项时，`grep` 可以统计匹配模式的行数。
```sh
    grep -c "warning" /var/log/messages # 统计 messages 文件中包含 "warning" 的行数
```
4. **显示不匹配的行**： 当使用 `-v` 选项时，`grep` 可以显示不匹配指定模式的行。
```sh
    grep -v "#" my_script.sh            # 显示 my_script.sh 中所有不包含 "#" 的行（通常用于去除注释）
```
    
### 常用选项
- `-i` (--ignore-case)：**忽略大小写**。在搜索模式时不区分大小写字母。
- `-v` (--invert-match)：**反向匹配**。显示所有**不**匹配指定模式的行。
- `-n` (--line-number)：**显示行号**。在输出匹配行时，同时显示该行在文件中的行号。
- `-c` (--count)：**统计行数**。只输出匹配模式的行数，而不显示具体内容。
- `-l` (--files-with-matches)：**只列出文件名**。只输出包含匹配模式的文件的名称，而不是匹配的行内容。
- `-L` (--files-without-match)：**列出不匹配的文件名**。只输出**不**包含匹配模式的文件的名称。
- `-r` (--recursive)：**递归搜索**。在指定目录及其所有子目录中递归搜索文件。
- `-w` (--word-regexp)： **全字匹配**。只匹配作为完整单词出现的模式，而不是部分单词。
- `-x` (--line-regexp)：**整行匹配**。只匹配整行都与模式完全相同的行。
- `-A num` (--after-context=num)：**显示匹配行后的 num 行**。在显示匹配行的同时，也显示其后 `num` 行内容。
- `-B num` (--before-context=num)：**显示匹配行前的 num 行**。在显示匹配行的同时，也显示其前 `num` 行内容。
- `-C num` (--context=num)：**显示匹配行前后各 num 行**。显示匹配行以及它前后各 `num` 行内容。
- `-E` (--extended-regexp)：**使用扩展正则表达式**。允许使用更高级的正则表达式特性，例如 `+`、`?`、`|` 等。等同于 `egrep` 命令。
- `-F` (--fixed-strings)：**使用固定字符串**。将模式视为纯字符串，不解释为正则表达式。这在搜索包含特殊正则表达式字符（如 `.`、`*`、`[` 等）的字符串时非常有用，且通常更快。等同于 `fgrep` 命令。
- `-o` (--only-matching)：**只显示匹配部分**。只输出匹配模式的文本部分，而不是整行。   

## `sort`
**基本语法**：
- `sort [选项] [文件...]`
### 基本用法
1. **对文件内容进行排序**： 对文本文件中的行进行排序。默认情况下，它会根据每行的ASCII值（字典序）进行升序排列。
```sh
    sort names.txt              # 对 names.txt 文件内容进行升序排序并输出
    sort scores.txt > sorted_scores.txt # 排序并将结果重定向到新文件
```
2. **通过管道输入进行排序**： `sort` 经常与其它命令通过管道 `|` 结合使用，对前一个命令的输出结果进行排序。
```sh
    ls -l | sort -k 5n          # 根据文件大小（第5列，数值排序）对 ls -l 的输出进行排序
    cat data.txt | sort -r      # 读取 data.txt 内容并进行逆序排序
```
3. **合并已排序的文件**： `sort` 也可以使用 `-m` 选项来合并多个已经排序的文件，并保持其排序顺序。
```sh
    sort -m sorted_file1.txt sorted_file2.txt # 合并两个已排序的文件
```
4. **去重并排序**： 使用 `-u` 选项时，`sort` 可以在排序的同时删除重复的行。
```sh
    sort -u unique_items.txt    # 排序并去除重复行
```
    
### 常用选项
- `-r` (--reverse)：**逆序排序**。以降序（从大到小）排列结果。    ```
- `-n` (--numeric-sort)：**数值排序**。将每行内容解释为数值进行排序，而不是按字典序。这对于包含数字的行非常重要，可以避免 "10" 在 "2" 之前的错误排序。    ```
- `-k` (--key=KEYDEF)：**指定排序键**。根据行中的特定字段（列）进行排序。`KEYDEF` 的格式通常是 `F[.C][OPTS][,F[.C][OPTS]]`，其中 `F` 是字段号，`C` 是字符位置，`OPTS` 是排序选项。字段默认以空白字符分隔。
```
    # 假设文件 content.txt 内容为：
    # apple 10
    # banana 5
    # cherry 15
    sort -k 2n content.txt      # 根据第2列（数字）进行排序
    # 结果：
    # banana 5
    # apple 10
    # cherry 15
    
    sort -k 1,1r content.txt    # 根据第1列（整个字段，逆序）排序
```
- `-u` (--unique)：**去重**。在排序后，只保留唯一的行，删除重复的行。    ```
- `-f` (--ignore-case)：**忽略大小写**。在进行字典序比较时，将小写字母视为其对应的大写字母。    ```
- `-b` (--ignore-leading-blanks)：**忽略前导空白字符**。在比较行时，忽略每行开头（或字段开头）的空格和制表符。    ```
- `-t` (--field-separator=SEP)：**指定字段分隔符**。设置用于分隔字段的字符，默认为空白字符。
```sh
	# 假设文件 data.csv 内容为：
	# Name,Age,City
	# Alice,30,New York
	# Bob,25,London
	sort -t',' -k2n data.csv    # 以逗号为分隔符，根据第2列（年龄）进行数值排序
```
- `-V` (--version-sort)：**版本排序**。对版本号进行排序，例如 "v1.0", "v2.0", "v1.10" 会正确排序为 "v1.0", "v1.10", "v2.0"。
- `-M` (--month-sort)：**月份排序**。将三字母的月份名称（如 Jan, Feb）识别为月份顺序。
```sh
    echo -e "Jan\nDec\nFeb" | sort -M # 结果：Jan, Feb, Dec
```
- `-h` (--human-numeric-sort)：**人类可读的数值排序**。对带有单位（如 K, M, G）的数值进行排序（例如，`du -h` 的输出）。
```sh
    echo -e "1K\n2G\n100M" | sort -h # 结果：1K, 100M, 2G
```

## `head`与`tail`


# 文件权限

# Connecting programs
在 shell 中，程序有两个主要的“流”：它们的输入流和输出流。 想象一下，每个 Shell 程序都像是一个小工厂：它们需要原材料（输入）来工作，然后生产出产品（输出）。默认情况下，键盘就是所有程序的原材料来源（输入），而显示器就是所有产品展示的出口（输出）。 Shell 允许更改这些“原材料”的来源和“产品”的去向，甚至可以将一个工厂的产品直接输送到另一个工厂作为原材料。
## I/O 重定向：改变程序的输入和输出目的地
- **`> file`：重定向标准输出到文件 (覆盖)** 这个符号告诉 Shell，把左边程序的**标准输出**不再打印到屏幕上，而是**写入**到指定的 `file` 中。如果 `file` 已经存在，它的内容会被**完全覆盖**。
```sh
    echo hello > hello.txt
    # 这里的 'echo hello' 的输出是 'hello'。
    # '>' 符号将这个 'hello' 从屏幕重定向到了 hello.txt 文件里。
    
    cat hello.txt
    # 现在我们用 cat 命令来查看 hello.txt 的内容，确认 'hello' 已经写入。
    # 输出：hello
```
    
- **`< file`：重定向标准输入从文件** 这个符号告诉 Shell，把右边程序的**标准输入**不再从键盘获取，而是**从指定的 `file` 中读取**。
```sh
    cat < hello.txt
    # 'cat' 命令通常默认从标准输入（键盘）或指定文件读取。
    # 这里 '< hello.txt' 告诉 cat 从 hello.txt 文件中读取内容。
    # 效果和 'cat hello.txt' 类似，都是显示文件内容。
    # 输出：hello
```
    
- **结合使用：`< input_file > output_file`** 甚至可以同时重定向一个程序的输入和输出。
```sh
    cat < hello.txt > hello2.txt
    # 这里的 'cat' 命令从 hello.txt 读取内容（输入重定向）。
    # 然后 '>' 符号将 'cat' 的输出（也就是 hello.txt 的内容）写入到 hello2.txt 中。
    # 结果就是创建了一个 hello2.txt，内容与 hello.txt 相同。
    
    cat hello2.txt
    # 输出：hello
```
    
- **`>> file`：重定向标准输出到文件 (追加)** 如果不想覆盖现有文件的内容，而是想在文件末尾**添加**新的内容，就可以使用 `>>`。
```sh
    echo "This is a new line." >> hello.txt
    # 这会将 "This is a new line." 添加到 hello.txt 的末尾，不会覆盖之前的内容。
    
    cat hello.txt
    # 输出：
    # hello
    # This is a new line.
```
---

## 管道 (`|`)：连接程序的输入与输出
管道操作符 `|` 允许将**一个程序的标准输出，直接作为另一个程序的标准输入**。这就像在两个工厂之间搭建了一条传送带，一个工厂的产品（输出）立即变成了另一个工厂的原材料（输入），无需中间文件。
**基本概念：** `command1 | command2`
- `command1` 的输出 **不再显示在屏幕上**。
- `command1` 的输出 **直接传递** 给 `command2` 作为输入。
- `command2` 的输出 **正常显示在屏幕上**（除非它也被重定向）。

# 根用户、`sudo` 命令与权限
在类 Unix 系统中，**根用户 (root)** 几乎不受任何限制，但直接使用风险极高。通常用 **`sudo` 命令**，以当前用户身份临时获得 root 权限来执行特定操作，避免系统破坏。当遇到“**权限拒绝 (permission denied)**”错误时，往往是需要 `sudo`（super user do） 了。

---
`sudo`可以进行的操作：
```sh
	# 安装、更新和删除软件
	sudo apt update       # 更新软件包列表
	sudo apt upgrade      # 升级已安装的软件包到最新版本
	sudo apt install vim  # 安装 Vim 编辑器
	sudo apt remove vim   # 卸载 Vim

	# 启动、停止或重启系统服务
	sudo systemctl restart nginx  # 重启 Nginx 服务
	sudo systemctl stop apache2   # 停止 Apache 服务
	sudo systemctl start ssh      # 启动 SSH 服务

	# 管理用户和权限
	sudo useradd newuser       # 添加新用户
	sudo passwd newuser        # 设置新用户密码
	sudo userdel newuser       # 删除用户

	# 修改文件权限
	sudo chmod 755 /var/www/html  # 修改目录权限
	sudo chown user:user file.txt # 修改文件所属用户

	# 关机或重启系统
	sudo shutdown -h now  # 立即关机
	sudo reboot           # 立即重启
```
---
有一件事情是必须作为根用户才能做的，那就是向 `sysfs` 文件写入内容。系统被挂载在 `/sys` 下，`sysfs` 文件则暴露了一些内核（kernel）参数。 因此，不需要借助任何专用的工具，就可以在运行期间配置系统内核。**注意 Windows 和 macOS 没有这个文件**
但是`sudo command > file` 不能以 root 权限写入受保护的文件。
```sh
# 错误示例：Shell 试图以当前用户权限重定向，导致拒绝访问
sudo echo 3 > /sys/class/backlight/thinkpad_screen/brightness
# 输出：An error occurred while redirecting file 'brightness'
# open: Permission denied
```
**原因在于：** 重定向操作 (`>`, `|`, `<`) 是由**你的 Shell (以当前用户权限运行)** 执行的，而不是由 `sudo` 提升权限的命令执行的。在 `echo` 运行前，Shell 已经尝试打开文件并被拒绝了。

**解决方案**：
```sh
# 正确示例：tee 以 root 权限运行，能够写入文件
echo 3 | sudo tee /sys/class/backlight/thinkpad_screen/brightness
```
这里 `echo 3` 的输出通过管道 (`|`) 传递给 `sudo tee`。`tee` 本身在 `sudo` 的作用下以 root 权限运行，因此它能成功打开并写入受保护的文件。

