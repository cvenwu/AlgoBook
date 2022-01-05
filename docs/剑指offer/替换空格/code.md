<!--
 * @Author: yirufeng
 * @Date: 2020-07-25 08:10:45
 * @LastEditTime: 2022-01-05 10:03:13
 * @LastEditors: yirufeng
 * @Description: 
 * @FilePath: /AlgoBook/docs/剑指offer/替换空格/code.md
-->
# 替换空格

## 方法一：使用strings.Builder
!> 注意：与许多支持string类型的语言一样，golang中的string类型也是只读且不可变的。因此，这种拼接字符串的方式会导致大量的string创建、销毁和内存分配。如果你拼接的字符串比较多的话，这显然不是一个好的方法。

```go
func replaceSpace(s string) string {
	var ret strings.Builder
	for i := 0; i < len(s); i++ {
		if s[i] == ' ' {
			ret.WriteString("%20")
		}else {
			ret.WriteByte(s[i])
		}
	}
	return ret.String()
}
```

## 方法二：使用库函数


```go
func replaceSpace(s string) string {
	return strings.Join(strings.Split(s, " "), "%20")
}
```

## 【推荐】方法三：

1. 首先获得空格的个数
2. 然后不断开始遍历我们的原始数组并填充我们的结果数组


```go
func replaceSpace(s string) string {
	backspaceCount := 0
	//扫描一遍字符串，看一下有多少个空格
	for _, v := range s {
		if v == ' ' {
			backspaceCount++
		}
	}

	ret := make([]byte, len(s)+2*backspaceCount)
	cur := 0
	for i := 0; i < len(s); i++ {
		if s[i] != ' ' {
			ret[cur], cur = s[i], cur+1
		} else {
			ret[cur] = '%'
			ret[cur+1] = '2'
			ret[cur+2] = '0'
			cur += 3
		}
	}
	return string(ret)
}

```