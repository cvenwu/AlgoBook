# [203. 移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)

## 方法一：通过建立哨兵节点统一删除所有节点的操作

思路：有些人可能一眼觉得如果删除的是头结点怎么办，其实我们通过一个dummyNode将所有节点的删除进行了统一。


> 执行用时：8 ms, 在所有 Go 提交中击败了89.78%的用户
> 内存消耗：4.7 ms, 在所有 Go 提交中击败了13.17%的用户


```go
func removeElements(head *ListNode, val int) *ListNode {
	if head == nil {
		return nil
	}

	dummyNode := &ListNode{Next: head}

	cur := dummyNode
	for cur.Next != nil {
		if cur.Next.Val == val {
			cur.Next = cur.Next.Next
		} else {
			cur = cur.Next
		}
	}

	return dummyNode.Next
}
```
