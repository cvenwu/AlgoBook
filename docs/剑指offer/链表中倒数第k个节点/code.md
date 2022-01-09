# 链表中倒数第k个节点

## 方法一：栈存放遍历链表的数据

衍生思路：递归

!> 既然递归是天然的栈，我们不由想到使用递归解决

## 方法二：两次遍历链表

!> 思路：第一次遍历链表统计长度，第二次遍历到第n - k + 1个节点(走n-k步)的时候就是我们想要的节点，直接返回

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



## 【推荐】方法三：双指针

!> 思路：使用双指针，让快指针先走k步，注意走的过程中可能走到nil。之后两个一起走，一直到快指针指向nil，然后返回慢指针

原因：第1个指针与第2个指针的距离为k，当第1个指针遍历到尾部的null，第2个指针正好在倒数第k个节点上

使用双指针的优点：方法三相比于方法二，只需要遍历一次链表，**相比于方法一以及其衍生方法并且不需要额外的空间复杂度**

**总结**：对于链表题目，双指针作为一种常用的解题思路有大量的应用，例如快慢指针，维持固定距离的两个指针等思想

**链表无法高效获取长度，无法根据偏移快速访问元素，是链表的两个劣势。然而面试的时候经常碰见诸如获取倒数第k个元素，获取中间位置的元素，判断链表是否存在环，判断环的长度等和长度与位置有关的问题。这些问题都可以通过灵活运用双指针来解决。**


```go
func getKthFromEnd(head *ListNode, k int) *ListNode {

	//参数非法，直接返回nil
	if k <= 0 {
		return nil
	}

	fast, slow := head, head
	//fast先走k步
	for i := 0; i < k; i++ {
		if fast == nil {  //说明k超出了链表的长度
			return nil
		}
		fast = fast.Next
	}

	for fast != nil {
		fast, slow = fast.Next, slow.Next
	}

	return slow
}
```


```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* getKthFromEnd(ListNode* head, int k) {
        //让一个指针先走k步
        ListNode *fast = head, *slow = head;
        for (int i = 1; i <= k; i++)
        {
            if (fast == nullptr) return nullptr;
            fast = fast->next;
        }

        //如果当fast为空的时候，slow指向的就是我们要的结果
        while(fast != nullptr)
        {
            fast = fast->next;
            slow = slow->next;
        }

        return slow;
    }
};
```