# 用队列实现栈

## 方法一：两个队列实现栈



思路：使用两个队列模拟栈，
使用队列实现栈，使用两个队列来实现栈，queue1, queue2,
1. 当加入元素的时候直接加入到queue1
2. 当弹出元素的时候，将queue1中的所有元素移动到queue2，只留下最后一个元素返回并移除，此时将queue1 与 queue2 互换
3. 当查看栈顶的时候，将queue1中所有的元素一样，压入到queue2，记录queue1最后一个元素的值。之后将queue1与queue2互换，此时返回我们记录的最后一个元素的值


> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2 MB, 在所有 Go 提交中击败了80.70%的用户



```go
type MyStack struct {
	queue1, queue2 []int

}


/** Initialize your data structure here. */
func Constructor() MyStack {
	return MyStack{
		queue1: []int{},
		queue2: []int{},
	}
}


/** Push element x onto stack. */
func (this *MyStack) Push(x int)  {
	this.queue1 = append(this.queue1, x)
}


/** Removes the element on top of the stack and returns that element. */
func (this *MyStack) Pop() int {
	for len(this.queue1) != 1 {
		this.queue2 = append(this.queue2, this.queue1[0])
		this.queue1 = this.queue1[1:]
	}

	//获得最后一个元素的值并弹出
	val := this.queue1[0]
	this.queue1 = this.queue1[1:]
	//交换queue1与queue2
	this.queue1, this.queue2 = this.queue2, this.queue1
	//弹出并返回我们栈中的栈顶
	return val
}


/** Get the top element. */
func (this *MyStack) Top() int {
	for len(this.queue1) != 1 {
		this.queue2 = append(this.queue2, this.queue1[0])
		this.queue1 = this.queue1[1:]
	}

	//获得最后一个元素的值
	val := this.queue1[0]
	this.queue2 = append(this.queue2, this.queue1[0])
	this.queue1 = this.queue1[1:]
	//交换queue1与queue2
	this.queue1, this.queue2 = this.queue2, this.queue1
	//弹出并返回我们栈中的栈顶
	return val
}


/** Returns whether the stack is empty. */
func (this *MyStack) Empty() bool {
	return len(this.queue1) == 0 && len(this.queue2) == 0
}

```





## 方法二：[推荐]一个队列实现栈

思路：因为我们每次加入的元素在栈里面都是栈顶，所以我们使用一个队列在加入元素的时候，将队列里面的元素进行反转。此时我们队列头就是栈顶元素，其他操作与栈雷同。



> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
			内存消耗：2 MB, 在所有 Go 提交中击败了45.61%的用户



```go
type MyStack struct {
	queue []int
}


/** Initialize your data structure here. */
func Constructor() MyStack {
	return MyStack{
		queue: []int{},
	}
}


/** Push element x onto stack. */
func (this *MyStack) Push(x int)  {
	this.queue = append(this.queue, x)
	//将前面n-1个元素扔到后面
	for i := 1; i <= len(this.queue); i++ {
		this.queue = append(this.queue, this.queue[0])
		this.queue = this.queue[1:]
	}
}


/** Removes the element on top of the stack and returns that element. */
func (this *MyStack) Pop() int {
	val := this.queue[0]
	this.queue = this.queue[1:]
	return val
}


/** Get the top element. */
func (this *MyStack) Top() int {
	return this.queue[0]
}


/** Returns whether the stack is empty. */
func (this *MyStack) Empty() bool {
	return len(this.queue) == 0
}

```

## 总结：

这里是使用队列来实现栈，自己参考了LeetCode上很多解法，采用了一个双端队列来实现，并不符合题目中的要求`peek/pop from front`。这里罗列了两个方法，其中一个方法只使用一个队列，空复为O(N)，另外一个方法使用了两个队列，空复为O(N*2)。

**每个操作的时间复杂度：**

|       | 方法一：两个队列 | 方法二：一个队列 |
| ----- | ---------------- | ---------------- |
| Push  | O(1)             | O(N)             |
| Pop   | O(N)             | O(1)             |
| top   | O(1)             | O(1)             |
| empty | O(1)             | O(1)             |

其他解法[详见](https://leetcode-cn.com/problems/implement-stack-using-queues/solution/yong-dui-lie-shi-xian-zhan-by-leetcode/)