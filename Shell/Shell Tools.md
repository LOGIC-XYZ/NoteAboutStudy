## Finding how to use commands
对于一个命令，如何找出它的作用及其不同选项
- 使用 `-h` 或 `--help` 标志：`ls --help`
- 使用 `man` 命令查看手册页：`man ls`
- [TLDR pages](https://tldr.sh/) 提供命令的常见用例示例
- `apropos <keyword>` 或 `man -k <keyword>`: 搜索与关键字相关的手册页摘要。
- `info <command>`: GNU 项目的文档系统，通常更结构化和详细。
- `help <builtin_command>`: 查看 Bash 内建命令的帮助 (例如: `help cd`)。

## Finding files
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
```