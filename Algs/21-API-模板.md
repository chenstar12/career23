[347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/)

```python
        count = collections.Counter(nums) # 计数
        heap = [] # 最小堆
        for key, val in count.items():
            if len(heap) == k:
                if val > heap[0][0]: # heap[0][0]为val
                    heapq.heapreplace(heap, (val, key)) # heapreplace函数：（1）pop首元素；（2）赋值新元素，调整堆；
            else:
                heapq.heappush(heap, (val, key))
        return [item[1] for item in heap]
```

```python
        count = collections.Counter(nums)
        return [item[0] for item in count.most_common(k)]
```

