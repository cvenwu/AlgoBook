# [11. 盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)

## 方法一：暴力

1. 每次我们确定两条线，之后计算两条线可以容纳的水的体积，之后动态更新最大值

2. 两条线的横向距离我们可以求出，高度的话肯定是两条线高度的最小值，因此这两条线可以围成的面积为 两条线高度最小值 * 两条线的横向距离

```go
func maxArea(height []int) int {
	max := 0
	for i := 0; i < len(height)-1; i++ {
		for j := i+1; j < len(height); j++ {
			if max < (j - i) * min(height[i], height[j]) {
				max = (j - i) * min(height[i], height[j])
			}
		}
	}

	return max
}

func min(num1 int, num2 int) int {
	if num1 > num2 {
		return num2
	}

	return num1
}
```



## 方法二【推荐】：双指针

看了官方题解才知道的思路，

思路：双指针最开始分别指向左右边界，每次移动指针所在高度最小的那个指针

![hKldvp](https://cdn.jsdelivr.net/gh/sivanWu0222/ImageHosting@master/uPic/hKldvp.png)

> 执行用时：12 ms, 在所有 Go 提交中击败了96.41%的用户
> 		内存消耗：6.3 ms, 在所有 Go 提交中击败了5.90%的用户


```go
func maxArea(height []int) int {
	max := 0

	l, r := 0, len(height)-1
	for l < r {

		//动态更新max
		if max < (r-l)*min(height[l], height[r]) {
			max = (r - l) * min(height[l], height[r])
		}

		//高度小的移动
		if height[l] < height[r] {
			l++
		} else {
			r--
		}
	}

	return max
}

func min(num1 int, num2 int) int {
	if num1 > num2 {
		return num2
	}

	return num1
}
```

