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
Shell 通过查询`$PATH`环境变量来寻找程序进而执行。
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
为什么需要绕过`$PATH`？
- **执行当前目录的脚本或程序：** 出于安全考虑，当前目录通常不在 `$PATH` 中。因此，如果想执行当前目录下的脚本或程序，就必须使用 `./` 前缀。
- **执行特定版本的程序：** 如果系统上安装了同一个程序的多个版本（例如，不同的 Python 版本），可以通过指定完整路径来确保执行的是想要执行的版本。
- **执行未添加到 `$PATH` 的程序：** 有些程序可能不会自动将其可执行文件添加到 `$PATH` 中的目录，此时就需要手动指定它们的路径来运行。
---
- 临时添加: `export PATH="/new/path:$PATH"`
- 永久修改: 编辑 `~/.bashrc` 或 `~/.zshrc`

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
ls -l            # 使用长列表格式显示
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

### 主要功能
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

### 主要功能
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

### 主要功能
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

### 主要功能
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

### 主要功能
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

### `cat`
**基本语法**：
- `cat [选项] [文件名...]`

### 主要功能
1. **显示文件内容**
	```sh
		cat myfile.txt           # 显示 myfile.txt 的内容
		cat file1.txt file2.txt  # 依次显示 file1.txt 和 file2.txt 的内容
    ```
# Connecting programs
