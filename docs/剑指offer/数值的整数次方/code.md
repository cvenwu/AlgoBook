# 数值的整数次方

## 方法一：分治
思路：传入一个数以及对应的指数，返回数值的整数次方，有如下几种情况

1. 指数为0，直接返回1
2. 传入的数值为0，但是指数为负数，此时会让分母为0，返回-1，即错误情况
3. 如果指数为负数，使用flag进行标记，转换为正数计算结果(此时使用分治计算指数为一般的时候的结果，然后再乘以自己)，然后指数如果是奇数说明还要乘以x，最后返回的时候判断flag后确定是否返回倒数

```go
func myPow(x float64, n int) float64 {
	if n == 0 {
		return 1
	}

	flag := true
	if n < 0 {
		n, flag = -n, false
	}

	num := myPow(x, n>>1)
	num *= num
	if n&1 == 1 {
		num *= x
	}
	if !flag  {
		return 1.0 / num
	}
	return num
}
```
