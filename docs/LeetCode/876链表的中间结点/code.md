#  876 链表的中间结点

## 方法一：双指针

思路：双指针。

注意：链表中可能有1个中间节点也可能有两个中间节点，取决于链表的节点的个数

1. **如果链表有奇数个节点，则快指针最后移动到最后一个节点，此时慢指针恰好指向中间节点**
2. **如果链表有偶数个节点，则快指针最后移动到最后一个节点的下一个节点(nil)，此时慢指针恰好指向中间节点**

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2 MB, 在所有 Go 提交中击败了24.64%的用户



```go
func middleNode(head *ListNode) *ListNode {

	//如果没有节点或者只有一个节点
	if head == nil || head.Next == nil {
		return head
	}

	fast, slow := head, head
	for fast != nil && fast.Next != nil {
		fast, slow = fast.Next.Next, slow.Next
	}


	return slow
}
```





## 方法二：方法一的改进

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2 MB, 在所有 Go 提交中击败了75.36%的用户

思路：同方法一



> 改进：最开始的if判断不需要了，因为在for循环中就已经判断了

```go
func middleNode(head *ListNode) *ListNode {

	//如果没有节点或者只有一个节点
	//if head == nil || head.Next == nil {
	//	return head
	//}

	fast, slow := head, head
	for fast != nil && fast.Next != nil {
		fast, slow = fast.Next.Next, slow.Next
	}


	return slow
}
```







## 其他方法：

[参考](https://leetcode-cn.com/problems/middle-of-the-linked-list/solution/lian-biao-de-zhong-jian-jie-dian-by-leetcode-solut/)



1. 使用数组存储链表各元素的值，之后直接采用下标获取中间的节点
2. 使用单指针两次遍历数组，第一次获得链表长度，第二次求得中间节点