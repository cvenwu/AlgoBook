# [56.合并区间](https://leetcode-cn.com/problems/merge-intervals/)


## 方法一：自己的想法

参考LeetCode官方题解，其实就是贪心策略

!> 思路：首先对区间按照起点从小到达进行排序，然后将第一个放置到结果中，之后的区间与前面结果中的最后一个区间进行比较，如果后面的区间的起点大于结果中最后一个区间的终点，那么直接加入到结果中，否则将结果中最后一个区间的终点修改为后面区间的终点与最后一个区间终点的较大值。


```go
//执行用时：80 ms, 在所有 Go 提交中击败了7.84%的用户
//内存消耗：4.6 MB, 在所有 Go 提交中击败了28%的用户
func merge(intervals [][]int) [][]int {

	if len(intervals) <= 1 {
		return intervals
	}

	//遍历intervals，将里面的区间按照区间起点升序排序
	for i := 1; i < len(intervals); i++ {
		j := i
		for intervals[j][0] < intervals[j-1][0] {
			intervals[j], intervals[j-1] = intervals[j-1], intervals[j]
			j--
		}
	}

s	ret := [][]int{intervals[0]}

	//之后遍历现在排序好的每个区间
	for i := 1; i < len(intervals); i++ {
		//如果当前区间的起点大于上一个区间的终点，那么直接加入
		if intervals[i][0] > intervals[i-1][1] {
			ret = append(ret, intervals[i])
			continue
		}

		//否则，则找出当前区间与结果中最后一个区间的终点的较大值
		ret[len(ret)-1][1] = max(ret[len(ret)-1][1], intervals[i][1])
	}

	return ret
}
```


## 对方法一的改进：

改进点：
1. 使用**sort.Slice()**进行排序
2. 对于每次获取结果中最后一个区间的元素，我们可以**使用一个变量即可替代**。

```go
//执行用时：8 ms, 在所有 Go 提交中击败了96.74%的用户
//内存消耗：4.6 MB, 在所有 Go 提交中击败了79.09%的用户
func merge(intervals [][]int) [][]int {
	if len(intervals) <= 1 {
		return intervals
	}

	//改进1
	sort.Slice(intervals, func(i, j int) bool {
		return intervals[i][0] < intervals[j][0]
	})

	ret := [][]int{intervals[0]}
	//改进2
	prev := ret[0]
	//之后遍历现在排序好的每个区间
	for i := 1; i < len(intervals); i++ {
		//如果当前区间的起点大于上一个区间的终点，那么直接加入
		if intervals[i][0] > prev[1] {
			//改进2
			prev = intervals[i]
			ret = append(ret, intervals[i])
			continue
		}

		//否则，则找出当前区间与结果中最后一个区间的终点的较大值
		//改进2
		if prev[1] < intervals[i][1] {
			prev[1] = intervals[i][1]
		}
	}

	return ret
}
```


!> 另外一种写法：**2021-04-09更新**


```go
package main

import "sort"

/**
 * @Author: yirufeng
 * @Date: 2021/4/9 9:43 上午
 * @Desc:
 **/

/*
1. 按照区间起点和终点进行排序
2. 如果后面的区间的起点小于前面的终点，那么将上一个加入的区间的终点修改为后面区间的终点与上一个区间的终点的最大值
3. 如果后面的区间的起点大于前面的终点，直接将上一个区间加入到我们的结果中
*/
func merge(intervals [][]int) [][]int {
	if len(intervals) <= 0 {
		return nil
	}

	//按照区间起点进行排序，起点相同的时候按照终点进行排序
	sort.Slice(intervals, func(i, j int) bool {
		return intervals[i][0] < intervals[j][0] || (intervals[i][0] == intervals[j][0] &&
			intervals[i][1] < intervals[j][1])
	})

	ret := [][]int{intervals[0]}
	var curLength int
	for i := 1; i < len(intervals); i++ {
		curLength = len(ret)
		//如果起点大于ret最后一个的终点则直接加入
		if intervals[i][0] > ret[curLength-1][1] {
			ret = append(ret, intervals[i])
		} else {
			ret[len(ret)-1][1] = max(intervals[i][1], ret[curLength-1][1])
		}
	}
	return ret
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```