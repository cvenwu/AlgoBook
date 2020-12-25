# [389. 找不同](https://leetcode-cn.com/problems/find-the-difference/)

## 方法一：

思路：将字符串拼接之后，将字符串中的所有字符进行异或

```
其中一个字符串随机加了一个字母并重排形成另一个字符串，因此两个字符串拼接在一起，相当于某个字符只出现1次，其他字符全都出现2次，
因此考虑遍历每个字符转换为整形并用异或求得结果
```


> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.3 MB, 在所有 Go 提交中击败了8.62%的用户


```go
func findTheDifference(s string, t string) byte {
	ret := 0
	for _, val := range s + t {
		ret ^= int(val)
	}

	return byte(ret)
}
```

## 【推荐】方法二：方法一的改进

改进点：直接使用一个变量保存异或结果即可


> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.2 MB, 在所有 Go 提交中击败了53.45%的用户


```go
func findTheDifference_(s string, t string) byte {
	var ret uint8
	for i := 0; i < len(s); i++ {
		ret ^= t[i]
		ret ^= s[i]
	}

	return ret ^ t[len(t)-1]
}
```


## 总结：


注意：
1. 遍历字符串我们获得每个元素的数据类型为int32。
2. 如果通过对字符串索引取值获得的数据类型是uint8，等价于byte
