**队列:** 看作受限的线性标，具有先进先出FIFO特性
 
**堆:** 用于堆排序，大根堆和小根堆 

思路都是一样的 -> 先建堆，再调整，叶子节点-父节点-根节点不断比较交换

*最终状态:* 大根堆中，父节点的值大于左右孩子节点


leetcode 239
输出滑动窗口中的最大值
```
Input: nums = [1,3,-1,-3,5,3,6,7], and k = 3
Output: [3,3,5,5,6,7] 
Explanation: 

Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```
基本思路：
滑动是肯定要做的，关键是如何找出覆盖区域中的最大值。

1. 暴力方式
![image.png](https://upload-images.jianshu.io/upload_images/5220317-f552247fe0850a6c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
class Solution(object):  
    def maxSlidingWindow(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: List[int]
        """
        #暴力方式，每次对局部list取max |464ms
         if not nums: 
             return []    
         return [max(nums[i:i+k]) for i in range(len(nums) - k + 1)]
```

2. 
借助双端队列
```
class Solution:
    def maxSlidingWindow(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: List[int]
        """
        res = []
        d = collections.deque()
        for i in range(len(nums)):
            while d and d[-1] < nums[i]:
                d.pop()
            d.append(nums[i])
            if i > k - 1 and d[0] == nums[i - k]:
                d.popleft()
            if i >= k - 1:
                res.append(d[0])
                
        return res
```
3. 堆
[heapq模块介绍](https://github.com/qiwsir/algorithm/blob/master/heapq.md)
直接调用现有的pyhton包实现堆排序功能, 但是实现的速度异常慢
![image.png](https://upload-images.jianshu.io/upload_images/5220317-beeda60d17301935.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
[代码思路来源](https://blog.csdn.net/dpengwang/article/details/86350688)
```
def maxSlidingWindow(self, nums, k):
        res = []
        import  heapq
        pq = []
        for i in range(len(nums)):
            if i>=k-1:
                index = -1
                for j in range(len(pq)):
                    if pq[j] == -nums[i-k]:
                        index = j
                        break
                # print(pq.queue,index)
                if index!=-1:
                    del pq[index]
                heapq.heappush(pq, -nums[i])
                heapq.heapify(pq)
                res.append(-pq[0])
            else:
                heapq.heappush(pq,-nums[i])
        return res
```

