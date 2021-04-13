# 圆圈中最后剩下的数字

## 方法一：
思路：![IMG_D613E7FEFD2A-1](https://cdn.jsdelivr.net/gh/sivanWu0222/ImageHosting@master/uPic/IMG_D613E7FEFD2A-1.jpeg)

!> [参考](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/solution/javajie-jue-yue-se-fu-huan-wen-ti-gao-su-ni-wei-sh/)

```go
func lastRemaining(n int, m int) int {
	ans := 0
	for i := 2; i <= n; i++ {
		ans = (ans + m) % i
	}
	return ans
}
```
