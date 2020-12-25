# [709. 转换成小写字母](https://leetcode-cn.com/problems/to-lzzwer-case/)



## 方法：

> 思路：转换成bytes，然后每个字符进行判断，如果是大写转换为小写，最后转换为string

```go
/*
 */
func toLowerCase(str string) string {
	strBytes := []byte(str)

	for i := 0; i < len(strBytes); i++ {
		if strBytes[i] >= 'A' && strBytes[i] <= 'Z' {
			strBytes[i] += 32
		}
	}

	return string(strBytes)
}
```

