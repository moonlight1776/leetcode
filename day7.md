0-1 背包问题：
物品价值 v = [x,x,x,x] 物品占据体积 v=[x,x,x,x]
限制背包总体积，如何让总价值最大？
【0-1意味每件物品要或者不要，不可拆分 1/2】
思考：每件物品要么带，要么不带，总的可能性是2^n，暴力的话，可以计算出对应的2^n种不同的重量，从中选最大的。
复杂度至少是 O(2^n * n * logn)
指数爆炸！！！
价值：1，2，6，6，9
重量：1，1，3，3，5
限制重量6，如果优先选价值高的，选了9和2，总价值11 < 6 + 6 【失败】

换个角度：假设我们已经找到了最优解决方案，最优方案的性质有哪些？
1. 总重量 < 限制
2. 价值最大
。。。废话
假设最优是选择a1...ak，如果拿掉ak的话，a1...ak-1应该是重量限制在V-vk条件下的最优解，否则一定能找到另一组1...k-1的方案比之前的好，从而推翻a1...ak是最优解的假设。
【反人类之反证法】和维特比的思路一致，看上去奇怪，但是逻辑是完全正确的。「概括一下就是最优解的子集一定也是最优的」
逐步递推 -> a1 应该是限制V - vk -vk-1 - ...- v2条件下的，最优选择。

to be or not to be?
c[i - 1, w-wi] + vi : 选择物品i
c[i-1,w] : 无法选择
![经典公式](https://upload-images.jianshu.io/upload_images/5220317-9ceb6f6a333dff62.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
这时候就来了这个经典公式：
1. 限制重量为0，价值肯定为0
2. 单个物品就超过总重量，铁定不能加入
3. 单个物品小于限制，能不能要看条件：
    限制总重量w,  只加 i-1件条件下的最大价值 VS wi + 限制 i-1件物品，总重 w -wi条件下的最大价值；二者需要进行比较，限制的约束更关注的是最终加入的物品的个数。

```
import numpy as np

def solve(vlist,wlist,totalWeight,totalLength):
    resArr = np.zeros((totalLength+1,totalWeight+1),dtype=np.int32)
    for i in range(1,totalLength+1):
        for j in range(1,totalWeight+1):
            if wlist[i] <= j:
                resArr[i,j] = max(resArr[i-1,j-wlist[i]]+vlist[i],resArr[i-1,j])
            else:
                resArr[i,j] = resArr[i-1,j]
        print(resArr)    
        return resArr[-1,-1]

if __name__ == '__main__':
    v = [0,60,100,120]
    w = [0,10,20,30]
    weight = 50
    n = 3
    result = solve(v,w,weight,n)
    print(result)
```
实际上我们在计算的过程中，就是在填充下表的过程。
限制重量条件下的，总价值
![总价值矩阵](https://upload-images.jianshu.io/upload_images/5220317-c07be9c345e2aa73.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**132**
呃呃呃呃呃 
蒙圈了
看了解析才大概明白思路
![132](https://upload-images.jianshu.io/upload_images/5220317-ca5f9634c560edbe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```   
 def minCut(self, s):
        """
        :type s: str
        :rtype: int
        """
        if len(s) == 0:
            return 0
        else:
            res = [i for i in range(-1, (len(s)))]
            if_palindrome = [[False] * (len(s) + 1) for _ in range(len(s) + 1)]
            for i in range(1, len(s) + 1):
                for j in range(i):
                    if s[i-1] == s[j] and (i - j <= 2 or if_palindrome[j + 1][i - 1]):
                        if_palindrome[j][i] = True
                        res[i] = min(res[j] + 1, res[i])
            return res[-1]
```
核心思路：
>if s[j:i] 回文：
    dp[i] = min(dp[i], dp[j] +1） 
参考：
https://www.smwenku.com/a/5c2f7c3cbd9eee35b21c7375
https://zhuanlan.zhihu.com/p/45261567
