# [509.斐波那契数](https://leetcode-cn.com/problems/fibonacci-number/)


## 方法一: 递归
```go
//方法一：递归
// 执行用时：12 ms, 在所有 Go 提交中击败了27.00的用户
// 内存消耗：1.9 MB, 在所有 Go 提交中击败了78.36%的用户
func fib(n int) int {
	if n <= 1 {
		return n
	}

	return fib(n-1) + fib(n-2)
}

```

## 方法二: 带备忘录的递归


## 方法三: dp

```go
package main

/**
 * @Author: yirufeng
 * @Date: 2021/1/4 8:49 上午
 * @Desc: 509.斐波那契数
 **/

//方法二：dp
// 执行用时：0 ms, 在所有 Go 提交中击败了100.00的用户
// 内存消耗：1.9 MB, 在所有 Go 提交中击败了98.36%的用户
func fib(n int) int {
    if n <= 1 {
        return n
    }
    prev, cur := 0, 1
    for i := 2; i <= n; i++ {
        prev, cur = cur, prev+cur
    }
    return cur
}
```

## 方法四: 矩阵快速幂