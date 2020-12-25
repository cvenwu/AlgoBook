# 链表中倒数第k个节点

## 方法一：栈存放遍历链表的数据

衍生思路：递归

> 既然递归是天然的栈，我们不由想到使用递归解决

## 方法二：两次遍历链表

> 思路：第一次遍历链表统计长度，第二次遍历到第n - k + 1个节点(走n-k步)的时候就是我们想要的节点，直接返回

> //执行用时：0 ms, 在所有 Go 提交中击败了100.00% 的用户

> //内存消耗：2.2 MB, 在所有 Go 提交中击败了 100.00% 的用户

```go
func getKthFromEnd(head *ListNode, k int) *ListNode {
	//如果链表为空或者k为非正数
	if head == nil || k <= 0{
        return nil
	}
	
	//获取链表长度
	tempHead, length := head, 0
	for {
		if tempHead != nil {
			length, tempHead = length + 1, tempHead.Next
		} else {
			break
		}
	}

	//如果长度小于k,直接返回nil
	if length < k {
		return nil
	}

	//获取倒数第k个节点
	tempHead = head
	for i := 0; i < length - k; i++ {
		tempHead = tempHead.Next
	}

	return tempHead
}
```



## 方法三[推荐]：双指针

> 思路：使用双指针，保持两个指针的距离为k-1。

> 原因：第1个指针与第2个指针的距离为k-1，当第1个指针遍历到尾，第2个指针正好在倒数第k个节点上



> //执行用时：0 ms, 在所有 Go 提交中击败了100.00% 的用户

> //内存消耗：2.2 MB, 在所有 Go 提交中击败了 100.00% 的用户

```go
//方法3：双指针，第1个指针先走k-1步，之后两个指针开始走，当第1个指针走到最后一个，第2个指针便是倒数第k个节点
func getKthFromEnd(head *ListNode, k int) *ListNode {
    //如果链表不存在怎么办
    //如果k是一个非正数怎么办
    if head == nil || k <= 0{
        return nil
    }
        
    //如果k大于链表长度，返回nil
    firstPointer, secondPointer := head, head
    //第1个指针先走k-1步
    for i := 0; i < k - 1; i++ {
        firstPointer = firstPointer.Next
        if firstPointer == nil {
            return nil
        }
    }
    
    //之后两个指针一起开始走。当第1个指针走到尾，第2个指针便指向了倒数第k个节点
    for {
        if firstPointer.Next == nil { //说明第1个指针走到尾巴
            return secondPointer
        } else {
            firstPointer, secondPointer = firstPointer.Next, secondPointer.Next
        }
    }
    
    return secondPointer
}
```







方法三相比于方法二，只需要遍历一次链表，**相比于方法一以及其衍生方法并且不需要额外的空间复杂度**

**总结**：对于链表题目，双指针作为一种常用的解题思路有大量的应用，例如快慢指针，维持固定距离的两个指针等思想

**链表无法高效获取长度，无法根据偏移快速访问元素，是链表的两个劣势。然而面试的时候经常碰见诸如获取倒数第k个元素，获取中间位置的元素，判断链表是否存在环，判断环的长度等和长度与位置有关的问题。这些问题都可以通过灵活运用双指针来解决。**

