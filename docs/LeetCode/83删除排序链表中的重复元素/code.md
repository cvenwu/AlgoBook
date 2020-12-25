# [83. 删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

## 方法一：

思路：直接删除

1. 如果当前节点的值与下一个节点的值相同，则将当前节点指向下一个节点的下一个节点
2. 如果当前节点的值与下一个节点的值不同，
  1. 则将当前节点移动到下一个节点
  2. **同时记得清除当前节点的下一个节点的野指针**，让其指向nil
3. 重复步骤1，2直到当前节点的下一个节点为Nil

> 执行用时：4 ms, 在所有 Go 提交中击败了86.85%的用户
> 		内存消耗：3.1 MB, 在所有 Go 提交中击败了61.54%的用户

```go
func deleteDuplicates(head *ListNode) *ListNode {
	if head == nil {
		return head
	}
	node := head
	for node.Next != nil {
		if node.Val == node.Next.Val {
			//记得清楚野指针
			node.Next, node.Next.Next = node.Next.Next, nil
		} else {
			node = node.Next
		}
	}

	return head
}
```

