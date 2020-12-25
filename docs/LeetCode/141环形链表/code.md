# [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)



## 方法一：使用hashset

思路：使用hashset存储走过的节点，如果下次走到的节点在hashset中说明有环



## 方法二：快慢指针

> 思路：快指针一次走两步，慢指针一次只走一步

> 执行用时：4 ms, 在所有 Go 提交中击败了98.94%的用户
> 		内存消耗：3.8 MB, 在所有 Go 提交中击败了100.00%的用户

```go
func hasCycle(head *ListNode) bool {
	//if判断可以省略，因为下面的for第一步就会判断
	// if head == nil {
	//     return false
	// }

	fast, slow := head, head

	for fast != nil && fast.Next != nil {
		fast, slow = fast.Next.Next, slow.Next
		//这个if一定要放到下面，如果放在上面，刚开始fast和slow都为head，会报错
		if fast == slow {
			return true
		}
	}

	return false
}
```

