# [147.对链表插入排序](https://leetcode-cn.com/problems/insertion-sort-list/)

## 方法一

!> 直接按照题目的策略进行模拟即可

思路：![IMG_0380](https://cdn.jsdelivr.net/gh/sivanWu0222/ImageHosting@master/uPic/IMG_0380.jpg)

```go
//执行用时：8 ms, 在所有 Go 提交中击败了47.71%的用户
//内存消耗：3.2 MB, 在所有 Go 提交中击败了100.00%的用户
func insertionSortList(head *ListNode) *ListNode {
	if head == nil {
		return head
	}

	dummyNode := &ListNode{}
	dummyNode.Next = head
	cur := head.Next
	var next, prev *ListNode
	lastOrderNode := head
	//注意点1：这个如果不设置为空，很有可能之前lastOrderNode的下一个节点就插入到前面，此时将会形成一个环，因为前面那个节点的next指向自己，而之前我们lastOrderNode的next指向该节点，并没有设置为空
	lastOrderNode.Next = nil
	for cur != nil {
		next = cur.Next
		if cur.Val >= lastOrderNode.Val {
			lastOrderNode.Next, cur, lastOrderNode = cur, next, cur
			//注意点2：原因同注意点1
			lastOrderNode.Next = nil
			continue
		}

		//从头找prev
		prev = dummyNode
		for prev.Next.Val < cur.Val {
			prev = prev.Next
		}
		prev.Next, cur, cur.Next = cur, next, prev.Next
	}

	return dummyNode.Next
}
```