# [剑指 Offer 58 - I. 翻转单词顺序](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/)


此题思路：将句子进行反转，然后反转句子中的每个单词

**两个注意的点：**

1. 句子的前后可能有多个空格
2. 单词与单词之间不止有一个空白字符，可能有多个空白字符，需要将多个转换为1个


步骤如下：
1. 去掉两边的空格字符，并进行截取
2. 对截取的字符去掉单词之间的空格
3. 反转整个句子
4. 将句子中的单词内部进行反转
5. 返回结果


>执行用时：0 ms, 在所有 Go 提交中击败了 100.00% 的用户
		内存消耗：3.3 MB, 在所有 Go 提交中击败了 99.14% 的用户

代码：[参考](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/solution/0ms-34mbshuang-bai-by-linkangsun/)


```go
func reverseWords(s string) string {


	//提取出句子中的每个单词，会将首尾以及字符串之间所有的空格都去掉
	wordList := strings.Fields(s)

	l, r := 0, len(wordList)-1
	for l < r {
		wordList[l], wordList[r] = wordList[r], wordList[l]
		l++
		r--
	}

	return strings.Join(wordList, " ")
}
```

