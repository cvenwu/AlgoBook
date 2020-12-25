# [119. 杨辉三角 II](https://leetcode-cn.com/problems/pascals-triangle-ii/)

## 方法：找规律并进行压缩

> 思路：同118，无非不过进行状态压缩

**注意更新每行的时候是从右往左更新，否则会出错**，因为我们有一个公式`nums[i] = nums[i-1]+nums[i]`，所以如果`nums[i-1]`更新，导致当前行用的是上一行的`nums[i-1]`所以会出错。因此我们一定要记得从右往左更新

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
内存消耗：2.1 MB, 在所有 Go 提交中击败了26.82%的用户


下面的代码较容易理解

```go
func getRow(rowIndex int) []int {
	//如果传入的参数非法
	if rowIndex < 0 {
		return nil
	}

	//该行有rowIndex+1个元素，其中第1个元素不需要更新
	nums := make([]int, rowIndex+1)
	for i := 0; i <= rowIndex; i++ {  //从第0行开始遍历，遍历到第rowIndex行。所在的行有i+1个元素
		//每一行的第1个元素为1
		nums[0] = 1
		//每一行的中间元素从下标为1开始一直到该行最后一个元素为止(不包括，当前行最后一个元素下标为i)
		//最大的注意点：这里要从后往前更新，因为后面的会用到前面的值，如果前面更新导致后面会出错
		for j := i-1; j >= 1; j-- {
			nums[j] = nums[j-1] + nums[j]
		}
		//如果元素个数大于1，一定要记得最后一个元素加上1
		if i >= 1 {
			nums[i] = 1
		}
	}
	return nums
}
```



## 方法二：参考LeetCode他人代码进行改进

[LeetCode的双百代码Golang](https://leetcode-cn.com/problems/pascals-triangle-ii/solution/zhi-xing-yong-shi-0-ms-zai-suo-you-golang-ti-jia-8/)

```go
func getRow(rowIndex int) []int {
	// 第0行
	nums := []int{1}
	for i := 1; i <= rowIndex; i++ {
		// 尾部追加1
		nums = append(nums, 1)
		// 倒序计算杨辉三角当前行
		for j:=i-1;j>0;j--{
			nums[j]+=nums[j-1]
		}
	}
	return nums
}
```

