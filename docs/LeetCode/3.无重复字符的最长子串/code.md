
# [3.无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)

## 方法一：滑动窗口
**技巧：这里窗口只是使用两个指针，并没有使用队列**

```go

func lengthOfLongestSubstring(s string) int {
	//定义双指针开确定窗口的左右边界
	l, r := 0, 0
	hmap := make(map[byte]int)
	ret := 0
	for r < len(s) {
		//获取当前应该加入窗口的值
		c := s[r]
		//窗口右移
		r++
		//更新窗口
		hmap[c]++

		//左侧收缩，为什么左侧会收缩，因为新加入的元素重复出现在了窗口中
		for hmap[c] > 1 {
			//更新窗口内的值
			hmap[s[l]]--
			//移动左侧窗口
			l++
		}

		//此时左侧收缩了之后窗口内就没有重复元素，此时我们就要动态更新长度
		if ret < r-l {
			ret = r - l
		}
	}
	return ret
}

```