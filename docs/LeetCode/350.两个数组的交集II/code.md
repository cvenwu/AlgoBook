# [350. 两个数组的交集 II](https://leetcode-cn.com/problems/intersection-of-two-arrays-ii/)

思路：
1. 使用map统计数组1中出现的元素，其中**键作为元素，值作为次数**
2. 之后遍历数组2，如果在map中出现了并且次数不为0，次数减去1，同时将其加入结果，


> 执行用时：4 ms, 在所有 Go 提交中击败了91.16%的用户
> 		内存消耗：3.2 MB, 在所有 Go 提交中击败了9.48%的用户


```go
func intersect(nums1 []int, nums2 []int) []int {
	mymap := make(map[int]int)
	ret := []int{}
	for _, num := range nums1 {
		mymap[num] += 1
	}

	for _, num := range nums2 {
		if mymap[num] != 0 {
			mymap[num] -= 1
			ret = append(ret, num)
		}
	}

	return ret
}
```

