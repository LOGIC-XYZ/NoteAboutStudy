# 项目结构和初始化
## 目录结构
```tree
myproject/
├── docs/         # 文档
├── src/          # 源代码
├── tests/        # 测试代码
├── tools/        # 辅助工具脚本
├── LICENSE
├── README.md
```
## README.md
- 项目简介
- 安装/构建方法
- 使用示例
- 最小运行样例

# 项目中的命名
## 文件夹与工程名
全部小写 + 下划线分隔
## 文件名
文件名要全部小写, 可以包含下划线 (`_`) 或连字符 (`-`)， 依照项目的约定. 如果没有约定, 那么 “`_`” 更好
- `my_useful_class.cc`
- `my-useful-class.cc`
- `myusefulclass.cc`
## 命名空间命名
所有命名空间名使用小写字母
顶级命名空间应为项目名或团队名
命名空间中禁止使用缩写
命名空间结构应与目录结构一致

## 类型命名
所有类型命名 —— 类、结构体、类型定义 (`typedef`)、枚举、类型模板参数 —— 均使用相同约定，即以大写字母开始，每个单词首字母均大写，不包含下划线.
```cpp
// 类和结构体
class UrlTable { ...
class UrlTableTester { ...
struct UrlTableProperties { ...

// 类型定义
typedef hash_map<UrlTableProperties *, string> PropertiesMap;

// using 别名
using PropertiesMap = hash_map<UrlTableProperties *, string>;

// 枚举
enum UrlTableErrors { ...
```
## 变量命名
变量 (包括函数参数) 和数据成员名一律小写，单词之间用下划线连接。类的成员变量以下划线结尾，但结构体的就不用
## 常量命名
声明为 `constexpr` 或 `const` 的变量，或在程序运行期间其值始终保持不变的，命名时以 “k” 开头，然后大驼峰
## 函数命名
一般来说, 函数名的每个单词首字母大写，没有下划线。 对于首字母缩写的单词， 更倾向于将它们视作一个单词进行首字母大写 (例如, 写作 `StartRpc()` 而非 `StartRPC()`)

# 注释
[[02 - study/ProjectFormat/annotation|annotation]]