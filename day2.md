**142**
![image.png](https://upload-images.jianshu.io/upload_images/5220317-79344b7b74090689.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
    def detectCycle(self, head):
        """
        head 是链表节点
        :type head: ListNode
        :rtype: ListNode
        """
        # 遍历一次，看下next的指向是否曾出现过，tail.next即为所求
        #history = []
        # list换set能得到10倍的速度提升
        history = set()
        # 若成环 head.next 永不为None -> 为None则无环
        # 必须加上head ： 对于无环情况，走到终点时，head=head.next,此时的head=None
        if head and head.next == None:
            return None
        while head:
            # 若某个节点曾出现，则定为 tail 所指
            if head in history:
                return head
            # 记录遇到的所有节点
            #history.append(head)
            history.add(head)
            head = head.next
```
同样是记录next节点的方式，稍加改动，站在head.next角度来看问题
```
    def detectCycle(self, head):
        """
        head 是链表节点
        :type head: ListNode
        :rtype: ListNode
        """
        # 遍历一次，看下next的指向是否曾出现过，tail.next即为所求
        #history = []
        # list换set能得到10倍的速度提升
        history = set()
        # 若成环 head.next 永不为None -> 为None则无环
        # 必须加上head ： 对于无环情况，走到终点时，head=head.next,此时的head=None
        if head == None:
            return None
        while head.next:
            # 若某个节点曾出现，则定为 tail 所指
            if head in history:
                return head
            # 记录遇到的所有节点
            #history.append(head)
            history.add(head)
            head = head.next
```
快慢指针的解释：https://blog.csdn.net/willduan1/article/details/50938210
![image.png](https://upload-images.jianshu.io/upload_images/5220317-edf3151db448c9bc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
不怎么刷过题的话，空想会有点乱，画个图就很好理解啦
```
    def detectCycle(self, head):
        """
        用快慢指针做追及问题
        :type head: ListNode
        :rtype: ListNode
        """
        fast = head
        slow = head
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
            if fast is slow:
                # 相遇的时候重置fast,且设置fast和slow同速度，再次相遇即为环的起点
                fast = head
                while fast is not slow:
                    fast, slow = fast.next, slow.next
                return fast
```
**206**
最容易想到的就是递归和后入先出的栈w
![](https://upload-images.jianshu.io/upload_images/5220317-f0c7b64b56bbf9a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
    def reverseList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        if not head or not head.next:
            return head
        p = head.next
        newhead = self.reverseList(p)
        
        head.next = None
        p.next = head
        
        return newhead
```
