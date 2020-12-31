# [1669. 合并两个链表](https://leetcode-cn.com/problems/merge-in-between-linked-lists/)




## 方法一

思路：
	1. 找到l1的a的前一个节点prev以及l1的b的后一个节点next
	2. 找到l2的尾部的节点tail
	3. 然后将prev的next指向list2，将tail的尾部指向next
!> 注意：由于我们可能从第1个节点开始就不要，所以我们需要建立dummyNode


```go
//执行用时：116 ms, 在所有 Go 提交中击败了97.00%的用户
//内存消耗：7.6 MB, 在所有 Go 提交中击败了36.00%的用户
func mergeInBetween(list1 *ListNode, a int, b int, list2 *ListNode) *ListNode {

	//建立dummyNode
	dummyNode := &ListNode{}
	dummyNode.Next = list1

	//声明我们后面需要用到的变量
	var prev, next *ListNode

	cur := dummyNode
	//计数用来找到a和b
	count := 0
	//找到prev
	for count != a {
		cur = cur.Next
		count++
	}
	prev = cur

	//找到next,注意这里因为我们从dummyNode开始所以得都走一步，同时下一个节点为cur.Next
	for count != b+1 {
		cur = cur.Next
		count++
	}
	next = cur.Next

	//找到第2个链表的尾部
	tail2 := list2
	for tail2.Next != nil {
		tail2 = tail2.Next
	}

	//直接将两个链表连接起来
	prev.Next, tail2.Next = list2, next



	return dummyNode.Next
}
```