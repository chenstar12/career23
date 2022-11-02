[60. 排列序列](https://leetcode.cn/problems/permutation-sequence/):

```python
        def dfs(n, k, index, path):
            if index == n: return
            cnt = factorial[n - 1 - index] # 阶乘：包含的结果数量
            for i in range(1, n + 1):
                if used[i]: continue
                if cnt < k:
                    k -= cnt
                    continue
                used[i] = True
                path += [i]
                dfs(n, k, index + 1, path)
                return # 必须return，后面的数没必要遍历了

        if n == 0: return ""
        used = [False for _ in range(n + 1)]
        factorial = [1 for _ in range(n + 1)] # 阶乘：计数n!包含的结果数量cnt
        for i in range(2, n + 1): factorial[i] = factorial[i - 1] * i
        path = []
        dfs(n, k, 0, path)
        return ''.join([str(num) for num in path])
```

[93. 复原 IP 地址](https://leetcode.cn/problems/restore-ip-addresses/)：medium

```python
        def backtrack(s, tmp):
            if len(s) == 0 and len(tmp) == 4:
                res.append('.'.join(tmp))
                return
            if len(tmp) < 4:
                for i in range(min(3, len(s))):
                    seg, subS = s[:i + 1], s[i + 1:] # segment 和 subString: 
                    if seg and 0 <= int(seg) <= 255 and str(int(seg)) == seg:
                        backtrack(subS, tmp + [seg])
        res = []
        backtrack(s, [])
        return res
```

