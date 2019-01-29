GBDT: 梯度提升决策树Gradient Boosting Decision Tree
又称
MART（Multiple Additive Regression Tree)
GBRT（Gradient Boosting Regression Tree）
和**随机森林**相同，也是基于集成的决策树模型。但又有本质的区别，随机森林利用投票的方式来提升单棵决策树的性能，而GBDT中利用GB梯度提升来提高效果，并且是**回归树**not决策树。

**分类树**：
**回归树**：
[参考](https://blog.csdn.net/w28971023/article/details/8240756)
>回归树总体流程也是类似，不过在每个节点（不一定是叶子节点）都会得一个预测值，以年龄为例，该预测值等于属于这个节点的所有人年龄的平均值。分枝时穷举每一个feature的每个阈值找最好的分割点，但衡量最好的标准不再是最大熵，而是最小化均方差--即（每个人的年龄-预测年龄）^2 的总和 / N，或者说是每个人的预测误差平方和 除以 N。这很好理解，被预测出错的人数越多，错的越离谱，均方差就越大，通过最小化均方差能够找到最靠谱的分枝依据。分枝直到每个叶子节点上人的年龄都唯一（这太难了）或者达到预设的终止条件（如叶子个数上限），若最终叶子节点上人的年龄不唯一，则以该节点上所有人的平均年龄做为该叶子节点的预测年龄。若还不明白可以Google "Regression Tree"，或阅读本文的第一篇论文中Regression Tree部分。

**Boosting Decision Tree**
[参考](https://zhuanlan.zhihu.com/p/38443853)
[知乎](https://zhuanlan.zhihu.com/p/36339161)
>Boosting，迭代，即通过迭代多棵树来共同决策。这怎么实现呢？难道是每棵树独立训练一遍，比如A这个人，第一棵树认为是10岁，第二棵树认为是0岁，第三棵树认为是20岁，我们就取平均值10岁做最终结论？--当然不是！且不说这是投票方法并不是GBDT，只要训练集不变，独立训练三次的三棵树必定完全相同，这样做完全没有意义。之前说过，GBDT是把所有树的结论累加起来做最终结论的，所以可以想到每棵树的结论并不是年龄本身，而是年龄的一个累加量。GBDT的核心就在于，每一棵树学的是之前所有树结论和的残差，这个残差就是一个加预测值后能得真实值的累加量。比如A的真实年龄是18岁，但第一棵树的预测年龄是12岁，差了6岁，即残差为6岁。那么在第二棵树里我们把A的年龄设为6岁去学习，如果第二棵树真的能把A分到6岁的叶子节点，那累加两棵树的结论就是A的真实年龄；如果第二棵树的结论是5岁，则A仍然存在1岁的残差，第三棵树里A的年龄就变成1岁，继续学。这就是Gradient Boosting在GBDT中的意义,一般梯度迭代。


有时序的意味
![image.png](https://upload-images.jianshu.io/upload_images/5220317-af2c26c409ee0e81.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
计算逻辑
![image.png](https://upload-images.jianshu.io/upload_images/5220317-8f7c43ee7eab23e4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
损失函数的负梯度来拟合本轮的损失，得到CART回归树，再将Gradient与Boosting Decision Tree相结合得到Gradient Boosting Decision Tree的**负梯度拟合**。

*GBDT中都是强学习器，而随机森林中是弱学习器*
![](https://upload-images.jianshu.io/upload_images/5220317-efc5e653a28a4e01.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>针对每一个叶子节点中的样本，我们求出使损失函数最小，也就是拟合叶子节点最好的输出值ctj。其中决策树中叶节点值已经生成一遍，此步目的是稍加改变决策树中叶节点值，希望拟合的误差越来越小。
 ![image.png](https://upload-images.jianshu.io/upload_images/5220317-53f14ef6027a5a49.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>针对每一个叶子节点中的样本，我们求出使损失函数最小，也就是拟合叶子节点最好的输出值ctj。其中决策树中叶节点值已经生成一遍，此步目的是稍加改变决策树中叶节点值，希望拟合的误差越来越小。
![image](http://upload-images.jianshu.io/upload_images/5220317-fab5a2b1ad12af45.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
这样我们便得到本轮的决策树拟合函数
![image](http://upload-images.jianshu.io/upload_images/5220317-79feec3da8cbdb21.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
从而本轮最终得到的强学习器表达式如下
![image](http://upload-images.jianshu.io/upload_images/5220317-54747ec71271ac00.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
通过损失函数的负梯度拟合，我们找到一种通用的拟合损失函数的方法，这样无论是分类问题还是回归问题，我们通过其损失函数的负梯度拟合，就可以用GBDT来解决我们的分类回归问题。

这个blog写的很详细，值得参考
[刘建平blog](https://www.cnblogs.com/pinard/p/6140514.html)

todo:转载的比较多，没有提炼出个人的思考，需要补
