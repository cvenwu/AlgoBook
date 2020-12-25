# 两个链表的第一个公共节点



## 方法一：第一个链表遍历完，指针指向第二个链表



>  思路：当我们第一个链表遍历完之后，指针指向第2个链表头。这样可以避免遍历链表获取链表长度



> *// 执行用时：44 ms, 在所有 Go 提交中击败了91.37% 的用户*
>
> *// 内存消耗：7.9 MB, 在所有 Go 提交中击败了 100.00% 的用户*

```go
func getIntersectionNode(headA, headB *ListNode) *ListNode {
	//如果两个链表有一个为空，返回nil
    if headA == nil || headB == nil {
		return nil
	}

	//判断两个链表是否相交
	//找到两个链表的交点
	flag := 1
    tempHeadA, tempHeadB := headA, headB
	for tempHeadA != tempHeadB {
		//当其中一个到达了末尾
		if tempHeadB.Next == nil {
			tempHeadB = headA
			flag += 1
		}
		if tempHeadA.Next == nil {
			tempHeadA = headB
			flag += 1
		}
		//说明两个链表的头都已遍历完自己的链表和另外一个链表，并且其中一个又遍历完另一个链表
		if flag > 3 {
			return nil
		}
        tempHeadA, tempHeadB = tempHeadA.Next, tempHeadB.Next
	}

	return tempHeadA;
}
```





## 方法二：



> 思路：遍历两个链表分别获取其长度，然后其中长的链表先走长度差的步数



> *// 执行用时：44 ms, 在所有 Go 提交中击败了91.37% 的用户*
>
> *// 内存消耗：7.9 MB, 在所有 Go 提交中击败了 100.00% 的用户*

```go
func getIntersectionNode(headA, headB *ListNode) *ListNode {
	//如果两个链表有一个为空，返回nil
	if headA == nil || headB == nil {
		return nil
	}

	//获取两个链表的长度
	lenA, lenB := 1, 1
	tempHeadA, tempHeadB := headA, headB
	for tempHeadA.Next != nil {
		tempHeadA, lenA = tempHeadA.Next, lenA + 1
	}
	for tempHeadB.Next != nil {
		tempHeadB, lenB = tempHeadB.Next, lenB + 1
	}

	//重置
	tempHeadA, tempHeadB = headA, headB

	//第一个链表长
	if lenA > lenB {
		for i := 0; i < lenA - lenB; i++ {
			tempHeadA = tempHeadA.Next
		}
	} else {	//第2个链表长
		for i := 0; i < lenB - lenA; i++ {
			tempHeadB = tempHeadB.Next
		}
	}


	for tempHeadA != tempHeadB {
		//说明没有交点
		if tempHeadB.Next == nil || tempHeadA == nil {
			return nil
		}
		tempHeadA, tempHeadB = tempHeadA.Next, tempHeadB.Next
	}

	return tempHeadB
}
```

