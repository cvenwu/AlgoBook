

# [14. 最长公共前缀](https://leetcode-cn.com/problems/longest-common-prefix/)

## 方法：

核心：因为是求公共最长前缀，所以只要任意的两个字符串的公共前缀也可能是最长公共前缀
		思路：

1. 假设第1个字符为公共前缀(prefix)，
2. 然后不断与后面的进行匹配，查看当后面的字符串作为主串，我们这个prefix作为子串，
3. 看一下是否匹配，
   3.1 如果匹配则继续与将下一个字符串作为主串，重复该操作，
   3.2 如果不匹配，则我们不断将prefix尾巴不断去掉直到匹配为止，如果中间prefix为空，我们可以直接返回一个空串

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.3 ms, 在所有 Go 提交中击败了92.78%的用户


```go
func longestCommonPrefix(strs []string) string {

	if len(strs) < 1 {
		return ""
	}
	prefix := strs[0]
	for _, str := range strs {
		//如果prefix为空字符串，strings.Index()也会返回0，但是每次都是0所以循环到最后还是会返回return prefix。
		for strings.Index(str, prefix) != 0 { //说明不匹配，此时我们需要缩减prefix
			if len(prefix) == 0 {
				return ""
			}
			prefix = prefix[:len(prefix)-1]
		}
	}

	return prefix
}

```

