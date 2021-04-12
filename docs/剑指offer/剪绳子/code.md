# 剪绳子

## 方法一：贪心

!> 思路：尽可能多的裁剪3，如果3的个数超过1并且最后余1，那么我们需要将3的次数减去1，并将最后分成2*2
```go
func cuttingRope(n int) int {
	if n == 2 {
		return 1
	}
	if n == 3 {
		return 2
	}
	threeTimes, temp := n/3, n%3
	if temp == 1 && threeTimes > 0 {
		temp, threeTimes = 4, threeTimes-1
	}
	if temp == 0 {
		return int(math.Pow(3.0, float64(threeTimes)))
	}
	return temp * int(math.Pow(3.0, float64(threeTimes)))
}

```