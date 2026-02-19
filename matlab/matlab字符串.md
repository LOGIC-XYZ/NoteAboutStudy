---
tags:
  - matlab
---
## 矩形字符数组
在程序中存储更多维度的文本数据，通过创建矩形字符数组来实现的。
创建矩形字符数组的最简单方法是根据需要垂直或水平连接两个或多个一维字符数组。
可以通过以下两种方式垂直组合字符串
- 使用MATLAB连接运算符[]，并用分号（；）分隔每一行。请注意，在此方法中，每行必须包含相同数量的字符。对于长度不同的字符串，应根据需要使用空格字符填充。
- 使用char函数。如果字符串的长度不同，char会用尾随空格填充较短的字符串，以便每行具有相同的字符数。
#### 实例
创建一个脚本文件并在其中键入以下代码
示例
```
doc_profile = ['Zara Ali                             '; ...
               'Sr. Surgeon                          '; ...
               'R N Tagore Cardiology Research Center']
doc_profile = char('Zara Ali', 'Sr. Surgeon', ...
                  'RN Tagore Cardiology Research Center')
```
运行文件时，它显示以下结果-
```
doc_profile =
Zara Ali                             
Sr. Surgeon                          
R N Tagore Cardiology Research Center
doc_profile =
Zara Ali                            
Sr. Surgeon                         
RN Tagore Cardiology Research Center
```
可以通过以下两种方式水平组合字符串
- 使用MATLAB串联运算符**[]**，并用逗号或空格分隔输入字符串。此方法保留输入数组中的所有尾随空格。
- 使用字符串串联函数 strcat，此方法删除输入中的尾随空格。
#### 实例
创建一个脚本文件并在其中键入以下代码
示例
```
name =     'Zara Ali                             ';
position = 'Sr. Surgeon                          '; 
worksAt =  'R N Tagore Cardiology Research Center';
profile = [name ', ' position ', ' worksAt]
profile = strcat(name, ', ', position, ', ', worksAt)
```
运行文件时，它显示以下结果
```
profile = Zara Ali      , Sr. Surgeon      , R N Tagore Cardiology Research Center
profile = Zara Ali,Sr. Surgeon,R N Tagore Cardiology Research Center
```

## 将字符串合并到单元格数组中
从前面的讨论中可以明显看出，合并长度不同的字符串可能会很麻烦，因为数组中的所有字符串都必须具有相同的长度。在字符串的末尾使用了空格来使它们的长度相等。
但是，组合字符串的更有效方法是将结果数组转换为单元格数组。
MATLAB单元数组可以在数组中保存不同大小和类型的数据。单元数组提供了一种更灵活的方式来存储长度可变的字符串。
**cellstr**函数将字符数组转换为字符串的单元格数组。
#### 实例
创建一个脚本文件并在其中键入以下代码
示例
```
name =     'Zara Ali                             ';
position = 'Sr. Surgeon                          '; 
worksAt =  'R N Tagore Cardiology Research Center';
profile = char(name, position, worksAt);
profile = cellstr(profile);
disp(profile)
```
运行文件时，它显示以下结果
```
{                        
   [1,1] = Zara Ali                                      
   [2,1] = Sr. Surgeon                                 
   [3,1] = R N Tagore Cardiology Research Center
}
```

## MATLAB中的字符串函数

| 功能                        | 作用                    |
| ------------------------- | --------------------- |
| 用于在字符数组中存储文本，组合字符数组等的函数   |                       |
| blanks                    | 创建空白字符字符串             |
| cellstr                   | 从字符数组创建字符串的单元格数组      |
| char                      | 转换为字符数组（字符串）          |
| iscellstr                 | 确定输入是否为字符串的单元格数组      |
| ischar                    | 确定项目是否为字符数组           |
| sprintf                   | 将数据格式化为字符串            |
| strcat                    | 水平连接字符串               |
| strjoin                   | 将单元格数组中的字符串连接为单个字符串   |
| 识别字符串部分，查找和替换子字符串的函数      |                       |
| ischar                    | 确定项目是否为字符数组           |
| isletter                  | 字母数组元素                |
| isspace                   | 作为空格字符的数组元素           |
| isstrprop                 | 确定字符串是否属于指定类别         |
| sscanf                    | 从字符串读取格式化数据           |
| strfind                   | 在另一个字符串中查找一个字符串       |
| strrep                    | 查找并替换子字符串             |
| strsplit                  | 在指定的分隔符处分割字符串         |
| strtok                    | 字符串的选定部分              |
| validatestring            | 检查文本字符串的有效性           |
| symvar                    | 确定表达式中的符号变量           |
| regexp                    | 匹配正则表达式（区分大小写）        |
| regexpi                   | 匹配正则表达式（不区分大小写）       |
| regexprep                 | 使用正则表达式替换字符串          |
| regexptranslate           | 将字符串转换为正则表达式          |
| 字符串比较功能                   |                       |
| strcmp                    | 比较字符串（区分大小写）          |
| strcmpi                   | 比较字符串（不区分大小写）         |
| strncmp                   | 比较字符串的前n个字符（区分大小写）    |
| strncmpi                  | 比较字符串的前 n 个字符(不区分大小写) |
| 用于将字符串更改为大写或小写，创建或删除空格的函数 |                       |
| deblank                   | 从字符串末尾去除尾随空白          |
| strtrim                   | 删除字符串的前导和尾随空格         |
| lower                     | 将字符串转换为小写             |
| upper                     | 将字符串转换为大写             |
| strjust                   | 对齐字符数组                |