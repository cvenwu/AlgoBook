# [剑指 Offer 13. 机器人的运动范围](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)



## 方法一：回溯


> 执行用时：0 ms, 在所有 Go 提交中击败了 100.00% 的用户
> 		内存消耗：2.7 MB, 在所有 Go 提交中击败了 40.52% 的用户


```go
func movingCount(m int, n int, k int) int {
	//定义一个visited数组，并且置默认值为false
	visited := make([][]bool, m)
	for i := 0; i < m; i++ {
		visited[i] = make([]bool, n)
	}

	backtrack(visited, 0, 0, m, n, k)

	//最后visited数组肯定有的为true,我们统计visited为true的元素个数
	count := 0
	for _, row := range visited {
		for _, val := range row {
			if val {
				count += 1
			}
		}
	}
	return count
}

func backtrack(visited [][]bool, rowIndex int, colIndex int, m int, n int, k int) {
	//回溯结束条件
	//1. 如果当前点访问过了直接返回0，注意这个要放在后面，因为确保不可以越界
	//2. 如果当前要访问的坐标越界了，直接返回0
	//3. 如果当前要访问的坐标行列之和超过了k那么直接返回0
	if rowIndex >= m || colIndex >= n || rowIndex < 0 || colIndex < 0 || visited[rowIndex][colIndex] || getDigitSum(rowIndex)+getDigitSum(colIndex) > k {
		return
	}
	visited[rowIndex][colIndex] = true
	backtrack(visited, rowIndex+1, colIndex, m, n, k)
	backtrack(visited, rowIndex-1, colIndex, m, n, k)
	backtrack(visited, rowIndex, colIndex+1, m, n, k)
	backtrack(visited, rowIndex, colIndex-1, m, n, k)
	return
}

//计算各位数的和
func getDigitSum(num int) int {
	sum := 0
	for num != 0 {
		sum += (num % 10)
		num /= 10
	}
	return sum
}
```



## 方法二：回溯改进

改进：

1. 修改backtrack函数的返回值，我们不需要统计visited为true的元素个数，我们直接调用它返回结果即可，
2. 如果当前节点没有越界，数位之和小于k并且没有访问过就返回1 + 当前节点的上下左右递归


> 执行用时：0 ms, 在所有 Go 提交中击败了 100.00% 的用户
> 		内存消耗：2.7 MB, 在所有 Go 提交中击败了 39.66% 的用户




```go
func movingCount(m int, n int, k int) int {
	//定义一个visited数组，并且置默认值为false
	visited := make([][]bool, m)
	for i := 0; i < m; i++ {
		visited[i] = make([]bool, n)
	}

	return backtrack(visited, 0, 0, m, n, k)

}

func backtrack(visited [][]bool, rowIndex int, colIndex int, m int, n int, k int) int {
	//回溯结束条件
	//1. 如果当前点访问过了直接返回0，注意这个要放在后面，因为确保不可以越界
	//2. 如果当前要访问的坐标越界了，直接返回0
	//3. 如果当前要访问的坐标行列之和超过了k那么直接返回0
	if rowIndex >= m || colIndex >= n || rowIndex < 0 || colIndex < 0 || visited[rowIndex][colIndex] || getDigitSum(rowIndex)+getDigitSum(colIndex) > k {
		return 0
	}
	visited[rowIndex][colIndex] = true
	return 1 + backtrack(visited, rowIndex+1, colIndex, m, n, k) + backtrack(visited, rowIndex-1, colIndex, m, n, k) + backtrack(visited, rowIndex, colIndex+1, m, n, k) + backtrack(visited, rowIndex, colIndex-1, m, n, k)
}

//计算各位数的和
func getDigitSum(num int) int {
	sum := 0
	for num != 0 {
		sum += (num % 10)
		num /= 10
	}
	return sum
}

```

