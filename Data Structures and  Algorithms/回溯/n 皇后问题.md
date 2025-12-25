**Q**：根据国际象棋的规则，皇后可以攻击与同处一行、一列或一条斜线上的棋子。给定 $n$ 个皇后和一个 $n×n$ 大小的棋盘，寻找使得所有皇后之间无法相互攻击的摆放方案。[51. N 皇后 - 力扣（LeetCode）](https://leetcode.cn/problems/n-queens/description/)

从回溯算法的角度看，$n×n$ 大小的棋盘共有 $n^2$ 个格子，给出了所有的选择 `choices` 。在逐个放置皇后的过程中，棋盘状态在不断地变化，每个时刻的棋盘就是状态 `state` 。

总共三个约束条件：**多个皇后不能在同一行、同一列、同一条对角线上**。对角线分为主对角线 `\` 和次对角线 `/` 两种。

1. **行约束**：皇后的数量和棋盘的行数都为 n ，所以**棋盘每行都允许且只允许放置一个皇后**。因此可以逐行放置，从第一行开始，在每行放置一个皇后，直至最后一行结束。（从本质上看，逐行放置起到了剪枝的作用，即避免了同一行出现多个皇后的所有搜索分支）
2. **列约束**：利用一个长度为 n 的布尔型数组 `cols` 记录每一列是否有皇后。在每次决定放置前，通过 `cols` 将已有皇后的列进行剪枝，并在回溯中动态更新 `cols` 的状态。
3. **对角线约束**：设棋盘中某个格子的行列索引为 $(row,col)$ ，**主对角线上所有格子的 $row−col$ 为恒定值**，**次对角线上的所有格子的 $row+col$ 是恒定值**。如果两个格子满足 $row1−col1=row2−col2$ ，则它们一定处在同一条主对角线上。如果两个格子满足 $row1+col1=row2+col2$ ，则它们一定处在同一条次对角线上。（借助两个数组`diags1`与`diags2`来记录）

# 代码实现
$n$ 维方阵中 $row−col$ 的范围是 $[−n+1,n−1]$ ，$row+col$ 的范围是 $[0,2n−2]$ ，所以主对角线和次对角线的数量都为 $2n−1$ ，即数组 `diags1` 和 `diags2` 的长度都为 $2n−1$ 。因为主对角线的编号范围是  $[−(n-1),n−1]$，统一加 `n-1` 转换成正数索引。
```cpp
void backtrack(int row, int n, vector<vector<string>> &state, vector<vector<vector<string>>> &res, vector<bool> &cols,
               vector<bool> &diags1, vector<bool> &diags2) {
    // 当放置完所有行时，记录解
    if (row == n) {
        res.push_back(state);
        return;
    }
    // 遍历所有列
    for (int col = 0; col < n; col++) {
        // 计算该格子对应的主对角线和次对角线
        int diag1 = row - col + n - 1;
        int diag2 = row + col;
        // 剪枝：不允许该格子所在列、主对角线、次对角线上存在皇后
        if (!cols[col] && !diags1[diag1] && !diags2[diag2]) {
            // 尝试：将皇后放置在该格子
            state[row][col] = "Q";
            cols[col] = diags1[diag1] = diags2[diag2] = true;
            // 放置下一行
            backtrack(row + 1, n, state, res, cols, diags1, diags2);
            // 回退：将该格子恢复为空位
            state[row][col] = "#";
            cols[col] = diags1[diag1] = diags2[diag2] = false;
        }
    }
}

vector<vector<vector<string>>> nQueens(int n) {
    // 初始化 n*n 大小的棋盘，其中 'Q' 代表皇后，'#' 代表空位
    vector<vector<string>> state(n, vector<string>(n, "#"));
    vector<bool> cols(n, false);           // 记录列是否有皇后
    vector<bool> diags1(2 * n - 1, false); // 记录主对角线上是否有皇后
    vector<bool> diags2(2 * n - 1, false); // 记录次对角线上是否有皇后
    vector<vector<vector<string>>> res;

    backtrack(0, n, state, res, cols, diags1, diags2);

    return res;
}
```
逐行放置 n 次，考虑列约束，则从第一行到最后一行分别有 n、n−1、…、2、1 个选择，使用 $O(n!)$ 时间。当记录解时，需要复制矩阵 `state` 并添加进 `res` ，复制操作使用 $O(n^2)$ 时间。因此，**总体时间复杂度为 $O(n!⋅n^2)$** 。实际上，根据对角线约束的剪枝也能够大幅缩小搜索空间，因而搜索效率往往优于以上时间复杂度。

数组 `state` 使用 $O(n^2)$ 空间，数组 `cols`、`diags1` 和 `diags2` 皆使用 $O(n)$ 空间。最大递归深度为 n ，使用 $O(n)$ 栈帧空间。因此，**空间复杂度为 $O(n^2)$** 。
