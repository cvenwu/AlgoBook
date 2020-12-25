# [412. Fizz Buzz](https://leetcode-cn.com/problems/fizz-buzz/)

## 方法一：自己的解法()

思路：直接按照题目中的要求打印即可

缺点：拓展性不强，如果面试官有多个数字可以被整除的要求，代码逻辑太多且臃肿

> 执行用时：4 ms, 在所有 Go 提交中击败了95.95%的用户
> 		内存消耗：3.4 MB, 在所有 Go 提交中击败了78.43%的用户

```go
func fizzBuzz(n int) []string {
	if n <= 0 {
		return []string{}
	}

	ret := make([]string, n)
	for i := 1; i <= n; i++ {
		if i%3 == 0 && i%5 == 0 {
			ret[i-1] += "FizzBuzz"
		} else if i%3 == 0 {
			ret[i-1] += "Fizz"
		} else if i%5 == 0 {
			ret[i-1] += "Buzz"
		} else {
			ret[i-1] += strconv.Itoa(i)
		}
	}

	return ret
}

```

## 方法二：LeetCode官方解法二

优点：当判断条件过多我们可以简化代码

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：3.4 MB, 在所有 Go 提交中击败了88.24%的用户

```go
func fizzBuzz(n int) []string {
	if n <= 0 {
		return []string{}
	}
	ret := make([]string, n)
	for i := 1; i <= n; i++ {
		if i%3 == 0 {
			ret[i-1] += "Fizz"
		}
		if i%5 == 0 {
			ret[i-1] += "Buzz"
		}
		if i%3 != 0 && i%5 != 0 {
			ret[i-1] += strconv.Itoa(i)
		}
	}
	return ret
}

```

## 方法三：LeetCode官方解法三

对于面试官要求的更自由的映射关系我们可以采用散列表，但是需要注意的是，我们要采用有序的散列表，因为打印的时候我们从散列表中要按照输入的匹配顺序来取，例如我们要先能被3整除，其次才能被5整除，如果采用了无序的散列表，首先发现能够被5整除，其次能被3整除，打印结果将会与我们的正确结果不一致。

