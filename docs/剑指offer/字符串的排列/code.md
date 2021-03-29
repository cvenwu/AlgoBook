# [剑指 Offer 38. 字符串的排列](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/)

## 方法：回溯+剪枝

重点：[参考](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/solution/custerxue-xi-bi-ji-quan-pai-lie-hui-su-by-custergo/) [参考](https://leetcode-cn.com/problems/permutations-ii/solution/custerxue-xi-bi-ji-ji-shu-by-custergo/)

思路：因为当前循环一定是固定当前某一个位置，只不过通过循环将后面的元素交换到这里来，如果交换前发现已经和前面我们固定过的字符(我们存在了map中)重复，我们就直接跳过，


例如：aab 我们第1个固定a之后，如果将第2个a与第1个a交换，此时其实都是在一个函数的一个循环中，无非不过i为1, index为0，准备交换之前发现，要固定的字符第2个a之前我们已经固定过了，所以就不交换了

> 执行用时：52 ms, 在所有 Go 提交中击败了 79.85% 的用户
> 		内存消耗：7.6 MB, 在所有 Go 提交中击败了 45.19% 的用户


```go

func permutation(s string) []string {
	if len(s) == 0 {
		return []string{}
	}
	ret := []string{}
	backtrack([]byte(s), 0, &ret)

	return ret
}

func backtrack(content []byte, index int, ret *[]string) {
	//回溯结束条件，如果index为content的长度，说明我们要加入结果
	if len(content) == index {
		*ret = append(*ret, string(content))
	}

	mymap := make(map[byte]bool)
	//否则我们要不断做选择
	for i := index; i < len(content); i++ {

		//这里如果检测到之前我们固定过这个字符
		if _, ok := mymap[content[i]]; ok {
			continue
		}

		//固定一个字符，交换s[index]与s[i]
		content[i], content[index] = content[index], content[i]

		//然后递归到下一层
		backtrack(content, index+1, ret)

		//交换回原来的字符
		content[i], content[index] = content[index], content[i]

		//走到这里说明，当前i索引上的字符我们之前遍历过了，所以将其要记录下来，避免后面的重复遍历
		mymap[content[i]] = true
	}
}
```

