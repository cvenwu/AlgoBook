# [234. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)

## 方法：双指针+反转链表

> 执行用时：12 ms, 在所有 Go 提交中击败了96.94%的用户
> 		内存消耗：5.6 ms, 在所有 Go 提交中击败了100.00%的用户

思路：

```
1. fast为快指针，slow为慢指针，不断移动，移动过程中slow不断进行反转链表
2. 当fast指向链表最后一个节点或者fast为nil的时候就停止，此时prev为我们反转的前一半链表的表头
      2.1 如果有奇数个节点，fast指向最后一个节点，slow指向中间的链表正中间的节点，
      2.2 ，如果有偶数个节点，fast指向nil，slow指向链表中间第2个节点)
3. 此时如果有奇数个节点我们需要将slow后移一个单位
4. 将prev赋值给fast
5. 之后不断循环比较slow与fast，并进行移动
```

```go
func isPalindrome(head *ListNode) bool {

	//首先检查是否为空链表
	if head == nil {
		return true
	}

	fast, slow := head, head
	prev := head

	//让fast走到链表尾部的时候，(如果有奇数个节点，fast指向最后一个节点，如果有偶数个节点，fast指向nil)
	//slow指向链表的中间(如果有奇数个节点则为中间，如果是偶数个节点，则是中间第2个节点)
	for fast != nil && fast.Next != nil {
		fast, prev, slow, slow.Next = fast.Next.Next, slow, slow.Next, prev
	}

	//说明有奇数个节点，此时slow指向中间节点，我们需要将它后移一个单位
	if fast != nil {
		slow = slow.Next
	}

	//此时prev指向前半段反转完的头节点
	fast = prev
	//此时不断的从两个链表的开始进行比较，如果中间有不等就直接返回false
	for fast != nil && slow != nil {
		if fast.Val != slow.Val {
			return false
		}

		fast, slow = fast.Next, slow.Next
	}

	return true

}
```

