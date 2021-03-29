# [剑指 Offer 35. 复杂链表的复制](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)

## 方法一：暴力
1. 时间复杂度为O(N^2)
2. 空间复杂度O(1)

## 方法二：使用一个哈希表映射<原节点，新复制的节点>
1. 时间复杂度为O(N)
2. 空间复杂度O(N)

## 【推荐】方法三
**时间复杂度为O(N)**
**空间复杂度O(1)**

​步骤：
1.  将每个节点复制到后面，同时random指向之前源指针的指向
2.  修正每个新添加节点的random指向。注意：最后两个节点处理要特殊
3.  将两个链表提取出来

!> 注意：原链表中有的节点的random会指向nil，所以要if判断

```go

type Node struct {
	Val    int
	Next   *Node
	Random *Node
}

func copyRandomList(head *Node) *Node {

	//注意点1：head为空的情况，否则后面拆分的时候容易panic
	if head == nil {
		return nil
	}

	head = copyList(head)
	var newHead *Node
	head, newHead = splitList(head)
	return newHead
}

//第一步：复制链表并修改random指向
func copyList(head *Node) *Node {
	cur := head
	for cur != nil {
		node := &Node{
			Val:  cur.Val,
			Next: cur.Next,
		}
		cur.Next, cur = node, cur.Next
	}

	//修改random指向的域
	cur = head
	for cur != nil {
		//因为random可能为空
		if cur.Random != nil {
			cur.Next.Random = cur.Random.Next
		}
		cur = cur.Next.Next
	}
	return head
}

//第二步：将复制之后的链表拆分成两部分
func splitList(head *Node) (*Node, *Node) {
	cur, cur1 := head, head.Next

	for cur != nil && cur.Next != nil{
		//注意点2：一定要记得让cur不断移动
		cur.Next,cur = cur.Next.Next,cur.Next
	}

	return head, cur1
}

```