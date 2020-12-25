# 除自身以外数组的乘积

## 方法一：

思路：使用两个数组，分别求出nums[i]左右两边的乘积，最后进行相乘

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2 MB, 在所有 Go 提交中击败了21.38%的用户

> //时间复杂度：O(N)
> 		//空间复杂度：O(N)

1. numsBefore数组表示nums[0]*.....*nums[i-1]。公式如下：
   1.  numsBefore[0] = 1
   2. numsBefore[i] = numsBefore[i-1] * nums[i-1]
2. numsEnd数组表示nums[i+1]*.....*nums[len(nums)-1]
   1. numsEnd[len(nums)-1] = 1
   2. numsEnd[len(nums)-1-i] = numsEnd[len(nums)-i] * nums[len(nums)-i]


```go
func productExceptSelf(nums []int) []int {
	numsBefore, numsEnd := make([]int, len(nums)), make([]int, len(nums))

	numsBefore[0] = 1
	numsEnd[len(nums)-1] = 1

	for i := 1; i < len(nums); i++ {
		numsBefore[i] = numsBefore[i-1] * nums[i-1]
		numsEnd[len(nums)-1-i] = numsEnd[len(nums)-i] * nums[len(nums)-i]
	}

	fmt.Println(numsBefore, numsEnd)

	ret := make([]int, len(nums))
	for i := 0; i < len(nums); i++ {
		ret[i] = numsBefore[i] * numsEnd[i]
	}

	return ret
}

```



## [推荐]方法二：只使用一个数组和两次遍历来解决

> 改进思路：因为题目中说在常数空间复杂度内完成这道题目。所以我们只采用一个数组，按顺序求完前面i-1个元素的乘积，之后再从数组最后一个元素开始倒序开始求后半部分的乘积

> 
> 执行用时：16 ms, 在所有 Go 提交中击败了72.51%的用户
> 内存消耗：6.3 MB, 在所有 Go 提交中击败了98.36%的用户
> 

> //时间复杂度：O(N)
> 		//空间复杂度：O(1)


```go
func productExceptSelf(nums []int) []int {

	answer := make([]int, len(nums))

	answer[0] = 1
	//计算上三角
	for i := 1; i < len(nums); i++ {
		answer[i] = answer[i-1] * nums[i-1]
	}

	//计算下三角和上三角的乘积
	q := 1
	for i := len(nums)-2; i >= 0; i-- {
		q *= nums[i+1]
		answer[i] *= q
	}

	return answer
}

```



## 总结：

1. 方法一：我们使用了两个额外的数组，分别计算该数字左右两边的乘积，

2. 方法二：我们只使用了一个数组，但是两次遍历计算得到我们最终的结果，

