# 删除链表的节点

思路：

1. 如果要删除的节点是头节点，直接返回头节点的下一个节点
2. 如果删除的是中间节点或者尾节点，通过遍历找到要删除节点的上一个节点，将指针指向要删除节点的下一个节点

## 方法一
> *剑指offer函数参数传入了要删除的节点，因此不用再去遍历一次去确认要删除的节点是否在末尾*

> *// 执行用时：4 ms, 在所有 Go 提交中击败了 75.15% 的用户*
>
> *// 内存消耗：2.9 MB, 在所有 Go 提交中击败了 100.00% 的用户*

```go
func deleteNode(head *ListNode, val int) *ListNode {
	//首先判断链表是否为空
	if head == nil {
		return nil
	}

	//如果删除头节点
	if head.Val == val {
		//如果链表只有1个节点
		if head.Next == nil {
			return nil
		} else {
			node := head.Next
			head.Next = nil
			return node
		}
	}

	node := head
	//否则找到要删除的节点的前一个节点
	for node.Next != nil && node.Next.Val != val {
		node = node.Next
	}
	//如果没找到
	if node.Next == nil {
		return head
	}

	//说明找到了
	if node.Next.Next != nil {  //说明删除的不是尾节点
		node.Next = node.Next.Next
	} else {	//说明要删除的是尾节点
		node.Next = nil
	}
	return head
}
```
## 【推荐】方法：自己的简单解法
```go
func deleteNode(head *ListNode, val int) *ListNode {
	dummyNode := &ListNode{}
	dummyNode.Next = head
	cur := dummyNode

	for cur.Next != nil {
		if cur.Next.Val == val {
			cur.Next = cur.Next.Next
			break
		}
		cur = cur.Next
	}
	
	return dummyNode.Next
}
```