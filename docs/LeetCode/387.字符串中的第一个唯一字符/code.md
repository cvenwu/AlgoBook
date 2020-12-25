# [387. 字符串中的第一个唯一字符](https://leetcode-cn.com/problems/first-unique-character-in-a-string/)

## 方法一：map计数


1. 第一次遍历，使用map统计每个字符出现的次数

2. 第二次遍历，直接遍历字符串，如果对应字符在map中次数为1直接返回即可

   时间复杂度：O(N)
   空间复杂度：O(字符串长度)

> 执行用时：32 ms, 在所有 Go 提交中击败了59.25%的用户
		内存消耗：5.3 ms, 在所有 Go 提交中击败了89.64%的用户


```go
func firstUniqChar(s string) int {
	for len(s) == 0 {
		return -1
	}

	mymap := make(map[int32]int)

	for _, c := range s {
		mymap[c-97] += 1
	}

	for index, c := range s {
		if mymap[c-97] == 1 {
			return index
		}
	}
	return -1
}

```

## 方法二：使用一个长度为26的数组代替map

思路：每个索引代表一个元素出现的次数，


1. 第一次遍历，如果出现就加1，
2. 第二次遍历字符串，如果对应字符在数组中出现次数为1直接返回

> 执行用时：4 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：5.2 ms, 在所有 Go 提交中击败了92.40%的用户


```go
func firstUniqChar(s string) int {
	for len(s) == 0 {
		return -1
	}

	array := make([]int, 26)

	for _, c := range s {
		array[c-97] += 1
	}

	for index, c := range s {
		if array[c-97] == 1 {
			return index
		}
	}
	return -1
}

```

## 方法三：两次遍历

我们使用一个长度为26的数组，

1. 第1次遍历：将每个元素最后出现的索引放入到数组中
2. 第2次遍历：遍历字符串，并且每次查找对应字符在数组中的索引与当前索引是否相同，
   1. 如果相同说明是第一次出现也是最后一次出现的，直接返回。
   2. 如果不同，我们修改对应数组元素对应的索引为-1，
      当数组遍历完，说明没有找到，此时我们直接返回-1

> 执行用时：12 ms, 在所有 Go 提交中击败了61.00%的用户
> 		内存消耗：5.2 ms, 在所有 Go 提交中击败了92.40%的用户


```go
func firstUniqChar(s string) int {
	for len(s) == 0 {
		return -1
	}

	array := make([]int, 26)

	for index, c := range s {
		array[c-97] = index
	}

	for index, c := range s {
		//如果出现的最后一次索引等于第一次出现的索引
		if array[c-97] == index {
			return index
		}
		//说明出现很多次
		array[c-97] = -1
	}

	return -1
}

```

