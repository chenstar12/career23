[875.爱吃香蕉的珂珂](https://leetcode-cn.com/problems/koko-eating-bananas)

[1011.在D天内送达包裹的能力](https://leetcode-cn.com/problems/capacity-to-ship-packages-within-d-days)

二分查找运用：在**有序数组**中搜索目标值；如果目标值有重复，返回目标值的左边界/右边界；

二分查找如何运用到实际问题？当搜索空间有序，通过二分搜索「剪枝」来提升效率。

### 一、「Koko 吃香蕉」

每小时最多吃一堆香蕉，如果吃不下就下一小时再吃；如果吃完了一堆还有胃口，也是下一小时。求解吃香蕉的**最小速度**（根/小时）。

抛开二分法，如何暴力求解？要求`H` 小时吃完香蕉的最小速度 `speed`，请问 `speed` 最大值，最少值为多少？

显然：最少为 1，最大为 `max(piles)`。暴力解法：从 1 开始穷举到 `max(piles)`：

```java
    int max = getMax(piles); // piles 数组的最大值
    for (int speed = 1; speed < max; speed++)
        if (canFinish(piles, speed, H)) // 以 speed 能否在 H 小时内吃完
            return speed;
    return max;
```

在**连续的空间线性搜索，是二分查找可用的标志**。由于要求最小速度，可用**搜索左侧边界的二分查找**：

```java
    int left = 1, right = max(piles) // 搜索左侧边界的二分法
    while (left <= right) {
        int mid = ...
        if (canFinish(piles, mid, H))： right = mid;
        else：...
    return left;
```

辅助函数：

```python
def canFinish(int[] piles, int speed, int H): # 时间复杂度 O(N)
    int time = 0;
    for (int n : piles) {
        time += n / speed + (1 + if n % speed > 0 else 0;
    return time <= H
```

### 二、扩展延伸

[1011.在D天内送达包裹的能力](https://leetcode-cn.com/problems/capacity-to-ship-packages-within-d-days)

要在 `D` 天内运输完所有货物，货物不可分割，求解运输的最小载重（下文称 `cap`）；

类似上一题，先确定 `cap` 的最小值和最大值；要求**最小载重**，用搜索左侧边界的二分查找：

```java
    int left = getMax(weights);
    int right = getSum(weights) + 1;
    while (left < right) { // 寻找左侧边界的二分查找
        int mid = left + (right - left) / 2;
        if (canFinish(weights, D, mid)) {
            right = mid;
        } else {
            left = mid + 1;
    return left;

// 如果载重为 cap，是否能在 D 天内运完货物？
boolean canFinish(int[] w, int D, int cap) {
    int i = 0;
    for (int day = 0; day < D; day++) {
        int maxCap = cap;
        while ((maxCap -= w[i]) >= 0) {
            i++;
            if (i == w.length)
                return true;
    return false;
```

