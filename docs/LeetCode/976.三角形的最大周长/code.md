# [976. 三角形的最大周长](https://leetcode-cn.com/problems/largest-perimeter-triangle/)

## 方法一：

思路：对传入的数组进行排序，然后从最后一个位置开始遍历假设为i，同时另外两条边为j,k，初始令`j = i-2, k = i-1`，最终依次找到可组成三角形的三条边并返回。

注意：最后返回的i, j, k对应的三个元素一定相邻，因为如果某次A[k] + A[k] <= A[i]，那么下次i一定会左移1位，且k与j分别移动到i的左边1个位置和左边两个位置，如果下次两边之和依然小于第三边，继续循环。否则，返回刺客最大的三个边对应的边长，因此此刻返回的三条边必定在排序后的数组中是相邻的。

**知识点：如何判断是否是一个三角形：两边之和大于第三边就可以构成一个三角形。**

```go
func largestPerimeter(A []int) int {
	//如果数组中小于3个数，直接返回0
	if len(A) < 3 {
		return 0
	}

	//排序
	sort.Ints(A)

	j, k, i := len(A)-3, len(A)-2, len(A)-1
	for j >= 0 && A[j] + A[k] <= A[i] {
		j, k, i = i - 3, i- 2, i-1
	}

	//如果越界直接返回0
	if j < 0 {
		return 0
	}

	return A[j] + A[k] + A[i]
}
```

