# [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

**思路：首先确定是否有环，如果有环，当相遇时，让fast指向链表头，slow不动，之后fast每次与slow一致都走一步，下次相遇就是在环的入口位置处**



数学证明：假设链表有环，且环的入口到链表的起点有d个节点，从环的入口到第一次相遇的那个位置有m个节点，从第一次相遇的位置再走回到换的入口处有n个节点，

第一次相遇时：慢指针走了：d + k *(m+n) + m，因为快指针走两步，慢指针走一步，所以快指针走的路程是慢指针的2倍，那么快指针走了2 * d + 2 * k * (m + n) + m，相减得到d + m + k * (m + n)，其实此时快指针比慢指针多走了几圈环，所以d + m 其实也是一个环长度，也就是相当于得到`d = n`，从而我们可以让快指针此时指向链表头，此时让快慢每次都走一步，那么下次相遇一定在环的入口处

> 执行用时：4 ms, 在所有 Go 提交中击败了98.94%的用户
> 		内存消耗：3.8 MB, 在所有 Go 提交中击败了100.00%的用户

```go
func detectCycle(head *ListNode) *ListNode {
	//没有环，可以不用写，因为后面会执行
	//if head == nil {
	//	return nil
	//}

	fast, slow := head, head
	for fast != nil && fast.Next != nil {
		fast, slow = fast.Next.Next, slow.Next
		if fast == slow { //找到了相遇点
			fast = head
			break
		}
	}

	//说明没有环
	if fast == nil || fast.Next == nil {
		return nil
	}

	//再次制造机会让他们相碰撞，此时会在环的入口节点处碰撞
	for slow != fast {
		slow, fast = slow.Next, fast.Next
	}

	return slow

}

```

