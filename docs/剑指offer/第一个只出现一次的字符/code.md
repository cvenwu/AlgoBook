# 第一个只出现一次的字符

思路：通过自定义的一个map来统计字符出现次数，两次遍历我们自定义map来获得最终结果。有一定的片面性，因为一个字符占8个字节，因此总共有256种可能，所以我们采用了一个长度为256的数组来自定义map。**将字符对应的ASCII码值作为map的键**

> *// 执行用时：8 ms, 在所有 Go 提交中击败了 82.52% 的用户*
>
> *// 内存消耗：5.9 MB, 在所有 Go 提交中击败了 7.32% 的用户*

```go
func firstUniqChar(s string) byte {
	if len(s) <= 0 {
		return ' ' // 如果没有找到返回一个单空格

	}

	bytes, mymap := []byte(s), [256]int{}
	for _, v := range bytes {
		mymap[v] += 1
	}
	for _, v := range bytes {
		if mymap[v] == 1 {
			return v  //找到返回对应的byte即可
		}
	}
	//说明没找到
	return ' ' // 如果没有找到返回一个单空格
}
```

