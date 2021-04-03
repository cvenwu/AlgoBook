# [78.子集](https://leetcode-cn.com/problems/subsets/)

## 【推荐】方法：回溯

> 画图：![TQnge8](https://cdn.jsdelivr.net/gh/sivanWu0222/ImageHosting@master/uPic/TQnge8.png)

注意：画图之后发现我们每次路径上的元素都要加入到结果中。

步骤：
1. 每次我们循环选择当前可以使用的元素，当前可以使用的元素范围从传入的参数index到我们的最后一个元素的索引，只能向后选，不可以交换
2. 假设当前选择索引为i的元素之后，那么下一次我们需要递归到索引为i+1的元素。


!> 注意我们不需要判断递归的参数index越界，因为for循环会帮助我们进行判断。

```go
//思路：每次在选择当前数字的时候，只能使用循环向后选，不可以反转
func subsets(nums []int) [][]int {
	var ret [][]int
	backtrack(nums, 0, []int{}, &ret)
	return ret
}


//第二个参数表示该次从哪个数字开始遍历，也就是在nums中我们只能加入index之后的数(包含index)
func backtrack(nums []int, index int, path []int, ret *[][]int) {
	//将当前的path加入到我们的结果中
	temp := make([]int, len(path))
	copy(temp, path)
	*ret = append(*ret, temp)

	//循环向后做选择，
	for i := index; i < len(nums); i++ {
		path = append(path, nums[i])
		//注意第2个参数表示我们下一个要从哪个参数开始选，当前我们选择了nums[i]这个数字，因此下次一定是从i+1位置开始选
		backtrack(nums, i+1, path, ret)
		path = path[:len(path)-1]
	}
}

```