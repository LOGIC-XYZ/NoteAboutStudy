AVL树即是二叉搜索树，也是平衡二叉树。（所以是一种 balanced binary search tree）
## 常见术语
### 节点高度
1. AVL树节点类
```cpp
/* AVL 树节点类 */
struct TreeNode {
    int val{};          // 节点值
    int height = 0;     // 节点高度
    TreeNode *left{};   // 左子节点
    TreeNode *right{};  // 右子节点
    TreeNode() = default;
    explicit TreeNode(int x) : val(x){}
};
```
2. 获取和更新节点高度的函数
```cpp
/* 获取节点高度 */
int height(TreeNode *node) {
    // 空节点高度为 -1 ，叶节点高度为 0
    return node == nullptr ? -1 : node->height;
}

/* 更新节点高度 */
void updateHeight(TreeNode *node) {
    // 节点高度等于最高子树高度 + 1
    node->height = max(height(node->left), height(node->right)) + 1;
}
```
### 节点平衡因子
balance factor -- 节点左子树的高度减去右子树的高度，同时规定空节点的平衡因子为 0。设平衡因子为$f$，则一棵AVL树的任意节点的平衡因都满足$-1 \leq f \leq 1$
获取节点平衡因子的函数
```cpp
/* 获取平衡因子 */
int balanceFactor(TreeNode *node) {
    // 空节点平衡因子为 0
    if (node == nullptr)
        return 0;
    // 节点平衡因子 = 左子树高度 - 右子树高度
    return height(node->left) - height(node->right);
}
```
## AVL树旋转
**旋转操作既能保持“二叉搜索树”的性质，也能使树重新变为“平衡二叉树”**。
将平衡因子绝对值 >1 的节点称为“失衡节点”。
旋转操作分为：右旋、左旋、先右旋后左旋、先左旋后右旋；
### 右旋
以失衡节点为根节点的子树，将该节点记为 `node` ，其**左**子节点记为 `child` ，执行“右旋”操作。
1. 当`child`无**右**子节点，以`child`为原点，将`node`向**右**旋转，并用`child`替代以前`node`的位置，此时`node`为`child`的**右**子节点。
2. 当`child`有**右**子节点（记为`grand_child`），需将`grand_child` 作为 `node` 的**左**子节点。
```cpp
/* 右旋操作 */
TreeNode *rightRotate(TreeNode *node) {
    TreeNode *child = node->left;
    TreeNode *grandChild = child->right;
    // 以 child 为原点，将 node 向右旋转
    child->right = node;
    node->left = grandChild;
    // 更新节点高度
    updateHeight(node);
    updateHeight(child);
    // 返回旋转后子树的根节点
    return child;
}
```
### 左旋
以失衡节点为根节点的子树，将该节点记为 `node` ，其**右**子节点记为 `child` ，执行“左旋”操作。
1. 当`child`无**左**子节点，以`child`为原点，将`node`向**左**旋转，并用`child`替代以前`node`的位置，此时`node`为`child`的**左**子节点。
2. 当`child`有**左**子节点（记为`grand_child`），需将`grand_child` 作为 `node` 的**右**子节点。
```cpp
/* 左旋操作 */
TreeNode *leftRotate(TreeNode *node) {
    TreeNode *child = node->right;
    TreeNode *grandChild = child->left;
    // 以 child 为原点，将 node 向左旋转
    child->left = node;
    node->right = grandChild;
    // 更新节点高度
    updateHeight(node);
    updateHeight(child);
    // 返回旋转后子树的根节点
    return child;
}
```

### **右旋和左旋操作在逻辑上是镜像对称的，它们分别解决的两种失衡情况也是对称的**。(对调一下left与right即可)

### 先左旋后右旋
先对 `child` 执行“左旋”，再对 `node` 执行“右旋”。![[other/attachments/Pasted image 20250406161714.png]]
### 先右旋后左旋
先对 `child` 执行“右旋”，再对 `node` 执行“左旋”。![[other/attachments/Pasted image 20250406161809.png]]
### 旋转的选择
![[other/attachments/Pasted image 20250406161856.png]]

| 失衡节点的平衡因子  | 子节点的平衡因子 | 应采用的旋转方法 |     |
| ---------- | -------- | -------- | --- |
| $>1$ (左偏树) | $\geq 0$ | 右旋       |     |
| $>1$ (左偏树) | $< 0$    | 先左旋后右旋   |     |
| $<1$ (右偏树) | $\leq 0$ | 左旋       |     |
| $<1$ (右偏树) | $>0$     | 先右旋后左旋   |     |
旋转操作函数
```cpp
/* 执行旋转操作，使该子树重新恢复平衡 */
TreeNode *rotate(TreeNode *node) {
    // 获取节点 node 的平衡因子
    int _balanceFactor = balanceFactor(node);
    // 左偏树
    if (_balanceFactor > 1) {
        if (balanceFactor(node->left) >= 0) {
            // 右旋
            return rightRotate(node);
        } else {
            // 先左旋后右旋
            node->left = leftRotate(node->left);
            return rightRotate(node);
        }
    }
    // 右偏树
    if (_balanceFactor < -1) {
        if (balanceFactor(node->right) <= 0) {
            // 左旋
            return leftRotate(node);
        } else {
            // 先右旋后左旋
            node->right = rightRotate(node->right);
            return leftRotate(node);
        }
    }
    // 平衡树，无须旋转，直接返回
    return node;
}
```
## AVL树常用操作
### 插入节点
在 AVL 树中插入节点后，从该节点到根节点的路径上可能会出现一系列失衡节点。因此，**需要从这个节点开始，自底向上（递归）执行旋转操作，使所有失衡节点恢复平衡**。
```cpp
/* 插入节点 */
void insert(int val) {
    root = insertHelper(root, val);
}

/* 递归插入节点（辅助方法） */
TreeNode *insertHelper(TreeNode *node, int val) {
    if (node == nullptr)
        return new TreeNode(val);
    /* 1. 查找插入位置并插入节点 */
    if (val < node->val)
        node->left = insertHelper(node->left, val);
    else if (val > node->val)
        node->right = insertHelper(node->right, val);
    else
        return node;    // 重复节点不插入，直接返回
    updateHeight(node); // 更新节点高度
    /* 2. 执行旋转操作，使该子树重新恢复平衡 */
    node = rotate(node);
    // 返回子树的根节点
    return node;
}
```
### 删除操作
在 AVL 树中删除节点后，从该节点到根节点的路径上也可能会出现一系列失衡节点。因此，**需要从这个节点开始，自底向上（递归）执行旋转操作，使所有失衡节点恢复平衡**。
```cpp
/* 删除节点 */
void remove(int val) {
    root = removeHelper(root, val);
}

/* 递归删除节点（辅助方法） */
TreeNode *removeHelper(TreeNode *node, int val) {
    if (node == nullptr)
        return nullptr;
    /* 1. 查找节点并删除 */
    if (val < node->val)
        node->left = removeHelper(node->left, val);
    else if (val > node->val)
        node->right = removeHelper(node->right, val);
    else {
        if (node->left == nullptr || node->right == nullptr) {
            TreeNode *child = node->left != nullptr ? node->left : node->right;
            // 子节点数量 = 0 ，直接删除 node 并返回
            if (child == nullptr) {
                delete node;
                return nullptr;
            }
            // 子节点数量 = 1 ，直接删除 node
            else {
                delete node;
                node = child;
            }
        } else {
            // 子节点数量 = 2 ，则将中序遍历的下个节点删除，并用该节点替换当前节点
            TreeNode *temp = node->right;
            while (temp->left != nullptr) {
                temp = temp->left;
            }
            int tempVal = temp->val;
            node->right = removeHelper(node->right, temp->val);
            node->val = tempVal;
        }
    }
    updateHeight(node); // 更新节点高度
    /* 2. 执行旋转操作，使该子树重新恢复平衡 */
    node = rotate(node);
    // 返回子树的根节点
    return node;
}
```

