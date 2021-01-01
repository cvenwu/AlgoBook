# [369.给单链表加一](https://leetcode-cn.com/problems/plus-one-linked-list/)

## 方法一：反转

!> 思路：反转链表，加完之后有进位，然后反转回去

```go
//反转链表，加一之后反转回去即可
func plusOne(head *ListNode) *ListNode {

	//反转链表.返回反转后的头和尾
	start, end := reverseLinkedList(head)

	//进行加一
	cur := start
	carry := 1
	for cur != nil {
		cur.Val, cur, carry = (cur.Val+carry)%10, cur.Next, (cur.Val+carry)/10
	}

	if carry != 0 {
		end.Next = &ListNode{
			Val: carry,
		}
	}

	//反转回去
	start, end = reverseLinkedList(start)
	return start
}

func reverseLinkedList(head *ListNode)(*ListNode, *ListNode) {
	if head == nil {
		return nil, nil
	}

	var prev *ListNode
	cur := head

	for cur != nil {
		prev, cur, cur.Next = cur, cur.Next, prev
	}

	return prev, head
}
```

## 方法二：递归

```go
func plusOne(head *ListNode) *ListNode {
	ret, val := plusOneCore(head)
	if val != 0 {
		temp := &ListNode{Val: val}
		temp.Next = ret
		return temp
	}
	return ret
}

func plusOneCore(head *ListNode) (*ListNode, int) {
	if head == nil {
		return nil, 0
	}

	var val int
	if head.Next == nil {
		head.Val, val = (head.Val+1)%10, (head.Val+1)/10
		return head, val
	}

	_, val = plusOneCore(head.Next)
	head.Val, val = (head.Val+val)%10, (head.Val+val)/10

	return head, val
}
```

## 【推荐】方法三：使用快慢指针

!> 核心思路：如果出现了9，那么找到最后一连串9的开头的前一个，如果最后面只有1个单独的9，那么找到9的前一位，然后加1，将后面单独的9变成0即可

!> [参考他人解法](https://leetcode-cn.com/problems/plus-one-linked-list/solution/c-kuai-man-zhi-zhen-bu-fan-zhuan-lian-biao-by-kao-/)
