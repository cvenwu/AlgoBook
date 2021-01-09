# [1570.两个稀疏向量的点积](https://leetcode-cn.com/problems/dot-product-of-two-sparse-vectors/)

## 方法一
!> 我们直接使用两个切片进行对应元素相乘然后求和即可
```go

//方法一：我们直接使用两个切片进行对应元素相乘然后求和即可
//执行用时：240 ms, 在所有 Go 提交中击败了80.00%的用户
//内存消耗：8.2 MB, 在所有 Go 提交中击败了50.00%的用户
type SparseVector struct {
	data []int
}

func Constructor(nums []int) SparseVector {
	return SparseVector{
		data: nums,
	}
}

// Return the dotProduct of two sparse vectors
func (this *SparseVector) dotProduct(vec SparseVector) int {
	sum := 0

	for i := 0; i < len(this.data); i++ {
		sum += this.data[i] * vec.data[i]
	}

	return sum
}

```


## 方法二

!> 因为是稀疏矩阵，所以使用map存储索引和对应的值，之后遍历一个map并且与另外一个相同索引的元素直接可以相乘求和

```gos
//方法二：因为是稀疏矩阵，所以使用map存储索引和对应的值，之后遍历一个map并且与另外一个相同索引的元素直接可以相乘求和
//执行用时：244 ms, 在所有 Go 提交中击败了60.00%的用户
//内存消耗：8.6 MB, 在所有 Go 提交中击败了25.00%的用户
type SparseVector struct {
	DataMap map[int]int
}

func Constructor(nums []int) SparseVector {
	dataMap := make(map[int]int)
	for i := 0; i < len(nums); i++ {
		if nums[i] != 0 {
			dataMap[i] = nums[i]
		}
	}

	return SparseVector{
		DataMap: dataMap,
	}
}

// Return the dotProduct of two sparse vectors
func (this *SparseVector) dotProduct(vec SparseVector) int {
	sum := 0

	for k, v := range this.DataMap {
		if v2, ok := vec.DataMap[k]; ok {
			sum += v * v2
		}
	}

	return sum
}

/**
 * Your SparseVector object will be instantiated and called as such:
 * v1 := Constructor(nums1);
 * v2 := Constructor(nums2);
 * ans := v1.dotProduct(v2);
 */
```

