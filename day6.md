树是不包含回路的连通无向图，树中的任意两个节点之间有且仅有唯一的路径相连。
对于一棵树，若有N个节点，则一定有N-1条边。

树是任意两个节点之间只有一条路径的无向图，只要是没有回路的连通无向图就是树。
![完全二叉树](https://upload-images.jianshu.io/upload_images/5220317-b2403f5066ec7666.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

完全二叉树的好处：
可以用一维数组来表示完全二叉树。
由父节点可知道，左孩子2*N，右孩子2*N+1。

由孩子节点也能知道父节点的位置，X/2即可。
如果完全二叉树的节点个数为N，则树的高度为log2N。

完全二叉树的典型应用是堆，堆又称为是优先队列。

堆是完全二叉树，是特殊的完全二叉树，满足大根堆或者小跟堆的性质。
最大堆、最小堆

用处：99、5、36、7、22、17、46、12、2、19、25、28、1和92。

想要找出最大的数字，怎么办？

冒泡、插入。。。。

假设我们已经将数组按照最小堆进行排列，那么寻找最小值，只需要输出1即可【堆顶】。

输出堆顶，插入23，放到堆顶，不满足要求，需要调整。【小根堆向下调整】

调整：想要将较大的数字23调整到它应该在的位置。不断的和孩子节点进行比较，并且与最小的孩子节点进行交换。

【除非与叶子节点进行交换，否则结构应该是不会变的】

**自底向上建堆，自顶向下调整。**
如果仅仅是想插入一个新的值，而不删除最小的堆顶元素的话。只需要将元素插入到【完全二叉树对应的列表的表尾部】
_________________________________________________________


**啰啰嗦嗦写了一堆，开始正式刷题！**

_________________________________________________________



**17**
和斐波那契数列的思路类似，通过保存来减少重复计算。
![image.png](https://upload-images.jianshu.io/upload_images/5220317-acad7f2b69cd2bf0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```
def letterCombinations(self, digits):
        """
        :type digits: str
        :rtype: List[str]
        """
        if digits == '':
            return []
        self.DigitDict=[' ','1', "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"]
        res = ['']
        for d in digits:
            res = self.letterCombBT(int(d),res)
        return res

    def letterCombBT(self, digit, oldStrList):
        return [dstr+i for i in self.DigitDict[digit] for dstr in oldStrList]
```


**46**
![image.png](https://upload-images.jianshu.io/upload_images/5220317-cacccfc9d340a076.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
class Solution:
    def permute(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        if not nums:
            return [[]]
        else:
            res = []
            for i, e in enumerate(nums):
                rest = nums[:i] + nums[i + 1:]
                rest_perms = self.permute(rest)
                for perm in rest_perms:
                    perm.append(e)
                res += rest_perms
            return res
```

先写一种，等我吃完饭，继续搞起！
刷题到嗨

