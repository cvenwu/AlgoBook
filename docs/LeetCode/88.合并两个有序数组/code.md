# [88. 合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)







## 方法一：采用双指针

思路：最开始想到用双指针，从前往后合并，但是需要一个数组空间来存储nums1这个目标数组原来的内容，因此最好的建议是从后往前合并，不需要额外空间

时间复杂度：O(m+n)

空间复杂度：O(1)

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.3 MB, 在所有 Go 提交中击败了68.70%的用户

```go
func merge(nums1 []int, m int, nums2 []int, n int) {
	// 说明第2个数组初始化元素数量为0，都在第1个数组中
	if n == 0 {
		return
	}

	if m == 0 {
		for i := 0; i < n; i++ {
			nums1[i] = nums2[i]
		}
		return
	}

	//合并两个的元素
	eleIndex := m + n - 1
	for m != 0 && n != 0 {
		if nums1[m-1] > nums2[n-1] {
			nums1[eleIndex] = nums1[m-1]
			m -= 1
		} else {
			nums1[eleIndex] = nums2[n-1]
			n -= 1
		}
		eleIndex -= 1
	}

	//此时有一个数组一定弄完了
	if n != -1 {
		for i := n-1; i >= 0; i-- {
			nums1[i] = nums2[i]
		}
	}

	return
}

```

