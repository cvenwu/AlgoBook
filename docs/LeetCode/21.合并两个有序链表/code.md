# [21.合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)


## 方法一：

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
    head1, head2 := l1, l2
    if head1 == nil {
		return head2
	}

	if head2 == nil {
		return head1
	}

	//建立dummyNode
	dummyNode := &ListNode{}

	cur, l1, l2 := dummyNode, head1, head2
	for l1 != nil && l2 != nil {
		if l1.Val < l2.Val {
			cur.Next, l1, cur, l1.Next = l1, l1.Next, l1, nil
		} else {
			cur.Next, l2, cur, l2.Next = l2, l2.Next, l2, nil
		}
	}

	if l1 == nil {
		cur.Next = l2
	}

	if l2 == nil {
		cur.Next = l1
	}

	return dummyNode.Next
}
```