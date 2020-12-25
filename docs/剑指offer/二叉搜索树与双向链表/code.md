# [剑指 Offer 36. 二叉搜索树与双向链表](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)



> 由于Leetcode没有Go的提交，所以使用了python

## 方法：

思路：对树进行中序遍历，正好是按照升序的。

在中序遍历过程中，我们做如下几件事情：

1. 记录上一次访问过的节点，此时我们要让当前节点的左指针指向上一次访问节点，并且上一次访问节点的右指针指向当前节点
2. 记录我们访问的第一个节点，是链表的头节点，到时候我们要进行返回，
3. 当中序遍历完成之后，此时上一个节点指向的便是右子树的最右节点，也是我们想要的那个节点，但是此时我们要让head与上一个节点（当前指向右子树的最右节点）进行连接。



```python
class Solution:
    def treeToDoublyList(self, root: 'Node') -> 'Node':
        if root is None:
            return None
        
        self.head, self.prev = None, None

        def dfs(root, head, prev):
            if root.left is not None:
                dfs(root.left, self.head, self.prev)
            
            if self.head is None:
                self.head = root
            else: # 说明此时不是第1个节点
                root.left, self.prev.right = self.prev, root
            self.prev = root
            if root.right is not None:
                dfs(root.right, self.head, self.prev)

        dfs(root, self.head, self.prev)
        # 此时head指向头节点，prev指向最后一个节点
        self.head.left, self.prev.right = self.prev, self.head
        return self.head
```

