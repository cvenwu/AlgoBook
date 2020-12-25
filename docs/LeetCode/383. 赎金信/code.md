# [383. 赎金信](https://leetcode-cn.com/problems/ransom-note/)

## 方法一：使用map

思路：将第2个字符串用map统计，键为对应的字符，值为次数，之后遍历字符串1，出现1次，次数减去1，如果出现负数就返回false

> 执行用时：8 ms, 在所有 Go 提交中击败了70.25%的用户
> 		内存消耗：3.9 ms, 在所有 Go 提交中击败了74.64%的用户

```go

func canConstruct(ransomNote string, magazine string) bool {
	mymap := make(map[int32]int)

	for _, c := range magazine {
		mymap[c] += 1
	}

	for _, c := range ransomNote {
		mymap[c] -= 1
		if mymap[c] < 0 {
			return false
		}
	}

	return true
}
```

## 【推荐】方法二：使用数组代替map

【推荐】因为哈希表的常数项比较大

>  执行用时：4 ms, 在所有 Go 提交中击败了59.25%的用户
> 		内存消耗：3.8 ms, 在所有 Go 提交中击败了89.64%的用户

```go
func canConstruct(ransomNote string, magazine string) bool {
	mymap := make([]int, 26)

	for _, c := range magazine {
		mymap[c-'a'] += 1
	}

	for _, c := range ransomNote {
		mymap[c-'a'] += 1
		if mymap[c-'a'] < 0 {
			return false
		}
	}

	return true
}
```

