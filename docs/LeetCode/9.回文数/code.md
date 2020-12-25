# [9. 回文数](https://leetcode-cn.com/problems/palindrome-number/)

## 方法一：反转后直接比较和原数是否相等

注意：反转过程中会溢出，**因此不建议采用这种做法**

## 方法二：

思路：建立一个数字num，使得与x的最高位都在同一位，但是num的最高位为1，之后依次判断x的最高位和最低位是否相同，

1. 如果不相同则直接返回false
2. 如果相同需要去掉x的最高位和最低位继续判断
3. 不断重复以上步骤，直到x只剩一位



> 执行用时：8 ms, 在所有 Go 提交中击败了98.04%的用户
> 		内存消耗：5.2 ms, 在所有 Go 提交中击败了100.00%的用户


​```go

func isPalindrome(x int) bool {
	if x < 0 {
		return false
	}

	if x == 0 {
		return true
	}

	num := 1
	//构造最高位的1
	for x/(num*10) > 0 {
		num *= 10
	}

	//依次判断x的最高位和最低位是否同，并不断去掉最高位和最低位
	for num >= 10 {
		//最低位和最高位不同
		if x%10 != (x / num % 10) {
			return false
		}
		//x去掉%最高位和最低位
		x = x % num / 10
		//num缩小
		num /= 100
	}

	return true
}

```

## 方法三：将数字转换为字符串进行反转看是否匹配



## 方法四：参照LeetCode解法

思路：因为数字是回文数，数字一定是左右对称的，所以数字反转一半看是否匹配

