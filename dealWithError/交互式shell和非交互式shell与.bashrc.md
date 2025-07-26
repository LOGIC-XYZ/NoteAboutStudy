时间            2025.07.10
起因：发现 /.bashrc 文件后的 export 和 echo 指令无效

### 原因分析：
```.bashrc
# If not running interactively, don't do anything 
case $- in 
	*i*) ;; 
	  *) return;; 
esac
```
- `case $- in *i*)`：检查当前 shell 的选项 (`$-`) 是否包含 `i` (表示 **i**nteractive，交互式)。
- `*) return;;`：**如果不是交互式 shell (即 `-` 不包含 `i`)，则执行 `return` 命令。**

`return` 命令会使脚本**立即退出**。这意味着，如果 MobaXterm 会话在连接时，启动的 shell 被识别为**非交互式**，那么 `~/.bashrc` 在执行到 `return` 之后的所有内容都会被跳过。

#error #shell #MobaXterm 