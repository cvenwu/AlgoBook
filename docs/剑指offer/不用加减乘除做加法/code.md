# 不用加减乘除做加法



思路：两个数相加，可以拆分成如下几步

1. 计算进位的结果
2. 计算不进位时的结果
3. 将两个相加得到我们的结果



```go
func add(a int, b int) int {
	//如果有一个为0
    if a == 0 || b == 0 {
        return a ^ b
    }

	//如果两个都不为0
	andRet, xorRet := a & b, a ^ b
	for andRet != 0 {
		andRet <<= 1
		andRet, xorRet = andRet & xorRet, andRet ^ xorRet
	}

    return xorRet
    
}
```

