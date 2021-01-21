# [49.字母异位词分组](https://leetcode-cn.com/problems/group-anagrams/)


## 方法一：

!> 思路：字母异位词只是字母排列不同，但是字母种类以及个数相同，因此可以排序后一定是同一个字符串，我们可以使用排序后的字符串作为键，对应的未排序前的字符串组成的集合作为值

```go
func groupAnagrams(strs []string) [][]string {

	hashMap := make(map[string][]string)
	var ret [][]string

	for _, str := range strs {
		byteStr := []byte(str)
		//按照升序排序
		sort.Slice(byteStr, func(i, j int) bool {
			return byteStr[i] < byteStr[j]
		})
		hashMap[string(byteStr)] = append(hashMap[string(byteStr)], str)
	}

	for _, v := range hashMap {
		ret = append(ret, v)
	}

	return ret
}
```

## 【推荐】方法二：
!> 思路：使用长度为26的数组进行计数，每个字符串如果计数数组一样那么直接作为键加入到对应的哈希表字符串集合中

```go

//方法二【推荐】：使用一个数组计数作为key加入到对应的哈希表中
func groupAnagrams(strs []string) [][]string {

	var ret [][]string
	hashMap := make(map[[26]int][]string)

	for _, str := range strs {
		cnt := [26]int{}

		for i := 0; i < len(str); i++ {
			cnt[str[i]-'a']++
		}

		hashMap[cnt] = append(hashMap[cnt], str)
	}

	for _, v := range hashMap {
		ret = append(ret, v)
	}

	return ret
}

```