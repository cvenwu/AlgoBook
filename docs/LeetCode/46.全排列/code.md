# [46. 全排列](https://leetcode-cn.com/problems/permutations/)

## 方法：回溯

> ```
> 执行用时：0ms, 在所有 Go 提交中击败了100.00%的用户
> 内存消耗：2.8 MB, 在所有 Go 提交中击败了24.00%的用户
> ```

思路：DFS

当满足退出条件，也就是长度等于我们给的切片的长度就可以将其加入结果中。

**注意要拷贝，因为golang中二维数组存储的是一维数组的引用**

```go
func permute(nums []int) [][]int {
	if len(nums) == 0 {
		return nil
	}
	used, cur, res := make([]bool, len(nums)), []int{}, [][]int{}

	generatePermutation(&nums, &res, &used, cur)
	return res
}

func generatePermutation(nums *[]int, res *[][]int, used *[]bool, cur []int) {
	//添加正确之后的对比
	if len(*nums) == len(cur) {
		temp := make([]int, len(cur))
		copy(temp, cur)
		*res = append(*res, temp)
		return
	}

	for i := 0; i < len(*nums); i++ {
		//如果当前值没有使用
		if !(*used)[i] {
			(*used)[i] = true
			cur = append(cur, (*nums)[i])
			//进行递归
			generatePermutation(nums, res, used, cur)
			//到这里说明之前的已经遍历完了
			(*used)[i] = false
			cur = cur[:len(cur)-1]
		}
	}

}

```

