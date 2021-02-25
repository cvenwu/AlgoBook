# [46. 全排列](https://leetcode-cn.com/problems/permutations/)

## 【推荐】方法一：使用一个used数组判断是否访问过

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


## 方法二：不使用used数组而是交换

```go
func permute(nums []int) [][]int {
	var ret [][]int
	backtrack(nums, []int{}, 0, &ret)
	return ret
}

func backtrack(nums []int, path []int, depth int, ret *[][]int) {
	//说明当前已经形成了一个排列
	if depth == len(nums) {
		temp := make([]int, len(path))
		copy(temp, path)
		*ret = append(*ret, temp)
		return
	}

	//此时排列还没有形成
	for i := depth; i < len(nums); i++ {
		//交换当前选择的数字与我们当前开始的数字
		nums[i], nums[depth] = nums[depth], nums[i]
		//将当前的数字加入路径中
		path = append(path, nums[depth])
		//进入下一层递归
		backtrack(nums, path, depth+1, ret)
		//从路径中去除选择的数字
		path = path[:len(path)-1]
		//交换回去
		nums[i], nums[depth] = nums[depth], nums[i]
	}
}

```