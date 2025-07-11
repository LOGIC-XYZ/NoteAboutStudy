时间            2025.07.10
起因：发现在MobaXter远程连接系统时重新启动终端./bashrc文件没有被加载，直连时有被加载

### 原因分析：MobaXterm 的会话类型
MobaXterm 在建立 SSH 连接后，通常会启动一个**非登录式交互 shell**。但是，有些情况下，它可能会被配置为启动一个**非交互式 shell**，或者其初始化过程跳过了加载 `~/.bashrc`。

### 解决路径：MobaXterm 会话设置
- **编辑你的 MobaXterm SSH 会话：** 在 MobaXterm 主界面，找到你连接服务器的会话，右键点击选择 **"Edit session"**。
- 导航到 **"Terminal settings"** 或 **"Advanced SSH settings"** (具体名称可能略有不同，取决于你的 MobaXterm 版本)。
- 查找名为 **"Force pseudo-terminal allocation"** 或 **"Login shell"** 或 **"Execute command"** 类似的选项。
    - **推荐做法：** 确保 **"Force pseudo-terminal allocation"** 选项是**勾选**的。这通常能确保启动一个交互式 shell。
    - **备用做法 (如果上述无效)：** 在 **"Execute command"** 或 **"Remote command"** 字段中，尝试填写：
        ```
        bash --login
        ```
        这将强制 Bash 作为一个登录 shell 启动，从而触发 `~/.bash_profile` 或 `~/.profile` 的加载。
- **保存会话设置**并重新连接。

#error #MobaXterm #shell