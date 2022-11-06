[1.两数之和](https://leetcode-cn.com/problems/two-sum)

[170.两数之和 III - 数据结构设计](https://leetcode-cn.com/problems/two-sum-iii-data-structure-design)

### TwoSum I

先穷举：

```java
    for (int i = 0; i < nums.length; i++) 
        for (int j = i + 1; j < nums.length; j++) 
            if (nums[j] == target - nums[i]) 
                return new int[] { i, j };
    return new int[] {-1, -1};
```

时间复杂度 O(N^2)，空间复杂度 O(1)。

用哈希表减少时间复杂度：

```java
略
```

哈希表的查询时间为 O(1)，算法时间复杂度 O(N)，空间复杂度 O(N) 存储哈希表；

### TwoSum II

设计一个类，有两个 API：

```java
class TwoSum：
    public void add(int number);
    public boolean find(int value);    // 是否存在两数和为 value
```

 `find` 方法用哈希表：

```java
class TwoSum：
    Map<Integer, Integer> freq = new HashMap<>();  // number：出现频次

    public void add(int number) {
        freq.put(number, freq.getOrDefault(number, 0) + 1)
    
    public boolean find(int value)：
        for (Integer key : freq.keySet())：
            int residue = value - key;
            if (residue == key && freq.get(key) > 1)：return true;
            if (residue != key && freq.containsKey(residue))：return true;
        return false;
```

 **`find` 两种情况：**

- `add` 了 `[3,3,2,5]` 之后，执行 `find(6)`，由于 3 出现了两次，3 + 3 = 6，返回 true。

- `add` 了 `[3,3,2,5]` 之后，执行 `find(7)`， `key` 为 2，residue 为 5 ，返回 true。

时间复杂度：`add` 是 O(1)，`find` 是 O(N)，空间 O(N)；

如果API的 `find` 方法使用频繁，每次都要 O(N) 时间，是否可以优化？参考上一题，借助**哈希集合**优化 `find` 方法：

```java
class TwoSum {
    Set<Integer> sum = new HashSet<>();
    List<Integer> nums = new ArrayList<>();

    public void add(int number)：
        for (int n : nums)： // 记录所有可能的和
            sum.add(n + number);
        nums.add(number);
    
    public boolean find(int value)：
        return sum.contains(value);
```

 `sum` 中储存所有可能的和，每次 `find` 只花 O(1) 时间在集合中判断是否存在；

### 三、总结

**一般的，先把数组排序，再用双指针技巧**。

TwoSum 启发：HashMap 或 HashSet 可处理无序数组；如果数组有序：

```java
    int left = 0, right = nums.length - 1;
    while (left < right)：
        int sum = nums[left] + nums[right];
        if (sum == target)： return new int[]{left, right};
        else if (sum < target)： left++; // 让 sum 大一点
        else if (sum > target)： right--; // 让 sum 小一点
    return new int[]{-1, -1};
```

