# [19. 删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

## 方法：保持固定距离的双指针 + 哨兵节点

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.3 ms, 在所有 Go 提交中击败了5.78%的用户

因为我们找的是要删除节点的前一个节点。

```
思路：关键是要保持快慢两个指针的距离，因为快指针指向最后一个Nil的时候，慢指针指向倒数第n+1个节点，距离为n+1
1. 建立一个dummyNode, 让fast, slow 都指向这个节点
2. 让快指针先走n+1步
3. 之后快慢同时走，当快走向nil的时候，慢指向要删除节点的前一个节点
4. 修改指针指向并返回dummyNode.Next
```

**我们找的是要删除节点的前一个节点，因此使用哨兵节点是为了怕删除头节点**

思路：关键是要保持快慢两个指针的距离，因为快指针指向最后一个Nil的时候，慢指针指向倒数第n+1个节点，距离为n+1。

1. 建立一个dummyNode, 让fast, slow 都指向这个节点
2. 让快指针先走n+1步
3. 之后快慢同时走，当快走向nil的时候，慢指向要删除节点的前一个节点
4. 修改指针指向并返回dummyNode.Next

```go
func removeNthFromEnd(head *ListNode, n int) *ListNode {
	if head == nil {
		return nil
	}

	dummyNode := &ListNode{Next: head}

	fast, slow := dummyNode, dummyNode
	for i := 0; i < n+1; i++ {
		fast = fast.Next
	}

	for fast != nil {
		fast, slow = fast.Next, slow.Next
	}

	//此时fast指向最后一个节点的下一个
	//slow指向要删除节点的前一个节点
	//if slow.Next != nil {
	//注意：这里不用加判断，因为即使只有1个节点，我们的slow也会指向dummyNode,执行slow.Next.Next不会报错，并且slow不可能走到最后一个节点，因为我们的slow是倒数第n个节点的前一个节点
	slow.Next = slow.Next.Next
	//}

	return dummyNode.Next
}

```

