# 237 删除链表中的节点


## 方法一：
思路：

1. 因为题目中说明了 传入的是要删除的节点，并且保证这个要删除的节点不是最后一个节点。也就是当前节点的Next不为空。所以我们可以下一个节点的值替换到当前节点，
2. 之后将当前节点的Next 指向当前节点的Next的Next




```go
// 执行用时：4 ms, 在所有 Go 提交中击败了70.15%的用户
// 内存消耗：2.9 MB, 在所有 Go 提交中击败了59.74%的用户
func deleteNode(node *ListNode) {
	node.Val, node.Next = node.Next.Val, node.Next.Next
}
```

