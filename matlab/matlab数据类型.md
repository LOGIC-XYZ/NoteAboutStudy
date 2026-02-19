---
tags:
  - matlab
---
- single -- 单精度数值数据
- double -- 双精度数值数据
- logical -- 逻辑值1或0，分别表示true和false
- cell array -- 索引单元格数组，每个单元格能够存储不同维度和数据类型的数组
- structure -- 类似于C的结构，每个结构都具有能够存储不同维度和数据类型的数组的命名字段
- function handle -- 指向函数的指针
- user classes -- 从用户定义的类构造的对象
- java classes -- 从Java类构造的对象

## 数据类型转换
MATLAB提供了用于将值从一种数据类型转换为另一种数据的各种函数。下表显示了数据类型转换函数-

|函数|作用|
|---|---|
|char|转换为字符数组（字符串）|
|int2str|将整数数据转换为字符串|
|mat2str|将矩阵转换为字符串|
|num2str|将数字转换为字符串|
|str2double|将字符串转换为双精度值|
|str2num|将字符串转换为数字|
|native2unicode|将数字字节转换为 Unicode 字符|
|unicode2native|将Unicode字符转换为数字字节|
|base2dec|将N进制数字符串转换为十进制数|
|bin2dec|将二进制数字符串转换为十进制数|
|dec2base|将十进制数转换为字符串中的N进制数|
|dec2bin|将十进制转换为字符串中的二进制数|
|dec2hex|将字符串中的十进制数转换为十六进制数|
|hex2dec|将十六进制数字字符串转换为十进制数字|
|hex2num|将十六进制数字字符串转换为双精度数字|
|num2hex|将单精度和双精度转换为IEEE十六进制字符串|
|cell2mat|将单元格数组转换为数值数组|
|cell2struct|将单元数组转换为结构数组|
|cellstr|从字符数组创建字符串的单元格数组|
|mat2cell|将数组转换为单元格大小可能不同的单元格|
|num2cell|将数组转换为大小一致的单元格数组|
|struct2cell|将结构转换为单元格数组|

## 数据类型的确定
MATLAB提供了各种功能来识别变量的数据类型。
下表提供了确定变量的数据类型的功能-

|函数|作用|
|---|---|
|is|检测状态|
|isa|确定输入是否为指定类的对象|
|iscell|确定输入是否为单元格数组|
|iscellstr|确定输入是否为字符串的单元格数组|
|ischar|确定项目是否为字符数组|
|isfield|确定输入是否为结构数组字段|
|isfloat|确定输入是否为浮点数组|
|ishghandle|适用于处理图形对象句柄|
|isinteger|确定输入是否为整数数组|
|isjava|确定输入是否为Java对象|
|islogical|确定输入是否为逻辑数组|
|isnumeric|确定输入是否为数值数组|
|isobject|确定输入是否为MATLAB对象|
|isreal|检查输入是否为实数数组|
|isscalar|确定输入是否为标量|
|isstr|确定输入是否为字符数组|
|isstruct|确定输入是否为结构数组|
|isvector|确定输入是否为向量|
|class|确定对象的类别|
|validateattributes|检查数组的有效性|
|whos|列出工作空间中的变量，包括大小和类型|