#  [143.重排链表](https://leetcode-cn.com/problems/reorder-list/)

## 方法一

!> 思路：每次交换一个元素前要找到对应元素，时间复杂度O(N^2)


## 方法二
思路：首先遍历一次，然后使用哈希表存Key-val，其中key为对应的索引，val为对应的节点，同时获取链表的长度为l
而后遍历链表中的每个节点，并记录索引，若节点后没有元素就返回，否则找到第l-i个节点，当前节点的next指向第l-i个元素，同时第l-i个节点的next指向第i个节点，最后返回

!> 时间复杂度O(N)，空间复杂度O(N)

## 方法三：
!> 使用快慢指针找到链表的后半部分，然后反转链表的后半部分并独立出来，而后再将这两个链表相互交替合并

!> 时间复杂度O(N)，空间复杂度O(1)


```go
package main

/**
 * @Author: yirufeng
 * @Date: 2020/12/28 11:27 上午
 * @Desc:
 **/


/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */

//执行用时：12 ms, 在所有 Go 提交中击败了77.59%的用户
//内存消耗：5.3 MB, 在所有 Go 提交中击败了80.24%的用户
func reorderList(head *ListNode) {
	//如果头结点为空直接返回
	if head == nil {
		return
	}

	var prev *ListNode
	slow, fast := head, head
	for fast != nil && fast.Next != nil {
		prev, slow, fast = slow, slow.Next, fast.Next.Next
	}
	//此时slow指向奇数个节点中的中间节点，fast指向偶数个节点中的第2个中间节点，prev指向slow的前一个节点
	prev.Next = nil //断开连接
	//反转从slow开始的节点
	prev = nil
	for slow != nil {
		prev, slow, slow.Next = slow, slow.Next, prev
	}

	//反转后的链表的头部为prev
	dummyNode := &ListNode{}
	dummyNode.Next = head

	cur, cur1, cur2 := dummyNode, head, prev
	for cur1 != nil && cur2 != nil {
		cur.Next, cur1.Next, cur2.Next, cur1, cur2, cur = cur1, cur2, nil, cur1.Next, cur2.Next, cur2
	}

	if cur1 != nil {
		cur.Next = cur1
	}

	if cur2 != nil {
		cur.Next = cur2
	}
}
```