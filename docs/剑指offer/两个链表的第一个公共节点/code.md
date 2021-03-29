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

## 【推荐】方法三：
!> 思路：两个链表一起走各自的内容，当自己的内容走完之后，走另外一个链表，不断循环，**循环条件是如果两个有一个不为空就走，因为我们两个链表走完自己都会走另外一个链表，所以每次循环的时候如果不相等就接着走，如果没有交点，一定走了自己的链表和另外一个链表之后，两个指针都一起指向空，所以我们的循环条件是两个有一个不为空就走**
```go
func getIntersectionNode(headA, headB *ListNode) *ListNode {
    pA, pB := headA, headB
    for pA != nil || pB != nil {
        if pA == pB {
            break
        }
        if pA != nil {
            pA = pA.Next
        } else {
            pA = headB
        }
        if pB != nil {
            pB = pB.Next
        } else {
            pB = headA
        }
    }
    return pA
}
```