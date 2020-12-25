
# [228.汇总区间](https://leetcode-cn.com/problems/summary-ranges/)


## 方法：
思路：
1. 如果只有一个数就直接以字符串的形式输出添加到我们最终的结果中
2. 其他的区间，每次使用两个变量start以及end分别指向区间的起点和终点，另外使用index表示当前遍历到的位置
3. 如果区间可以扩张，即右边的数的序号为end+1没有越界并且等于左边的数+1，所以可以直接扩张
4. 否则说明当前区间start->end已经不可以扩张，直接加入到我们的结果中，并且下次让start=end+1即可

```go

import "strconv"

/**
 * @Author: yirufeng
 * @Date: 2020/12/15 9:22 上午
 * @Desc: 228.汇总区间
 **/



//执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
//内存消耗：1.9 MB, 在所有 Go 提交中击败了67.44%的用户
func summaryRanges(nums []int) []string {
	if len(nums) == 0 {
		return nil
	}

	start, end := 0, 0
	var ret []string
	for end < len(nums) {
		for end + 1 < len(nums) && nums[end+1] == nums[end]+1 {
			end++
		}

		if start != end {
			ret = append(ret, strconv.Itoa(nums[start]) + "->" + strconv.Itoa(nums[end]))
		} else {
			ret = append(ret, strconv.Itoa(nums[start]))
		}
		//添加到结果中后，重置start以及end
		start, end = end+1, end+1
	}

	return ret
}

```