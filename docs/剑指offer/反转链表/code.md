# 反转链表

## 方法一：迭代反转

```go
func reverseList(head *ListNode) *ListNode {
	var prev *ListNode
	cur := head
	for cur != nil {
		cur, cur.Next, prev = cur.Next, prev, cur
	}

	return prev
}
```

## 方法二：递归反转
```go
func reverseList(head *ListNode) *ListNode {
	if head == nil {
		return nil
	}
	reverHead := reverseList(head.Next)
	if reverHead == nil {
		return head
	}
	head.Next.Next, head.Next = head, nil
	return reverHead
}
```