# [303. 区域和检索 - 数组不可变](https://leetcode-cn.com/problems/range-sum-query-immutable/)

## 方法：使用一个数组保存前缀和

思路：使用一个数组保存前缀和


> 执行用时：36 ms, 在所有 Go 提交中击败了77.95%的用户
> 内存消耗：9.4 ms, 在所有 Go 提交中击败了36.15%的用户


```go
type NumArray struct {
	sumArray []int
}

func Constructor(nums []int) NumArray {
	sumArray := make([]int, len(nums))
	//sumArray[i]表示从0位置到i位置的元素的和
	if len(nums) > 0 {
		sumArray[0] = nums[0]
	}
	for i := 1; i < len(nums); i++ {
		sumArray[i] = sumArray[i-1] + nums[i]
	}
	return NumArray{
		sumArray: sumArray,
	}
}

func (this *NumArray) SumRange(i int, j int) int {
	if i > j {
		return 0
	}
	if i == 0 {
		return this.sumArray[j]
	} else {
		return this.sumArray[j] - this.sumArray[i-1]

	}
}

/**
 * Your NumArray object will be instantiated and called as such:
 * obj := Constructor(nums);
 * param_1 := obj.SumRange(i,j);
 */
```

## 方法二【推荐】：参考他人代码进行优化

```go

type NumArray struct {
	sumArray []int
}

func Constructor(nums []int) NumArray {
	if len(nums) < 0 {
		return NumArray{sumArray: nil}
	}
	sumArray := make([]int, len(nums))
	//sumArray[i]表示从0位置到i位置的元素的和
	sumArray[0] = nums[0]
	for i := 1; i < len(nums); i++ {
		sumArray[i] = sumArray[i-1] + nums[i]
	}
	return NumArray{
		sumArray: sumArray,
	}
}

func (this *NumArray) SumRange(i int, j int) int {

	if i == 0 {
		return this.sumArray[j]
	}

	return this.sumArray[j] - this.sumArray[i-1]
}
```

