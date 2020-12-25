# [剑指 Offer 35. 复杂链表的复制](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)

## 方法一：暴力

时间复杂度为O(N^2)

空间复杂度O(1)

## 方法二：使用一个哈希表映射<原节点，新复制的节点>

时间复杂度为O(N)

空间复杂度O(N)

## 方法三【推荐】：时间复杂度为O(N)空间复杂度O(1)


​	步骤：
 	1.  将每个节点复制到后面，同时random指向之前源指针的指向
	2.  修正每个新添加节点的random指向。注意：最后两个节点处理要特殊
	3.  将两个链表提取出来，注意：最后两个节点处理要特殊

>  执行用时：0 ms, 在所有 Go 提交中击败了 100.00% 的用户
> 		内存消耗：3.4 MB, 在所有 Go 提交中击败了 40.65% 的用户

```go

type Node struct {
	Val    int
	Next   *Node
	Random *Node
}


func copyRandomList(head *Node) *Node {
	//如果链表为空
	if head == nil {
		return nil
	}

	//第1步：在每个节点后边建立一个节点，并连接起来
	cur := head
	for cur != nil {
		cur.Next = &Node{
			Val:    cur.Val,
			Next:   cur.Next,
			Random: cur.Random,
		}
		cur = cur.Next.Next
	}

	//第2步：将新建立的每个节点的Random指针指向正确方向
	cur = head.Next
	for cur != nil {
		if cur.Random != nil {
			cur.Random = cur.Random.Next
		}

		//如果是最后一个节点
		if cur.Next != nil {
			cur = cur.Next.Next
		} else {
			break
		}
	}

	//第3步：将奇数节点与偶数节点对应的两个链表分开
	cur = head.Next
	odd, even := head, head.Next
	for odd != nil {
		if even.Next != nil {
			odd.Next, even.Next = odd.Next.Next, even.Next.Next
			odd, even = odd.Next, even.Next
		} else { 		//如果最后两个节点
			odd.Next = odd.Next.Next
			break
		}
	}

	return cur
}

```

