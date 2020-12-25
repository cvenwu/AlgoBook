# [24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

## 方法：


> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.1 MB, 在所有 Go 提交中击败了10.26%的用户


思路：直接交换，步骤如下，**前提条件，如果当前节点以及当前节点的下一个节点都不为空：**

1. 首先如果当前节点的上一个节点不空，将上一个节点的指针指向当前节点的下一个节点
2. 将当前节点的指针指向下下个节点，同时当前节点的下下个节点指针指向当前节点，同时移动prev到当前节点，同时将当前节点移动到下下个节点


```go
func swapPairs(head *ListNode) *ListNode {

	if head == nil || head.Next == nil {
		return head
	}

	prev, cur := head, head
	prev = nil
	ret := head.Next

	for cur != nil && cur.Next != nil {
		if prev != nil {
			prev.Next = cur.Next
		}
		cur.Next, cur.Next.Next, prev, cur = cur.Next.Next, cur, cur, cur.Next.Next
	}

	return ret

}
```

