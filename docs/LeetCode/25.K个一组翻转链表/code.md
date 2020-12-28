

# [25. K 个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

## 方法一

思路：
1. 每次遍历链表中的一个区间，也就是一个区间中的k个节点
    1.1 如果该区间中的节点不足k个我们直接返回，有两种情况，走了k步之后，end节点是Nil或者根本走不了k步
    1.2 如果该区间中的节点足够k个，我们直接反转这个区间的头和尾，注意反转之前与原链表的两边连接断开，反转之后重新连接
2. 重复步骤1直到全部遍历完或者不足k个节点


!> **注意：**需要建立一个dummyNode，因为我们可能从头部就开始进行反转


```go
/**
 * @Author: yirufeng
 * @Date: 2020/12/28 10:24 上午
 * @Desc: k个一组反转链表
 **/

/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reverseKGroup(head *ListNode, k int) *ListNode {
	if k <= 1 {
		return head
	}

	dummyNode := &ListNode{}
	dummyNode.Next = head

	prev := dummyNode
	//当前节点
	var cur, start, end, next *ListNode
	for {
		start, cur = prev.Next, prev
		//往后遍历k个节点，找到我们要反转区间的最后一个节点
		for i := 1; i <= k; i++ {
			if cur == nil {  //说明不足k个节点，那么我们就没有必要进行反转，直接返回
				return dummyNode.Next
			}

			cur = cur.Next
		}
		end = cur


		//例如1-2->3如果k=2，那么走到这里时，prev指向1,start为3,end为nil,cur也是nil,其实是不够k个节点,因此不需要反转
		//所以此时我们直接可以返回即可
		//TODO:注意点：最大的注意点
		if cur == nil  {
			return dummyNode.Next
		}

		//next表示当前要反转区间【start, end】的end的下一个节点，而prev表示start的上一个节点
		next = cur.Next
		//prev与后面断开，end与后面断开
		prev.Next, end.Next = nil, nil

		//反转[start, end]
		var tempPrev *ListNode
		tempCur := start
		for tempCur != nil {
			tempPrev, tempCur, tempCur.Next = tempCur, tempCur.Next, tempPrev
		}

		//反转之后进行连接
		prev.Next, start.Next = end, next
		//更新
		prev, cur = start, start
	}

	return dummyNode.Next
}

```