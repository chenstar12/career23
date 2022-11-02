[100.相同的树](https://leetcode-cn.com/problems/same-tree)

[450.删除二叉搜索树中的节点](https://leetcode-cn.com/problems/delete-node-in-a-bst)

[701.二叉搜索树中的插入操作](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree)

[700.二叉搜索树中的搜索](https://leetcode-cn.com/problems/search-in-a-binary-search-tree)

[98.验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree)

实践：框架思维灵活运用，秒杀一切二叉树；

二叉树解题：明确一个节点要做的事情，剩下的事抛给框架。

```java
void traverse(TreeNode root)：
    // root 节点（base case）
    traverse(root.left);    // 其他的抛给框架
    traverse(root.right);
```

**1. 把二叉树所有的节点中的值加一？**

```java
void plusOne(TreeNode root)：
    if (root == null) return;
    root.val += 1;
    plusOne(root.left);
    plusOne(root.right);
```

**2. 判断两棵二叉树是否相同？**

```java
boolean isSameTree(TreeNode root1, TreeNode root2)：
    if (root1 == null && root2 == null) return true;
    if (root1 == null || root2 == null) return false;
    if (root1.val != root2.val) return false;
    return isSameTree(root1.left, root2.left)
        && isSameTree(root1.right, root2.right);
```

二叉搜索树（Binary Search Tree，**BST**）：节点值大于等于左子树，且小于等于右子树;

<img src="../pictures/BST/BST_example.png" alt="BST" style="zoom:33%;" />


BST 的基础操作：判断 BST 的合法性、增、删、查;

**判断 BST 的合法性**

这里有坑：按刚才的思路，每个节点只要和左右孩子比较？
```java
boolean isValidBST(TreeNode root)：
    if (root == null) return true;
    if (root.left != null && root.val <= root.left.val) return false;
    if (root.right != null && root.val >= root.right.val) return false;
    return isValidBST(root.left)
        && isValidBST(root.right);
```

错误：要小于右子树所有节点，但下面显然不是 BST

<img src="../pictures/BST/假BST.png" alt="notBST" style="zoom: 50%;" />

别慌，框架没错，只是细节没处理。BST 的定义 -> root 不只是和左右子节点比较（而是子树的所有节点）

```java
boolean isValidBST(TreeNode root)：//辅助函数: 增加函数参数/携带额外信息
    return isValidBST(root, null, null);

boolean isValidBST(TreeNode root, TreeNode min, TreeNode max):
    if (root == null) return true;
    if (min != null && root.val <= min.val) return false;
    if (max != null && root.val >= max.val) return false;
    return isValidBST(root.left, min, root.val)  //左子树 均小于 root.val
        && isValidBST(root.right, root.val, max); // 右子树 均大于 root.val
```

**一、在 BST 中查找一个数是否存在**

细节：利用 BST “左小右大”的特性 ---- 用**二分思想，每次排除一半**：

```java
boolean isInBST(TreeNode root, int target):
    if (root == null) return false;
    if (root.val == target): return true;
    if (root.val < target): return isInBST(root.right, target);
    if (root.val > target): return isInBST(root.left, target);
```

抽象一套**BST 遍历框架**：

```java
void BST(TreeNode root, int target):
    if (root.val == target): // 找到目标
    if (root.val < target): BST(root.right, target);
    if (root.val > target): BST(root.left, target);
```

**二、在 BST 中插入一个数**

插入一个数: 先找插入位置，再插入;

套框架，加上“改”的操作即可。一旦涉及“改”，函数需返回 TreeNode ，并接收返回值；

```java
TreeNode insertIntoBST(TreeNode root, int val) {
    if (root == null) return new TreeNode(val); // 找到空位置，插入新节点
    // if (root.val == val) 	//BST 不插入已存在元素
    if (root.val < val)：root.right = insertIntoBST(root.right, val);
    if (root.val > val)：root.left = insertIntoBST(root.left, val);
    return root;
```

**三、BST 删除一个数**

较复杂，但有框架不难。与插入类似，先“找”再“改”

```java
TreeNode deleteNode(TreeNode root, int key)：
    if (root.val == key) { // 已找到，删除
    } else if (root.val > key) {
        root.left = deleteNode(root.left, key);
    } else if (root.val < key)：
        root.right = deleteNode(root.right, key);
    return root;
```

如何删除节点？不能破坏 BST 的性质。有三种情况：

1：恰好是末端节点，两个子节点都为空

<img src="../pictures/BST/bst_deletion_case_1.png" alt="1" style="zoom: 25%;" />

```java
if (root.left == null && root.right == null)
    return null;
```

2：只有一个非空子节点，让孩子接替自己。

<img src="../pictures/BST/bst_deletion_case_2.png" alt="2" style="zoom: 33%;" />

```java
// 排除上个情况之后
if (root.left == null) return root.right;
if (root.right == null) return root.left;
```

3：有两个子节点，必须找到左子树中最大节点，或者右子树中最小节点接替自己。以第二种方式讲解。

<img src="../pictures/BST/bst_deletion_case_3.png" alt="2" style="zoom: 33%;" />

```java
if (root.left != null && root.right != null)：
    TreeNode minNode = getMin(root.right); // 右子树的最小节点
    root.val = minNode.val;
    root.right = deleteNode(root.right, minNode.val); // 删除 minNode
```

三种情况代入框架：

```java
TreeNode deleteNode(TreeNode root, int key)：
    if (root == null) return null;
    if (root.val == key) {
        if (root.left == null) return root.right; // 情况 1 和 2，同时处理
        if (root.right == null) return root.left;
        TreeNode minNode = getMin(root.right); // 情况 3
        root.val = minNode.val;
        root.right = deleteNode(root.right, minNode.val);
    } else if (root.val > key) {
        root.left = deleteNode(root.left, key);
    } else if (root.val < key) {
        root.right = deleteNode(root.right, key);
    return root;

TreeNode getMin(TreeNode node)：// 最小节点：BST 最左边
    while (node.left != null) node = node.left;
    return node;
```

**四、最后总结**

1. 二叉树算法总路线：把当前节点的事做好，其他的交给递归框架；

2. 如果当前节点对子节点有整体影响：通过辅助函数增加参数列表；

3. BST 遍历框架：
```java
void BST(TreeNode root, int target)：
    if (root.val == target) // 找到目标
    if (root.val < target) 
        BST(root.right, target);
    if (root.val > target)
        BST(root.left, target);
```

[98.验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree)：

```python
    def isValidBST(self, root):
        def helper(root,min,max): # 辅助函数
            if not root: return True # base case
            if min and root.val <= min.val: return False # 不满足BST
            if max and root.val >= max.val: return False
            return helper(root.left, min, root) and helper(root.right, root, max) # 左右子树递归
        return helper(root,None,None)
```
```java
// 用中序遍历验证BST：用pre节点保存上节点，比较cur和pre；
    long pre = Long.MIN_VALUE;
    public boolean isValidBST(TreeNode root) {
        if (root == null)：return true;
        if (!isValidBST(root.left))：return false; // 左子树
        if (root.val <= pre)：return false; // 当前节点（中序遍历）
        pre = root.val;
        return isValidBST(root.right); // 右子树
```
``` python
        if not p and not q: # 相同的树
            return True
        if not p or not q:
            return False
        if p.val == q.val:
            return self.isSameTree(p.left,q.left) and self.isSameTree(p.right,q.right)
        return False # 即：p.val != q.val
```

```python
    def deleteNode(self, root, key): # 删除二叉搜索树中的节点
        if not root: return None
        if root.val > key: root.left = self.deleteNode(root.left, key)
        elif root.val < key: root.right = self.deleteNode(root.right, key)
        else:  # root.val == key
            if not root.left or not root.right: # 情况1，2
                root = root.left if root.left else root.right
            else: #情况3
                cur = root.right
                while cur.left: cur = cur.left # 即getMin(root.right)
                root.val = cur.val
                root.right = self.deleteNode(root.right, cur.val)
        return root
```

```python
# 二叉搜索树中的插入 ---- 迭代
def insertIntoBST(self, root, val):
        if not root: return TreeNode(val)
        cur = root
        while cur:
            if cur.val < val:
                if not cur.right: 
                    cur.right = TreeNode(val)
                    break
                else:
                    cur = cur.right
            else:
                if not cur.left:
                    cur.left = TreeNode(val)
                    break
                else:
                    cur = cur.left
        return root
```
