# [258. 各位相加](https://leetcode-cn.com/problems/add-digits/)

## 方法一：自己想到的解法，暴力迭代


> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.2 MB, 在所有 Go 提交中击败了17.16%的用户


```go
func addDigits(num int) int {

	sum := getDigitSum(num)
	for sum >= 10 {
		sum = getDigitSum(sum)
	}
	return sum
}

func getDigitSum(num int) int {
	sum := 0
	for num != 0 {
		sum += num % 10
		num /= 10
	}
	return sum
}
```

## 方法二：性质：一个数的树根就是它对9的余数

思路：各位相加对9取余(就是数根)等于该数对9取余

步骤：因此如果该数对9取余之后如果返回0说明该数可以被9整除，因此直接返回9，**0比较特殊**

[下面例子讲解参考](参考https://www.zhihu.com/question/30972581/answer/50203344)

![Fenbnt](https://cdn.jsdelivr.net/gh/sivanWu0222/ImageHosting@master/uPic/Fenbnt.png)

例子：例如12345 可以写成(1 + 9999) * 1 + (999 + 1) * 2 + (99 + 1) * 3 + (9 + 1) * 4 + 5 * 1 = (9999 + 999 + 99 + 9) + (1 * 1 + 2 * 1 + 3 * 1 + 4 * 1 + 5 * 1)
此时将上面化简得到的结果%9可以得到各位数的和

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.2 MB, 在所有 Go 提交中击败了17.16%的用户


```go
func addDigits(num int) int {
	if num == 0 {
		return num
	}else if num % 9 == 0 {
		return 9
	} else {
		return num % 9
	}
}
```

## 方法三：找规律

[参考](https://leetcode-cn.com/problems/add-digits/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-5-7/)

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
	> 		内存消耗：2.2 MB, 在所有 Go 提交中击败了17.16%的用户

```
原数: 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30
偏移: 0 1 2 3 4 5 6 7 8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29
取余: 0 1 2 3 4 5 6 7 8  0  1  2  3  4  5  6  7  8  0  1  2  3  4  5  6  7  8  0  1  2
数根: 1 2 3 4 5 6 7 8 9  1  2  3  4  5  6  7  8  9  1  2  3  4  5  6  7  8  9  1  2  3
```

```go
func addDigits(num int) int {
	return (num - 1) % 9 + 1
}
```

