
# [47.全排列II](https://leetcode-cn.com/problems/permutations-ii/)


## 方法一：
**核心：**因为有重复元素，所以结果中可能会出现重复的内容，因此我们使用一个map记录当前位置已经选择过的元素，如果重复元素在当前元素选择，我们就跳过

> 比如：1,1,3,5

!> 解释：在遍历到第1个位置选择第1个1之后，我们将第2个位置的1交换到第1个位置上，发现之前已经在第1个位置上选择过1，所以我们就跳过，因为如果不跳过就会重复。

```go
func permuteUnique(nums []int) [][]int {
	var ret [][]int

	backtrack(nums, &ret, 0, []int{})
	return ret
}

//当前要选择的数字从index位置开始
func backtrack(nums []int, ret *[][]int, index int, path []int) {
	//说明符合题目条件
	if index == len(nums) {
		temp := make([]int, index)
		copy(temp, path)
		*ret = append(*ret, temp)
		return
	}
    //每次到当前该元素的时候，使用一个map记录已经选择过的，避免在当前选择的时候选择和之前在当前放置的一样的元素
	visited := make(map[int]bool)
	//否则就要做选择
	for i := index; i < len(nums); i++ {
		if visited[nums[i]] {
			continue
		}

		//将选择的元素交换到当前index位置
		nums[i], nums[index] = nums[index], nums[i]
		//然后将nums[index]位置的元素加入到我们的path
		path = append(path, nums[index])
		//然后深入递归到下一步
		backtrack(nums, ret, index+1, path)
		//然后递归回来之后记得将当前元素去掉
		path = path[:len(path)-1]
		//然后交换回去
		nums[i], nums[index] = nums[index], nums[i]
		visited[nums[i]] = true
	}
}
```