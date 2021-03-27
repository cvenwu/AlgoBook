# 合并两个排序的链表

## 方法一：

!> 技巧：因为不确定链表的头结点是两个链表中的哪一个，所以我们可以先建立一个虚拟头节点

```go
func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
	dummyNode := &ListNode{}
	cur := dummyNode

	for l1 != nil && l2 != nil {
		if l1.Val > l2.Val {
			cur.Next, l2 = l2, l2.Next
			cur = cur.Next
		} else {
			cur.Next, l1 = l1, l1.Next
			cur = cur.Next
		}
	}

	if l1 != nil {
		cur.Next = l1
	}

	if l2 != nil {
		cur.Next = l2
	}

	return dummyNode.Next
}
```


!> 2021-03-27更新代码：

```go
func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {

	dummyNode := &ListNode{}
	cur := dummyNode
	cur1, cur2 := l1, l2
	for cur1 != nil && cur2 != nil {
		if cur1.Val < cur2.Val {
			cur.Next, cur1.Next, cur1 = cur1, nil, cur1.Next
		} else {
			cur.Next, cur2.Next, cur2 = cur2, nil, cur2.Next
		}

		cur = cur.Next
	}

	if cur1 != nil {
		cur.Next = cur1
	}

	if cur2 != nil {
		cur.Next = cur2
	}

	return dummyNode.Next
}
```