XGBoost stands for “Extreme Gradient Boosting”
GB论文：Greedy Function Approximation: A Gradient Boosting Machine, by Friedman
适用于有监督学习任务，
最简单的线性模型，带权重的输入特征的组合
![image.png](https://upload-images.jianshu.io/upload_images/5220317-b67cbe67923737ff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在训练过程中，boosting是加性训练。每个f都代表一棵树的结构和叶子节点的取值。
这种的不能简单梯度下降，可以fix what we have learned, and add one new tree at a time. 
损失函数中，一阶叫residual残差，二阶叫quadratic平方项

在XGB中，针对每一棵树[时序概念]都会有一个损失函数
在相同的叶子节点上，score是相同的，因此能对loss表达式进行一步优化。![image.png](https://upload-images.jianshu.io/upload_images/5220317-8610a951dbc038fb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/5220317-49ea86b3ca47cd09.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
简单记忆XGBoosting就是一大堆的CART，分类回归树，叶子节点对应score而不是具体的类别，有助于加速运算。XGB又准又快的功劳一部分就归功于CART。
问题1:树的结构是如何改变的？或者说，叶子节点的划分规则
【】如果能对一个树的结构进行评估，那么至少就能遍历所有可能的树结构，并选择出最好的。
![gains](https://upload-images.jianshu.io/upload_images/5220317-1c9d3abd87faf993.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

计算分裂效果，属于剪枝策略的一种

问题2: 如何根据不同的特征划分出t步对应的树？为什么这个树是tree1另一个就是tree2?
问题1问题2合并回答，固定t-1步的所有树，选择能使第t步的损失函数最小的CART树。迭代进行选择
同时，假定有N个样本，那每次选树需要进行N+N次计算G和H，BUT，这个过程中是完全没有联系的，所以可以并行处理。（CART建树本身就是NP难问题，通常也是贪婪的方式来做）
根据上面计算出的Gain，值越大越值得切分。逐层的进行树的构建，和普通决策树不同的地方是，在构建构成中就考虑了复杂度--γ参数。找到最优的树结构之后，就能找到最优节点值。


【tips】文章中提到，损失函数包含一次项和二次项很漂亮，为什么？
根据MSE计算出的公式，可以拓展到任意二次可微分的损失函数，因此可以自定义损失函数，只要能二次可微就可以。这大大拓宽了损失函数的选择范围。并且一阶导数和二阶导数的计算都是可并行的。

**正则化项**
对于损失函数来讲，正则化项能有效避免过拟合，但是在选择上就个人觉得很随意，能起到限制作用即可。
f是第t步的树，w是叶子的向量，T是叶子节点的总数。
![image.png](https://upload-images.jianshu.io/upload_images/5220317-0d1963cb2b9484c0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)






