# [345. 反转字符串中的元音字母](https://leetcode-cn.com/problems/reverse-vowels-of-a-string/)

## 方法一：对撞指针思想进行反转

思路：我们通过对传入的字符串设置两个指针，分别指向最左和最右，分别从左向右和从右向左搜寻元音字母，两个都找到之后就进行替换(前提是left < right)，不然我们之前替换过的又会替换回来，也就是替换两次。

注意事项：

1. 这里aA都是元音字母
2. 同时l,r每次搜寻的时候注意不要越界


> 执行用时：4 ms, 在所有 Go 提交中击败了83.52%的用户
> 		内存消耗：4.1 ms, 在所有 Go 提交中击败了52.70%的用户


```go
func reverseVowels(s string) string {
	if len(s) <= 1 {  //如果长度小于等于1直接返回
		return s
	}

	l, r := 0, len(s)-1
	vowelMap := map[uint8]bool{
		'a': true,
		'e': true,
		'i': true,
		'o': true,
		'u': true,
		'A': true,
		'E': true,
		'I': true,
		'O': true,
		'U': true,
	}

	str := []byte(s)

	for l < r {

		//首先让l找到元音字母
		//注意点1：这里必须判断l不能越界，不然会一直往后走
		for l < len(str) && !vowelMap[str[l]] {
			l += 1
		}
		//让r从后面找到元音字母
		//注意点2：这里必须判断r不能走到最开始的左边
		for r >= 0 && !vowelMap[str[r]] {
			r -= 1
		}

		//很有可能最后l指向后面的元音字母，而right位于left前面也指向元音字母，但此时不可以交换
		if l < r {
			str[l], str[r] = str[r], str[l]
		}
		l, r = l + 1, r - 1
	}

	return string(str)
}

```

