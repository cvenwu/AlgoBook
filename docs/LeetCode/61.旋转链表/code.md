# [61.旋转链表](https://leetcode-cn.com/problems/rotate-list/)

## 方法一


思路：其实等价于，找第i个节点是移动k步后的链表的头，然后将前后两个链表分离开，将前半部分的链表追加到后半部分的链表上(以第i个节点为头)

> 相当于第i个节点分成两部分，第二部分头为第i个节点，将第一部分的链表加入到第二部分的尾部即可

!> 因为第i个节点移动k%n步之后会移动到头，因此找倒数第k%n个节点即可

步骤：
1. 获取链表长度n以及链表的最后一个节点last
2. 再次遍历链表，找到倒数第k%n个节点node以及上一个节点prev(只需要遍历一次链表即可，也就是找到倒数第k%n个节点的前一个节点)，将前一个节点的Next置为空，划分为两部分，然后将后半部分Next指向前一部分，然后返回后半部分的头


```go

//执行用时：4 ms, 在所有 Go 提交中击败了66.82%的用户
//内存消耗：2.6 MB, 在所有 Go 提交中击败了100.00%的用户
func rotateRight(head *ListNode, k int) *ListNode {
	if head == nil || k <= 0 {
		return head
	}

	//记录链表中的节点个数
	n := 1
	var prev, last, node *ListNode
	cur := head
	//遍历一次链表
	for cur.Next != nil {
		n++
		cur = cur.Next
	}

	//此时cur指向了我们链表的最后一个节点
	//同时我们获取了链表的长度n
	last = cur

	//表示如果k是n的倍数则说明旋转之后还是和原链表一样的，因此直接返回即可
	if k%n == 0 {
		return head
	}

	//找倒数第k%n个节点以及前一个节点
	fast, slow := head, head
	//fast先走k%n + 1步
	for i := 0; i < k%n+1; i++ {
		fast = fast.Next
	}

	//然后两个节点一起走
	for fast != nil {
		fast, slow = fast.Next, slow.Next
	}

	//此时slow指向的是倒数第k%n个节点的前一个节点，
	prev, node = slow, slow.Next

	//将前面的链表与后面的断开，然后加在后半部分链表的尾巴上
	prev.Next, last.Next = nil, head
	return node

}

```


!> 对上述方法的改进，只需要走O(n + 倒数第k%n个几点的前一个节点所对应的位置) 2021-01-24
```go

func rotateRight(head *ListNode, k int) *ListNode {
	if head == nil || k <= 0 {
		return head
	}

	n := 1
	cur := head
	for cur.Next != nil {
		n, cur = n + 1, cur.Next
	}

	//说明移动之后依然是原链表直接返回即可
	if k % n == 0 {
		return head
	}

	tail := cur

	//找到倒数第k%n个节点，以及前一个节点
	cur = head
	for i := 0; i < n - k%n - 1; i++ {
		cur = cur.Next
	} //此时cur指向倒数第k%n个节点的前一个节点

	//将前一个节点的下一个节点赋值给ret
	ret := cur.Next

	//前一个节点的Next置空，同时让第二部分链表的尾部指向头部
	cur.Next, tail.Next = nil, head

	//返回第二部分链表的头部
	return ret
}

```

## 其他方法
!> 参考别人方法：遍历一次链表到末尾，然后将最尾部的节点Next指向到链表头部，然后找到倒数第K%n个节点的前一个节点，然后将该节点的Next与后面的断开，并返回倒数第k%n个节点

