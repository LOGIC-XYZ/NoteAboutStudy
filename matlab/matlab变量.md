- 变量在使用前必须具有值。
- 当表达式返回未分配给任何变量的结果时，系统会将其分配给名为ans的变量，该变量可在以后使用。
--[[NoteAboutStudy/matlab/matlab数据类型]]

## 我忘记了变量！
[[NoteAboutStudy/matlab/matlab命令]]
**who**命令显示已使用的所有变量名。
```
who
```
MATLAB将执行上述语句并返回以下结果-
```
Your variables are:
a    ans  b    c
```

**whos**命令显示了有关变量的更多信息-
- 当前内存中的变量
- 每个变量的类型
- 分配给每个变量的内存
- 它们是否为复杂变量
```
whos
```
MATLAB将执行上述语句并返回以下结果-
```
Attr Name        Size        Bytes      Class
==== ====        ====        ====       ===== 
   a             1x1           8        double
   ans           1x70         757        cell
   b             1x1           8        double
   c             1x1           8        double
Total is 73 elements using 781 bytes
```

该**clear**命令variable(s)从内存中删除所有（或指定的）。
```
clear x     % it will delete x, won't display anything
clear       % it will delete all variables in the workspace
            %  peacefully and unobtrusively
```

## format命令
默认情况下，MATLAB显示带有四个小数位值的数字。这就是所谓的**short format**。

- **format long**命令在小数点后显示16位数字。
- format short命令即为short format
- **format bank**命令将数字四舍五入到小数点后两位。例如，
- **format short e**命令允许以指数形式显示，小数点后四个位加上指数。
- **format long e**命令允许以指数形式显示，小数点后四个位加上指数。
- **format rat**命令给出了由计算得出的最接近的有理表达式。例如，
```
	format rat
	4.678 * 4.9
```
	MATLAB将执行上述语句并返回以下结果
```
	ans = 34177/1491
```

## 创建向量
[[NoteAboutStudy/matlab/matlab向量]]

向量是一维数字数组。MATLAB允许创建两种类型的向量
- **Row vectors**（行向量） 通过将元素集括在方括号中并使用空格或逗号定界元素来创建。
	例如，
```
	r = [7 8 9 10 11]
```
	MATLAB将执行上述语句并返回以下结果-
```
	r =
	   7    8    9   10   11

```
	另一个实例，
```
	r = [7 8 9 10 11];
	t = [2, 3, 4, 5, 6];
	res = r + t
```
	MATLAB将执行上述语句并返回以下结果-
```
	res =

         9         11         13         15         17
```

- **Column vectors（列向量）** 通过将元素集括在方括号中并使用分号（;）分隔元素来创建。

## 创建矩阵
[[NoteAboutStudy/matlab/matlab矩阵]]
矩阵是数字的二维数组。

在MATLAB中，通过以空格或逗号分隔的元素的顺序输入每一行来创建矩阵，并用分号分隔行的结尾。
示例
```
m = [1 2 3; 4 5 6; 7 8 9]
```
MATLAB将执行上述语句并返回以下结果-
```
m =
       1              2              3       
       4              5              6       
       7              8              9
```

## 创建字符串
[[NoteAboutStudy/matlab/matlab字符串]]
示例
```
my_string = '(cainiaojc.com)'
```
MATLAB将执行上述语句并返回以下结果
```
my_string = (cainiaojc.com)
```
