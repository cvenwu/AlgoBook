# [206.反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

## 方法一：迭代

```go
func reverseList(head *ListNode) *ListNode {
	var prev *ListNode
	cur := head

	for cur != nil {
		prev, cur.Next, cur = cur, prev, cur.Next
	}
	return prev
}
```

## 方法二：递归
!> 思路：新建一个函数，返回反转后的链表的头和尾

```go

//方法二：递归
func reverseList(head *ListNode) *ListNode {
	if head == nil {
		return nil
	}
	tempHead, _ := reverListCore(head)

	return tempHead
}

//返回反转后的链表的头和尾
func reverListCore(head *ListNode) (*ListNode, *ListNode) {

	if head.Next == nil {
		return head, head
	}
	tempHead, tempTail := reverListCore(head.Next)
	tempTail.Next, head.Next = head, nil
	return tempHead, head
}
```

## 方法三：对方法二的改进
!> 递归函数的含义就是返回以head为头反转之后的链表的头节点

思路：因为递归之后反转后的链表是当前head的Next指向的节点，所以我们可以直接让head.Next.Next 指向head，让后置空head.Next

```go

func reverseList(head *ListNode) *ListNode {
	if head == nil || head.Next == nil{  //注意：要写上head.Next!=nil 避免后面的head.Next.Next报错
		return head
	}
	reverseHead := reverseList(head.Next)
	//反转之后head.Next指向后面反转后链表的最后一个节点，但是也是head要插入的位置的前一个节点
	head.Next.Next, head.Next = head, nil

	return reverseHead
}

```