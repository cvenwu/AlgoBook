# 最长不含重复字符的子字符串

## 【推荐】方法一：
思路：使用双指针表示滑动窗口

```sql

//使用双指针表示窗口
func lengthOfLongestSubstring(s string) int {
	hmap := make(map[byte]int)
	l, r, ret := 0, 0, 0
	for r < len(s) {
		//加入当前r指向的字符
		hmap[s[r]]++

		//如果当期加入的字符之前就有了，不断从左侧移出直到没有重复字符
		for hmap[s[r]] > 1 {
			hmap[s[l]]--
			l++
		}
		//此时右移我们的指针
		r++

		//动态更新我们要求的结果，因为此时r指向的位置还没有确定是否有重复，从l到r-1的位置我们已经确保没有重复字符
		if ret < r-l+1 {
			ret = r - l + 1
		}
	}
	return ret
}
```