```python
def isSubsetSum(set, n, sum) :  # 模板：子集和、01背包
    if (sum == 0) : return True
    if (n == 0 and sum != 0) : return False
    if (set[n - 1] > sum) : 
        return isSubsetSum(set, n - 1, sum); 
    return isSubsetSum(set, n-1, sum) or isSubsetSum(set, n-1, sum-set[n-1]) 
```

[96. 不同的二叉搜索树](https://leetcode.cn/problems/unique-binary-search-trees/)

```python
# 1为根节点：左子树节点数为0，右子树节点数为n-1
# 2为根节点：左子树节点个数为1，右子树节点为n-2
# 转移方程：G(n) = G(0)*G(n-1) + G(1)*(n-2) + ...+ G(n-1)*G(0)
        dp = [0] * (n + 1)
        dp[0] = 1
        dp[1] = 1
        for i in range(2, n + 1):
            for j in range(i):
                dp[i] += dp[j] * dp[i - j - 1] # n=2: dp[2] = dp[0] * dp[1] + dp[1] * dp[0]
        return dp[-1]
```

