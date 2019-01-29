树的遍历

最直接的就是递归的方式，关注**边界**就好
还有深度优先和广度优先、先序中序后序
BST：二叉搜索树，满足二分的概念，leftchild < root < rightchild
对于BST按照中序遍历的话，应该是严格递增的
**对二叉树进行中序遍历，判断是否严格有序递增。**
**98**
```
    def isValidBST(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        # 中序遍历应是递增的有序list
        def inorderTraversal(root): 
            # 经典的递归   [左递归] -【处理root】-[右递归]
            if root == None:
                return []
            res = []
            res += inorderTraversal(root.left)
            res.append(root.val)
            res += inorderTraversal(root.right)
            return res
 
        res = inorderTraversal(root)
        if res != sorted(list(set(res))): return False
        return True
```
![98](https://upload-images.jianshu.io/upload_images/5220317-e9df46102f1dffc3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**102**
```
def levelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        # 层次遍历相对简单，定义list不断extend
        if not root:
            return []
        res, level = [], [root]
        while level:
            # 记录当前层
            res.append([node.val for node in level])
            temp = []
            # 获取当前层的所有孩子节点
            for node in level:
                temp.extend([node.left, node.right])
            # 去掉为空的子节点
            level = [node for node in temp if node]
        return res
```
![](https://upload-images.jianshu.io/upload_images/5220317-e692f65414bdad8f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**107**
逆序输出的话，取巧的方式是直接反转
```
def levelOrderBottom(self, root):
        # 层次遍历相对简单，定义list不断extend
        if not root:
            return []
        res, level = [], [root]
        while level:
            # 记录当前层
            res.append([node.val for node in level])
            temp = []
            # 获取当前层的所有孩子节点
            for node in level:
                temp.extend([node.left, node.right])
            # 去掉为空的子节点
            level = [node for node in temp if node]
        return res[::-1]
```
![image.png](https://upload-images.jianshu.io/upload_images/5220317-a5ea3044b170f616.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

用队列的BFS思想来做
```
    def levelOrderBottom(self, root):
        res = []
        queue = [(root, 0)]
        while len(queue) > 0:
            node, depth = queue.pop()
            if node:
                if len(res) <= depth:
                    # list.insert(index, obj) 指定index的插入
                    res.insert(0, [])
                # 在遍历的时候，更改下插入位置，插入至对应层
                res[-(depth+1)].append(node.val)
                queue.insert(0, (node.left, depth+1))
                queue.insert(0, (node.right, depth+1))
        return res
```
用栈实现DFS，和BFS很相似
```
def levelOrderBottom(self, root):
        res = []
        stack = [(root, 0)]
        while len(stack) > 0:
            node, depth = stack.pop()
            if node:
                if len(res) <= depth:
                    res.insert(0, [])
                res[-(depth+1)].append(node.val)
                stack.append((node.right, depth+1))
                stack.append((node.left, depth+1))
        return res
```
同样的DFS，写成递归
```
    def levelOrderBottom(self, root):
        res = []
        self.dfs(root, 0, res)
        return res

    def dfs(self, root, depth, res):
        if root:
            if depth >= len(res):
                res.insert(0, [])
            res[-(depth+1)].append(root.val)
            self.dfs(root.left, depth+1, res)
            self.dfs(root.right, depth+1, res)
```
这几种方式在速度上没有太多差别，都是40ms
