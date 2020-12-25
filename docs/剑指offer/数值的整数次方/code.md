# 数值的整数次方



思路：传入一个数以及对应的指数，返回数值的整数次方，有如下几种情况

1. 指数为0，直接返回1
2. 传入的数值为0，但是指数为负数，此时会让分母为0，返回-1，即错误情况
3. 如果指数为负数，使用flag进行标记，转换为正数计算结果，最后返回的时候判断flag后确定是否返回倒数

~~之前写的错误代码~~：

```go

func myPow(x float64, n int) float64 {
	//如果x是0并且n等于负数，非法情况，返回-1
	if x == 0 && n < 0 {
		return -1
	}

	if n == 0 {
		return 1
	} else if n == 1 {
		return x
	}

	flag := false
	if n < 0 {
		flag = true
		n = -n
	}

	ret := myPow(x, n>>1)
	if n&1 != 0 { //说明指数是奇数
		ret = ret * ret * x
	}

	if flag {
		ret = 1.0 / ret
	}
	return ret

}
```

错误原因在于：

第一次编写该代码，出错。错误原因在于`if n&1 != 0`这个代码块中，如果不为奇数，那么只会返回一个1，而不是ret*ret



**正确代码**：

> *// 执行用时：0 ms, 在所有 Go 提交中击败了 100.00% 的用户*
>
> *// 内存消耗：2 MB, 在所有 Go 提交中击败了 66.67% 的用户*

```go
func myPow(x float64, n int) float64 {
	//如果x是0并且n等于负数，非法情况，返回-1
	if x == 0 && n < 0 {
		return -1
	}

	if n == 0 {
		return 1
	} else if n == 1 {
		return x
	}

	flag := false
	if n < 0 {
		flag = true
		n = -n
	}

	ret := myPow(x, n>>1)
	ret *= ret
	if n&1 != 0 { //说明指数是奇数
		ret *= x
	}

	if flag {
		ret = 1.0 / ret
	}
	return ret

}
```

