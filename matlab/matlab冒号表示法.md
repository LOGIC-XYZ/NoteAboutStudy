colon(:)是在MATLAB中最有用的运算符[[NoteAboutStudy/matlab/matlab运算符]]之一。它用于创建向量[[NoteAboutStudy/matlab/matlab向量]]，下标数组[[NoteAboutStudy/matlab/matlab数组]]和**指定迭代**。
如果要创建包含1到10的整数的行向量，请编写
示例
```
1:10
```
MATLAB执行该语句并返回包含1到10的整数的行向量-
```
ans =                                                                                              
   1    2    3    4    5    6    7    8    9   10
```
如果要指定一个增量值而不是一个值，例如
示例
```
100: -5: 50
```
MATLAB执行该语句并返回以下结果-
```
ans =
   100    95    90    85    80    75    70    65    60    55    50
```
再举一个实例
示例
```
0:pi/8:pi
```
MATLAB执行该语句并返回以下结果-
```
ans =
   Columns 1 through 7
      0    0.3927    0.7854    1.1781    1.5708    1.9635    2.3562
   Columns 8 through 9
      2.7489    3.1416
```