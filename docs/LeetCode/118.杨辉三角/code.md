# [118. 杨辉三角](https://leetcode-cn.com/problems/pascals-triangle/)



## 方法：找规律后直接循环

规律：某一行的行号等于这一行的元素个数

思路：
1. 首先每行开始之前加入1个1
2. 如果当前行的元素个数大于2个说明中间有元素，开始加入，假设当前在第i行第j列(都从1开始)，那么`nums[i-1][j] = nums[i-2][j-1]+nums[i-2][j]`
3. 如果当前行数大于等于2说明要加上最后1个1

>执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
		内存消耗：2.1 MB, 在所有 Go 提交中击败了22.81%的用户




```go
func generate(numRows int) [][]int {
	ret := [][]int{}
	for i := 1; i <= numRows; i++ {
		rowRet := []int{1}
		for j := 1; j <=i-2; j++ {
			//注意这里的是i-2,因为我们i从1开始，要减去一个1，其次还要找到上一行因此还要再减去1
			rowRet = append(rowRet, ret[i-2][j-1]+ret[i-2][j])
		}
		//这一个要从第2行开始才有末尾的1
		if i >= 2 {
			rowRet = append(rowRet, 1)
		}
		ret = append(ret, rowRet)
	}
	return ret
}
```

