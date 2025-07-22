# Finding how to use commands
对于一个命令，如何找出它的作用及其不同选项
- 使用 `-h` 或 `--help` 标志：`ls --help`
- 使用 `man` 命令查看手册页：`man ls`
- [TLDR pages](https://tldr.sh/) 提供命令的常见用例示例
- `apropos <keyword>` 或 `man -k <keyword>`: 搜索与关键字相关的手册页摘要。
- `info <command>`: GNU 项目的文档系统，通常更结构化和详细。
- `help <builtin_command>`: 查看 Bash 内建命令的帮助 (例如: `help cd`)。

# Finding files
##  `find`
类 UNIX 系统附带 `find` 命令，可以查找文件，一个强大的递归搜索工具。
```bash
# 查找所有名为 src 的目录
find . -name src -type d
# 查找路径中包含 test 文件夹的所有 python 文件
find . -path '*/test/*.py' -type f
# 查找所有在过去一天内修改过的文件
find . -mtime -1
# 查找所有大小在 500k 到 10M 之间的 tar.gz 文件
find . -size +500k -size -10M -name '*.tar.gz'
# 删除所有扩展名为 .tmp 的文件
find . -name '*.tmp' -exec rm {} \;
# 查找所有 PNG 文件并将它们转换为 JPG
find . -name '*.png' -exec convert {} {}.jpg \;
```

-exec 与 xargs:
- `-exec command {} \;`: 为每个文件执行一次命令 (效率较低)。
- `-exec command {} +`: 传递多个文件给单个命令 (更高效)。
- 安全处理特殊文件名: `find . -print0 | xargs -0 command`
常用条件:
- `-iname <pattern>`: 忽略大小写匹配。
- `-user <username>`: 按所有者查找。
- `-group <groupname>`: 按所属组查找。
- `-perm <mode>`: 按权限查找。
- `-empty`: 查找空文件/目录。
- `-maxdepth <n>` / `-mindepth <n>`: 限制搜索深度。

## `fd`
是一个用 Rust 编写的现代化命令行工具，旨在成为 `find` 命令更快、更直观、更友好的替代品。它默认带有颜色高亮，并智能地忽略版本控制文件等。

**基本语法：**
- `fd [选项] [模式] [路径]`

- **`[模式]`**: 所要查找的文件名或路径片段。`fd` 默认将模式视为正则表达式，但也可以使用固定字符串。
- **`[路径]`**: 可选参数，指定从哪个目录开始搜索。如果省略，`fd` 默认从当前目录开始搜索。

### 基本用法
1. **文件搜索**：
通过智能默认值（如忽略 `.git` 目录和隐藏文件）减少了搜索结果的噪音，并且默认会读取 `.gitignore` 和 `.fdignore` 文件，自动排除版本控制或临时文件。搜索结果默认进行颜色高亮显示，增强可读性。
```bash
fd my_document         # 在当前目录及其子目录中查找包含 "my_document" 的文件或目录
fd .txt /home/user/docs # 在 /home/user/docs 目录下查找所有以 ".txt" 结尾的文件
```
2. **正则匹配与模糊搜索**：
```bash
fd "log.*\.conf$"      # 查找以 "log" 开头，中间有任意字符，以 ".conf" 结尾的文件
```

### 常用选项
- `-e`(--extension)：只搜索指定扩展名的文件
- `-H`(--hidden)：搜索隐藏文件和目录，不搜索被忽略的文件（.gitignore）
- `-I`(--no-ignore)：搜索被忽略的文件，不搜索隐藏文件和目录
- `-u`(--unrestricted)：所有文件都搜索
- `-t`(--type)：只搜索特定类型的文件
	- `f` (file): 普通文件
	- `d` (directory): 目录
	- `l` (symlink): 符号链接
	- `e` (empty): 空文件或空目录
- `-s`(--case-sensitive)：区分大小写搜索
- `-L`(--follow)：遍历符号链接指向的目录
- `-x`(--exec)：对每个找到的匹配项执行指定的命令。`{}` 代表找到的文件路径。
	```bash
	fd .log -x rm          # 删除所有 .log 文件
	fd .txt -x mv {} {}.bak # 将所有 .txt 文件重命名为 .txt.bak
	```

## `locate`
`locate` 命令的工作原理与 `fd` 和 `find` 完全不同。它不实时遍历文件系统，而是依赖一个预先构建并定期更新的**文件名数据库**来执行搜索，因此速度非常快。


**基本语法：**
- `locate [选项] 模式`

- **`[模式]`**: 所要查找的文件名或路径中的字符串。`locate` 默认将模式视为子字符串匹配，且不区分大小写。

### 基本用法
1. **文件搜索**：
```bash
locate chrome           # 快速查找系统中所有路径中包含 "chrome" 的文件和目录
locate .bashrc          # 查找所有 .bashrc 文件的位置
```
2. **数据库驱动**：
```bash
sudo updatedb           # 更新 locate 数据库 (通常需要 root 权限)
```

### 常用选项
- `-i`(--ignore-case)：忽略大小写进行搜索
- `-b`(--basename)：只匹配文件或目录的**基本名称**，不匹配路径中的其他部分
	```bash
	locate -b '\my_file.txt' # 查找名为 my_file.txt 的文件，不匹配 /path/to/my_file.txt.bak
	```
	**注意：这里需要用反斜杠 `\` 来转义，确保 `\` 被 shell 传递给 `locate`，因为 `locate` 本身也支持通配符。**
- `-c`(--count)：只输出匹配的条目数量，不显示具体路径
- `-r`(--regexp)：将模式解释为**基本正则表达式**
	```bash
	locate -r '.*\.txt$'    # 查找所有以 .txt 结尾的文件
	```
- `-e`(--existing)：只显示当前存在于文件系统中的文件。因为数据库可能包含已被删除但未更新的信息。

# Finding code
`grep` (Global Regular Expression Print) 是最常用的文本模式搜索工具，其次是 `rg`，它提供了更快的性能和更好的默认设置 -- 自动跳过被忽略的文件和目录，会默认对匹配的结果进行颜色高亮显示，提升可读性
### `rg`
**基本语法：**
- `rg [选项] 模式 [路径]`

- **`模式`**: 所要搜索的[[正则表达式]]。`rg` 默认就支持正则表达式，无需额外的选项（如 `grep -E`）
- **`[路径]`**: 可选参数，指定从哪个目录开始搜索。如果省略，`rg` 默认从当前目录开始递归搜索

### 基本用法
1. **在文件中搜索**：
在一个或多个文件，以及它们所在的目录中递归地搜索匹配指定模式的文本。
```bash
rg "function main" src/      # 在 src/ 目录下递归搜索包含 "function main" 的行
rg "TODO:" .                 # 在当前目录及其子目录中查找所有 "TODO:" 标记
```
2. **通过管道输入进行搜索**：
```bash
cat log.txt | rg "error"     # 从 log.txt 的内容中搜索 "error"
ps aux | rg "nginx"          # 查找所有与 nginx 相关的进程
```

### 常用选项
- `-e PATTERN` (`--regexp PATTERN`)：使用指定的**正则表达式模式**进行搜索
    ```bash
    rg -e "error|fail" logs/           # 搜索包含 "error" 或 "fail" 的行
    ```
- `-i` (`--ignore-case`)：**忽略大小写**。在搜索模式时不区分大小写字母
- **`-v` (`--invert-match`)**：**反向匹配**。显示所有**不**匹配指定模式的行
    ```bash
    rg -v "^#" config.ini              # 显示 config.ini 中所有不以 "#" 开头的行（排除注释）
    ```
- **`-n` (`--line-number`)**：**显示行号**。在输出匹配行时，同时显示该行在文件中的行号
- **`-c` (`--count`)**：**统计行数**。只输出匹配模式的行数，而不显示具体内容
- **`-l` (`--files-with-matches`)**：**只列出文件名**。只输出包含匹配模式的文件的名称，而不显示匹配的行内容。
- **`-L` (`--files-without-matches`)**：**列出不匹配的文件名**。只输出**不**包含匹配模式的文件的名称。
- **`-w` (`--word-regexp`)**：**全字匹配**。只匹配作为完整单词出现的模式。
    ```bash
    echo "This is a test" | rg -w "is" # 只匹配独立单词 "is"，不匹配 "This" 中的 "is"
    ```
- **`-A NUM` (`--after-context=NUM`)**：**显示匹配行后的 NUM 行**。在显示匹配行的同时，也显示其后 `NUM` 行内容。
- **`-B NUM` (`--before-context=NUM`)**：**显示匹配行前的 NUM 行**。在显示匹配行的同时，也显示其前 `NUM` 行内容。
- **`-C NUM` (`--context=NUM`)**：**显示匹配行前后各 NUM 行**。显示匹配行以及它前后各 `NUM` 行内容。
- **`-F` (`--fixed-strings`)**：**固定字符串模式**。将模式视为纯字符串，不解释为正则表达式。当搜索包含特殊正则字符（如 `.`、`*`、`[` 等）的字符串时非常有用，且通常更快。
    ```
    rg -F "file.txt" my_directory/     # 查找字符串 "file.txt"，而不是正则表达式
    ```
- **`-g GLOB` (`--glob GLOB`)**：根据**文件 glob 模式**进行过滤，只搜索符合 glob 模式的文件。
    ```
    rg "foo" -g "*.js"                 # 只在 .js 文件中搜索 "foo"
    rg "bar" -g '!*.min.js'            # 搜索所有 .js 文件，但排除 .min.js 文件
    ```
- **`-t TYPE` (`--type TYPE`)**：**按文件类型过滤**。只搜索特定类型的文件。可以使用 `rg --type-list` 查看支持的类型列表。
    ```
    rg "TODO" -tgo                     # 只在 Go 语言文件中搜索 "TODO"
    rg "bug" -tpy,js                   # 在 Python 和 JavaScript 文件中搜索 "bug"
    ```
- **`--sort path | modified | accessed | created | filesize`**：根据**路径、修改时间、访问时间、创建时间或文件大小**对结果排序。
    ```
    rg "error" --sort modified         # 根据文件的修改时间对搜索结果排序
    ```

# Finding shell commands
如何找回之前输入的命令
- 按**向上箭头**键可以浏览之前的命令
- `history` 命令显示你的命令历史
- `history | grep find` 搜索包含 "find" 的命令
- **Ctrl+R** 反向搜索命令历史

## and more
#### 1. 高级历史搜索工具：`fzf`
传统的 `Ctrl+R` 历史搜索功能虽然有用，但 `fzf` (fuzzy finder) 将其提升到了一个全新的水平。
- **功能**：`fzf` 是一个通用的命令行模糊查找器，它可以与 Shell 历史、文件系统、进程列表等任何列表进行集成。当你把它绑定到历史搜索时（通常是替换 `Ctrl+R`），它会弹出一个交互式界面。你只需键入几个字符，`fzf` 就会即时为你过滤并显示所有模糊匹配的历史命令。

- **优势**：
    - **模糊匹配**：即使你只记得命令片段或顺序不完全正确，也能快速找到。
    - **交互式界面**：实时预览结果，支持上下箭头选择。
    - **极高效率**：大大减少了查找历史命令所需的时间和按键次数。
        
- **示例**： 安装 `fzf` 后，通常会有一个自动的 Shell 配置脚本（如 `~/.bashrc` 或 `~/.zshrc` 中的配置），将 `Ctrl+R` 绑定到 `fzf`。之后，当你按下 `Ctrl+R`，就会出现一个交互式窗口，你可以在其中键入任何内容来模糊搜索历史命令。
    
#### 2. 修改 Shell 历史行为：`HISTCONTROL`
`HISTCONTROL` 是一个 Shell 变量，用于控制历史记录的行为，让你能更“干净”地记录命令。
- **`ignorespace`**：
    - **作用**：如果你在命令前添加一个**空格**，该命令将**不会**被记录到历史文件中。
    - **用途**：非常适合执行一些敏感命令（如输入密码的命令，尽管密码不应明文出现在命令行）、一次性命令或不想污染历史记录的测试命令。
        
- **`ignoredups`**：
    - **作用**：**不记录连续重复**的命令。如果你连续两次输入相同的命令，只有第一次会被记录。
    - **用途**：避免历史文件中出现大量重复的命令，保持历史记录的整洁。
        
- **`ignoreboth`**：
    - **作用**：是 `ignorespace` 和 `ignoredups` 的**组合**。
    - **用途**：同时实现忽略带空格命令和连续重复命令的功能。
        
- **`erasedups`**：
    - **作用**：在将历史记录写入历史文件 (`.bash_history` 或 `.zsh_history`) **之前**，删除所有先前的重复行。这意味着即使你输入了重复命令，历史文件中也只会保留最新的一条。
    - **用途**：保持历史文件的高度整洁，确保每个命令只出现一次，且是最新的那次。
        
- **配置方法**： 这些通常在你的 Shell 配置文件（如 `~/.bashrc` 或 `~/.zshrc`）中设置：
    ```bash
    # 例如，设置为忽略空格和删除所有重复项
    export HISTCONTROL="ignorespace:erasedups"
    ```
- **手动编辑历史文件**： 如果忘记了在命令前添加前导空格，或者想清理历史文件中的特定敏感或不必要的记录，你可以直接编辑 `~/.bash_history` (Bash) 或 `~/.zsh_history` (Zsh) 文件。 **注意**：编辑后，可能需要执行 `history -c` (清除内存中的历史) 和 `history -r` (重新读取历史文件) 或重启 Shell 来使更改生效。
    
#### 3. 基于历史的自动建议
- **原理**：当开始输入一个命令时，Shell 会根据历史记录中最常用的、与当前输入**前缀匹配**的命令，**动态地提供灰色或半透明的建议**。通常只需按一下右箭头键 (`→`) 或 `End` 键，就能接受这个建议并自动补全命令。
- **Shell 支持**：
    - **Fish Shell**：作为其核心特性，Fish Shell 默认就提供了这一功能，其开箱即用的体验非常好。
    - **Zsh**：Zsh 是一个高度可定制的 Shell。你可以通过安装插件来启用这个功能，最流行的插件就是 **`zsh-autosuggestions`**。安装 Oh My Zsh 这样的 Zsh 框架后，安装此插件非常简单。
    - **Bash**：虽然 Bash 原生没有这个功能，但也有类似的插件可以实现，例如 **`ble.sh`**。安装和配置会比 Zsh 稍微复杂一些，但也能带来类似的便利。

# Directory Navigation
快速在目录间导航是提高效率的关键。除了基本的 `cd` 命令外，还有更智能的工具：
- `fasd` 和 `autojump` 根据频率和最近访问记录目录
- 使用 `z cool` 或 `j cool` 快速跳转到包含 "cool" 的常用目录
- `tree` 显示目录的树状结构
- 文件管理器如 `nnn`, `ranger`, 和 `broot` 提供交互式导航

##### `tree`命令常用选项:
- `-L <level>`: 显示指定层数的目录树 (例如: `tree -L 2`)。
- `-d`: 只显示目录。
- `-a`: 显示所有文件，包括隐藏文件。