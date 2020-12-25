# 1290二进制链表转整数

## 方法一：

> //执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> //内存消耗：2 MB, 在所有 Go 提交中击败了100.00%的用户

思路：
1. 如果当前链表节点不空，每次获取当前链表节点值加入到ret中，
2. 之后每次后移，相当于二进制位左移了1位，同时值也要左移1位，
3. 重复如上步骤
```

​```go
func getDecimalValue(head *ListNode) int {
	ret := 0
	for head != nil {
		ret = ret<<1 + head.Val
		head = head.Next
	}
	return ret
}
```

## 方法二：参考LeetCode解法，对上述方法改进

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2 MB, 在所有 Go 提交中击败了100.00%的用户

原因：对上面代码进行改进，**因为链表中的值是二进制，所以左移1位后不用采用加法来计算，而可以采用或(只有0和1)**

> 思路：同方法一

```go
func getDecimalValue(head *ListNode) int {
	ret := 0
	for head != nil {
		ret = ret<<1 | head.Val
		head = head.Next
	}
	return ret
}
```

