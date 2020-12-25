# [92. 反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)



## 方法一：

要反转第m个节点到第n个节点这一段。

1. 找到4个节点：第m个节点的前一个节点prev，第m个节点start，第n个节点end，第n个节点的下一个节点next。

2. 然后prev以及end的Next指针置空
3. 反转start到end之间所有的节点
4. 将prev的Next指向反转后的头end，将反转后的尾start的Next指向next
5. 最后返回我们链表的头结点即可

注意：由于反转可能从开头节点开始，因此我们需要使用一个虚拟节点。



~~错误写法：~~

```go

//❌代码：
//错误点：返回的结果错误，因为我们建立了一个虚拟头节点(避免从头部第1个节点开始进行翻转)，因此我们应该返回虚拟头节点的下一个节点
func reverseBetween(head *ListNode, m int, n int) *ListNode {
	//建立dummyNode为了防止删除头节点
	dummyNode := &ListNode{}
	dummyNode.Next = head

	//从dummyNode走m-1步，此时来到反转开始节点的前一个节点，
	cur := dummyNode
	for i := 0; i < m-1; i++ {
		cur = cur.Next
	}

	prev := cur
	start := cur.Next

	//接下来继续走n-m+1步，来到当前节点为end,
	for i := 0; i < n-m+1; i++ {
		cur = cur.Next
	}
	end := cur
	next := cur.Next

	//将要反转链表节点的最后一个以及开始节点前面节点的next置空
	prev.Next, end.Next = nil, nil
	//开始反转链表的节点

	var temp *ListNode
	cur = start
	for cur != nil {
		temp, cur, cur.Next = cur, cur.Next, temp
	}

	//开始连接反转后的链表

	prev.Next, start.Next = end, next


	return head
}


```



**正确写法：**

```go

//✅
//执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
//内存消耗：2.1 MB, 在所有 Go 提交中击败了86.23%的用户
//改正：只修改了返回值
func reverseBetween(head *ListNode, m int, n int) *ListNode {

	//建立dummyNode
	dummyNode := &ListNode{}
	dummyNode.Next = head

	//从dummyNode走m-1步，此时来到反转开始节点的前一个节点，
	cur := dummyNode
	for i := 0; i < m-1; i++ {
		cur = cur.Next
	}

	prev := cur
	start := cur.Next

	//接下来继续走n-m+1步，来到当前节点为end,
	for i := 0; i < n-m+1; i++ {
		cur = cur.Next
	}
	end := cur
	next := cur.Next

	//将要反转链表节点的最后一个以及开始节点前面节点的next置空
	prev.Next, end.Next = nil, nil
	//开始反转链表的节点

	var temp *ListNode
	cur = start
	for cur != nil {
		temp, cur, cur.Next = cur, cur.Next, temp
	}

	//开始连接反转后的链表
	prev.Next, start.Next = end, next

	return dummyNode.Next
}

```

