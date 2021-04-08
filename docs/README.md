# AlgoBook
刷题文档


滑动窗口问题：我们其实可以使用双指针来确定一个窗口，不需要使用一个队列
滑动窗口只能从右侧加入，左侧移出
滑动窗口模板：
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
		//更新窗口数据
		...

		//左侧收缩，为什么左侧会收缩，因为新加入的元素重复出现在了窗口中
		for hmap[c] > 1 {
            //c是将要移出窗口的值
            c = s[l]
			//更新窗口内的值
			hmap[c]--
			//移动左侧窗口
			l++
		}

		
	}
	return ret
}

```

## 一些刷题大佬
1. [宫水三叶](https://leetcode-cn.com/u/ac_oier/) 微信公众号：宫水三叶的刷题日记

