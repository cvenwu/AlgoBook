# [1441. 用栈操作构建数组](https://leetcode-cn.com/problems/build-an-array-with-stack-operations/)

## 方法一：使用一个栈模拟数组


具体流程看代码注释即可


> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
			内存消耗：2.3 ms, 在所有 Go 提交中击败了7.23%的用户

需要额外的O(N)空间复杂度

```go
func buildArray(target []int, n int) []string {

	stack := []int{}
	ret := []string{}
	curIndex := 0

	//题目中说明了因为有的时候不一定要读取n个元素，也就是说我们构建stack的过程中一旦遇到curIndex越界就直接可以返回了
	for i := 1; i <= n && curIndex < len(target); i++ {
		//因为每次都要加入当前元素
		stack = append(stack, i)
		//每次都要加入push到结果中
		ret = append(ret, "Push")
		//如果刚加入的元素与我们的目标数组目前指向的元素不同，那么说明刚加入的元素一定弹出了
		if stack[len(stack)-1] != target[curIndex] {
			stack = stack[:len(stack)-1]
			ret = append(ret, "Pop")
		} else { //如果相同说明不需要弹出，则curIndex移动到下一个
			curIndex++
		}
	}

	return ret
}

```

## 方法二：优化，使用双指针
额外空间复杂度O(1)

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 内存消耗：2.3 ms, 在所有 Go 提交中击败了30.23%的用户


```go
func buildArray(target []int, n int) []string {

	ret := []string{}
	curIndex := 0

	//题目中说明了因为有的时候不一定要读取n个元素，也就是说我们构建遍历的过程中一旦遇到curIndex越界就直接可以返回了
	for i := 1; i <= n && curIndex < len(target); i++ {
		//每次都要加入push到结果中
		ret = append(ret, "Push")
		//如果刚加入的元素与我们的目标数组目前指向的元素不同，那么说明刚加入的元素一定弹出了
		if i != target[curIndex] {
			ret = append(ret, "Pop")
		} else { //如果相同说明不需要弹出，则curIndex移动到下一个
			curIndex++
		}
	}

	return ret
}

```

