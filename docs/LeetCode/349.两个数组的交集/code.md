# [349. 两个数组的交集](https://leetcode-cn.com/problems/intersection-of-two-arrays/)

## 方法一：暴力遍历

思路：对数组1的所有元素依次判断是否在num2中

 时间复杂度：O(N^2)
​         空间复杂度：O(1)

## 方法二：

思路：借助一个map统计其中一个数组出现的数字，当另外一个数组数字在map中就加入结果中，同时加入之后记得将map中的键值对失效(可以通过map中设置值为false或者设置值为0)

> 执行用时：4 ms, 在所有 Go 提交中击败了89.06%的用户
> 		内存消耗：3.1 ms, 在所有 Go 提交中击败了12.07%的用户

时间复杂度：O(M+N)
         空间复杂度：O(M)

**技巧：这里我们当数组2的元素出现在map中后，将map的键对应的值减去1，避免重复加入**
        


```go
func intersection(nums1 []int, nums2 []int) []int {
	if len(nums1) == 0 || len(nums2) == 0{
		return []int{}
	}

	mymap := make(map[int]int)
	for _, val := range nums1 {
		mymap[val] = 1
	}
	ret := []int{}
	for _, val := range nums2 {
		if mymap[val] == 1 {
			ret = append(ret, val)
			mymap[val] = 0		//技巧操作，避免同一个元素出现多次但都是交集重复加入ret中
		}
	}

	return ret
}
```

