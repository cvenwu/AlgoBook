# [7. 整数反转](https://leetcode-cn.com/problems/reverse-integer/)

## 方法：直接进行反转

注意事项：注意反转过程中可能会越界，一旦越界直接返回0

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.2 ms, 在所有 Go 提交中击败了100.00%的用户

```go
func reverse(x int) int {
	if x == 0 {
		return x
	}
	//flag用来表示是否为负数
	flag := false
	temp := x
	if temp < 0 {
		flag = true
		temp *= (-1)
	}
	num := 0
	for temp != 0 {
		//说明会溢出
		if num >= 214748365 || ((num >= 214748364) && temp >= 8) {
			return 0
		}
		num = num * 10 + temp % 10
		temp /= 10
	}

	if flag {
		num *= (-1)
	}

	return num
}

```

## 方法二【推荐】：针对方法一的缺陷进行改进

> 上面的代码有缺陷，一旦输入一个负数为-2147483647，那么乘以-1之后就会越界，所以我们使用下面的代码进行纠正。

> 执行用时：4 ms, 在所有 Go 提交中击败了47.38%的用户
> 		内存消耗：2.2 ms, 在所有 Go 提交中击败了67.85%的用户


```go
func reverse(x int) int {
	if x == 0 {
		return x
	}
	//flag用来表示是否为负数
	//flag := false
	temp := x
	//if temp < 0 {
	//	flag = true
	//	temp *= (-1)
	//}
	num := 0
	for temp != 0 {
		//说明会溢出
		if num >= 214748365 || ((num >= 214748364) && temp >= 8) ||
			num <= -214748365 || ((num <= -214748364) && temp < -8){
			return 0
		}
		num = num * 10 + temp % 10
		temp /= 10
	}
	//
	//if flag {
	//	num *= (-1)
	//}

	return num
}
```

