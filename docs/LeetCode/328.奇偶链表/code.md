

# [328. 奇偶链表](https://leetcode-cn.com/problems/odd-even-linked-list/)

## 方法：


自己想到的方法：使用4个指针
1. 第1个指针指向奇数的头节点，第2个指针指向偶数的头节点
2. 第3个指针不断遍历并修改序号为奇数的节点的指向，第4个指针不断遍历并修改序号为偶数的节点的指向
3. 当修改完成后，此时第3个指针指向最后一个奇数节点，第4个指针指向最后一个偶数节点
4. 此时将第3个指针指向偶数的第1个节点（第2个指针），第4个指针的Next记得设置为空，否则可能会死循环(如果后面还有一个奇数节点)

时间复杂度：O(N)
		空间复杂度：O(1)

> 执行用时：4 ms, 在所有 Go 提交中击败了91.25%的用户
> 		内存消耗：3.2 ms, 在所有 Go 提交中击败了23.08%的用户


代码：

```go
package main

import "fmt"

/**
 * @Author: yirufeng
 * @Email: yirufeng@foxmail.com
 * @Date: 2020/9/29 4:43 下午
 * @Desc: 328.奇偶链表
 */

type ListNode struct {
	Val  int
	Next *ListNode
}
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */


/*
自己想到的方法：使用4个指针
1. 第1个指针指向奇数的头节点，第2个指针指向偶数的头节点
2. 第3个指针不断遍历并修改序号为奇数的节点的指向，第4个指针不断遍历并修改序号为偶数的节点的指向
3. 当修改完成后，此时第3个指针指向最后一个奇数节点，第4个指针指向最后一个偶数节点
4. 此时将第3个指针指向偶数的第1个节点（第2个指针），第4个指针的Next记得设置为空，否则可能会死循环(如果后面还有一个奇数节点)


时间复杂度：O(N)
空间复杂度：O(1)

执行用时：4 ms, 在所有 Go 提交中击败了91.25%的用户
内存消耗：3.2 ms, 在所有 Go 提交中击败了23.08%的用户
 */
func oddEvenList(head *ListNode) *ListNode {
	if head == nil || head.Next == nil || head.Next.Next == nil {
		return head
	}

	oddHead, evenHead, oddFast, evenFast := head, head.Next, head, head.Next

	for (oddFast.Next != nil && oddFast.Next.Next != nil)  || (evenFast.Next != nil && evenFast.Next.Next != nil){
		if oddFast.Next != nil && oddFast.Next.Next != nil {
			oddFast.Next = oddFast.Next.Next
			oddFast = oddFast.Next
		}
		if evenFast.Next != nil && evenFast.Next.Next != nil {
			evenFast.Next = evenFast.Next.Next
			evenFast = evenFast.Next
		}
	}

	//注意点1：此时oddFast指向最后一个奇数节点
	oddFast.Next = evenHead
	//注意点2：记得修改偶数链表最后一个节点为空，否则会死循环
	evenFast.Next = nil


	return oddHead
}

func main() {
	head := &ListNode{
		Val: 1,
	}
	head2 := &ListNode{
		Val: 2,
	}
	head3 := &ListNode{
		Val: 3,
	}
	head4 := &ListNode{
		Val: 4,
	}
	head5 := &ListNode{
		Val: 5,
	}
	head.Next = head2
	head2.Next = head3
	head3.Next = head4
	head4.Next = head5
	oddEvenList(head)


	for head != nil {
		fmt.Printf("%d\t", head.Val)
		head = head.Next
	}
}

```

