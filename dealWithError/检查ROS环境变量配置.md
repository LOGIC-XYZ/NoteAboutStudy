时间            2025.04.18
起因：今天尝试跑包的时候，无论是roslaunch、rosbag还是roscore，都说命令没有找到，需要安装包（但是明明早安装好了ros，都已经用了好久）

### 解决路径：检查ROS环境变量
#### **1. 确认当前使用的 Shell 类型**
```bash
echo $SHELL
```
- 输出 `/bin/bash` → 使用 **Bash**
- 输出 `/bin/zsh` → 使用 **Zsh**
---

#### **2. 根据 Shell 类型配置环境变量**
- **如果使用 Bash**：
```bash
# 将配置写入 ~/.bashrc
    echo "source /opt/ros/<版本名>/setup.bash" >> ~/.bashrc
    # 立即生效
    source ~/.bashrc
```
- **如果使用 Zsh**：
```bash
# 将配置写入 ~/.zshrc
    echo "source /opt/ros/<版本名>/setup.zsh" >> ~/.zshrc
    # 立即生效
    source ~/.zshrc
```
> 注意：Zsh 应使用 `setup.zsh` 而非 `setup.bash`，因为不同 Shell 的初始化脚本可能不同。

#error #shell #ROS