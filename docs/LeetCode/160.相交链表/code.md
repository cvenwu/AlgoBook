# [160. 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

## 方法：

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

