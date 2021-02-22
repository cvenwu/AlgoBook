# [160. 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)


## 方法一：
>思路：长链表走两个链表的长度差，然后不断判断两个不断移动的节点是否相等，如果相等直接返回该节点，如果走到了最后，说明没有相交

注意：没有判断循环是因为题目中说了链表没有循环

```go

func getIntersectionNode(headA, headB *ListNode) *ListNode {

	cur1, cur2 := headA, headB
	l1, l2 := 0, 0
	//统计第一个链表的长度
	for cur1 != nil {
		cur1, l1 = cur1.Next, l1+1
	}
	//统计第二个链表的长度
	for cur2 != nil {
		cur2, l2 = cur2.Next, l2+1
	}

	cur1, cur2 = headA, headB

	if l1 > l2 {
		//l1先走
		for i := 0; i < l1-l2; i++ {
			cur1 = cur1.Next
		}
	} else {
		//l2先走
		for i := 0; i < l2-l1; i++ {
			cur2 = cur2.Next
		}
	}

	for cur1 != nil && cur1 != cur2{
		cur1, cur2 = cur1.Next, cur2.Next
	}

	//如果是因为走到了尾巴结束，则说明没有相交节点
	if cur1 == nil {
		return nil
	}
	return cur1
}

```

## 方法二：
!> 思路：一个链表走完了开始走另外一个链表，如果两个链表走到最后都没有相遇，直接返回nil
> 技巧：使用一个flag标志进行判断，如果flag大于等于3说明有一个链表被走了三次，所以肯定不相交
```go
func getIntersectionNode(headA, headB *ListNode) *ListNode {

	cur1, cur2 := headA, headB
	flag := 0
	for cur1 != nil && cur2 != nil {
		if flag >= 3 {
			return nil
		}
		if cur1 == cur2 {
			return cur1
		}
		cur1, cur2 = cur1.Next, cur2.Next
		if cur1 == nil {
			cur1 = headB
			flag++
		}
		if cur2 == nil {
			cur2 = headA
			flag++
		}
	}

	return nil
}
```
## 【推荐】方法二的改进：

**关键：如何弥补长度差，我们可以让两个指针遍历完自己所在的链表之后再遍历另一个链表**


> 执行用时：40 ms, 在所有 Go 提交中击败了97.34%的用户
> 		内存消耗：7.4 MB, 在所有 Go 提交中击败了81.51%的用户

使用双指针，让两个指针同时走自己的链表，走完自己的链表之后走另外一个链表，这样就可以弥补长度差。

最后的退出条件说明：如果两个链表指向了同一个节点，说明该节点就是相交的起始节点，也有可能两个链表都走到最后(如果两个链表有长度差，那么弥补完长度差之后如果不相交也都会指向nil)且都为空，此时也会退出循环


```go
func getIntersectionNode(headA, headB *ListNode) *ListNode {

	//如果有一个为空，直接返回nil
	if headA == nil || headB == nil {
		return nil
	}

	tempHeadA, tempHeadB := headA, headB
	//为什么这么写而不使用标志位，因为之前自己使用标志位怕的就是死循环，但是其实没有必要用标志位，
	//因为第一个链表或第二个链表走完自己都会走另外一个链表，
	//所以各自走另外一个链表的时候，如果相交中间就会相等直接返回
	//如果不相交，走到最后两个都同时为nil，因为各自走另外一个链表屏蔽了长度差
	for tempHeadA != tempHeadB {
		if tempHeadA == nil {
			tempHeadA = headB
		} else {
			tempHeadA = tempHeadA.Next
		}

		if tempHeadB == nil {
			tempHeadB = headA
		} else {
			tempHeadB = tempHeadB.Next
		}
	}
	//走到这里说明两个节点相等或者相交，
	//如果相等则说明走到了最后也就是nil，因此只要返回其中一个就可以
	//如果两个节点走的时候相交也会退出循环，因此只要返回其中一个就可以

	return tempHeadB
}
```

~~**之前自己写的错误代码:**~~

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func getIntersectionNode(headA, headB *ListNode) *ListNode {

	//如果有一个为空，直接返回nil
	if headA == nil || headB == nil {
		return nil
	}

	tempHeadA, tempHeadB := headA, headB
	count := 0
	for tempHeadA != tempHeadB {
		if tempHeadA == nil {
			tempHeadA = headB
			count += 1
		}
		if tempHeadB == nil {
			tempHeadB = headA
			count += 1
		}

		tempHeadA, tempHeadB = tempHeadA.Next, tempHeadB.Next

		if count >= 3 {  //说明没找到
			break
		}
	}
	//说明最后只走了2次就找到了节点
    //count如果为0，说明两个等长且找到了相交的节点
	if count == 2 || count == 0 {
		return tempHeadA
	}
	return nil

}
```

