# 求1+2+...+n

## 方法：

思路：利用短路与的特性
!> A && B ：只要A为true就会执行B
我们这里只要n > 0 就要执行递归函数，参数是n-1。但是我们需要额外传入一个参数保存我们最终要返回的结果，也就是1~n的和
```go
func sumNums(n int) int {
	ret := 0
	sumNumsCore(n, &ret)
	return ret
}

func sumNumsCore(n int, ret *int) bool {
	*ret += n
	return n > 0 && sumNumsCore(n-1, ret)
}
```