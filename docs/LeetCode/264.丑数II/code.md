# [264. 丑数 II](https://leetcode-cn.com/problems/ugly-number-ii/)



## 方法一：暴力穷举



## 方法二：

思路：空间换时间。下一个未知的丑数一定是前面我们求出的某个丑数 乘以 2，前面我们求出的某个丑数 乘以 3，前面我们求出的某个丑数 乘以 5，这三者都要求比我们已知的最后一个丑数大，且最接近于我们已知的最后一个丑数

```go
package main

/** ✅
 * @Author: yirufeng
 * @Date: 2020/12/8 3:48 下午
 * @Desc:
 **/

func min(a, b int) int {
	if a < b {
		return a
	}

	return b
}

func nthUglyNumber(n int) int {
	if n <= 0 {
		return 0
	}

	array := make([]int, n)

	array[0] = 1
	i, iMultiply2, iMultiply3, iMultiply5 := 1, 0, 0, 0
	for i < n {
		array[i] = min(array[iMultiply2]*2, min(array[iMultiply3]*3, array[iMultiply5]*5))

		//更新对应的索引iMultiply2, iMultiply3, iMultiply5
		for array[iMultiply2]*2 <= array[i] {
			iMultiply2++
		}

		for array[iMultiply3]*3 <= array[i] {
			iMultiply3++
		}

		for array[iMultiply5]*5 <= array[i] {
			iMultiply5++
		}

		i++
	}


	return array[n-1]
}

func main() {

}

```

