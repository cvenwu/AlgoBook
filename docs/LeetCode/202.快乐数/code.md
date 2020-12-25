# [202. 快乐数](https://leetcode-cn.com/problems/happy-number/)

## 方法一：使用hashset

性质：如果一个数的各位平方和加起来最后到不了1，那么肯定陷入无限循环
		思路：我们用map记录出现的数的各位平方和，当下次在map中，直接返回false,否则继续，并将没出现的数加入map，直到1出现

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.2 ms, 在所有 Go 提交中击败了8.14%的用户

```go
func isHappy(n int) bool {
	if n <= 0 { //正整数才有可能是快乐数
		return false
	}

	mymap := make(map[int]int)
	mymap[n] = 1
	sum := n
	for sum != 1 {
		sum = getDigitSquareSum(sum)
		if mymap[sum] == 0 { //说明没有出现过，加入map中
			mymap[sum] = 1
		} else { //说明出现过
			return false
		}
	}

	//走到这里说明最后各位平方和为1
	return true

}

//返回num的各个位的平方和
func getDigitSquareSum(num int) int {
	sum := 0
	for num != 0 {
		sum += (num % 10) * (num % 10)
		num = num / 10
	}
	return sum
}
```

## 方法二：快慢指针

思路：既然我们上面提到了，各位数字平方和最终到不了1，就一定会陷入无限循环，因此这道题目可以等价于链表找环，如果存在环就说明一定会陷入无限循环，如果不存在环，我们最终需要判断是否可以到达1



**改进：不需要使用额外空间**

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2 ms, 在所有 Go 提交中击败了87.13%的用户

```go
func getDigitSquareSum(num int) int {
	sum := 0
	for num != 0 {
		sum += (num % 10) * (num % 10)
		num = num / 10
	}
	return sum
}


func isHappy(n int) bool {
	if n <= 0 {
		return false
	}

	slow := getDigitSquareSum(n)
	fast := getDigitSquareSum(slow)
	for slow != fast && fast != 1 && slow != 1{
		slow = getDigitSquareSum(slow)
		fast = getDigitSquareSum(getDigitSquareSum(fast))
	}

	return fast == 1 || slow == 1
}

```

## 方法三：数学法

[参考LeetCode官方解法三](https://leetcode-cn.com/problems/happy-number/solution/kuai-le-shu-by-leetcode-solution/)