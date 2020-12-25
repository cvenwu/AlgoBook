# [240. 搜索二维矩阵 II](https://leetcode-cn.com/problems/search-a-2d-matrix-ii/)

![YOtsNf](https://cdn.jsdelivr.net/gh/sivanWu0222/ImageHosting@master/uPic/YOtsNf.png)

因为题目中说明了上一行的最后一个值小于下一行的第一个值，因此我们可以将其转换为一个有序数组，然后采用标准的二分进行搜索即可。

> 方法一二四不再赘述

## 方法三：

```go
/** ✅
 * @Author: yirufeng
 * @Date: 2020/12/7 11:41 上午
 * @Desc:
 * 方法一：暴力
 * 方法二：从右上角或者左下角每次排除一行或者一列
 * 方法三：每一行二分搜索
 * 方法四：采用二分
		思路：获取当前搜索矩阵的中心点，然后排出左上角或者右下角的矩阵
	如下是方法四代码
 **/
func searchTarget(matrix [][]int, x1, y1, x2, y2, target int) bool {
	//递归可能越界
	if x1 > x2 || y1 > y2 {
		return false
	}

	//之前这个if语句块没有写会死循环，
	//注意点：避免x1，y1，x2，y2相等时调用searchTarget(matrix, x1, y1, xMid, yMid, target)死循环
	if x1 == x2 && y1 == y2 {
		if matrix[x1][y1] == target {
			return true
		}
		return false
	}

	//得到中心点坐标
	xMid, yMid := x1+(x2-x1)>>1, y1+(y2-y1)>>1
	if matrix[xMid][yMid] > target { //排除右下角
		return searchTarget(matrix, x1, y1, xMid, yMid, target) ||
			searchTarget(matrix, x1, yMid+1, xMid-1, y2, target) ||
			searchTarget(matrix, xMid+1, y1, x2, yMid-1, target)
	} else if matrix[xMid][yMid] < target { //排除左上角
		return searchTarget(matrix, x1, yMid+1, xMid, y2, target) ||
			searchTarget(matrix, xMid+1, y1, x2, yMid, target) ||
			searchTarget(matrix, xMid+1, yMid+1, x2, y2, target)
	}
	return true

}

func searchMatrix(matrix [][]int, target int) bool {
	if len(matrix) == 0 || len(matrix[0]) == 0 {
		return false
	}
	return searchTarget(matrix, 0, 0, len(matrix)-1, len(matrix[0])-1, target)
}


```



## 【推荐】方法五：

> 针对该题特有的解法：因为多加了一个条件，所以整个矩阵可以按行遍历之后组成一个有序数组，从而直接使用标准的二分去查找即可

```go

/**
 * ✅
 * 方法一：暴力
 * 方法二：每行或每列进行二分
 * 方法三：从右上角或者左下角不断缩减行或列
 * 方法四：每次找到矩阵的中心点，然后划分为4个小矩阵，每次排除一个小矩阵
 * 方法五【当前方法】：因为多加了一个条件，所以整个矩阵可以按行遍历之后组成一个有序数组，从而直接使用标准的二分去查找即可
 * 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
 * 内存消耗：2.7 MB, 在所有 Go 提交中击败了75.17%的用户
 * @param matrix int整型二维数组
 * @param target int整型
 * @return bool布尔型
 */
func searchMatrix(matrix [][]int, target int) bool {
	// write code here

	if len(matrix) == 0 || len(matrix[0]) == 0 {
		return false
	}
	row, col := len(matrix), len(matrix[0])
	l, r := 0, row*col-1
	for l <= r {
		mid := l + (r-l)>>1
		if matrix[mid/col][mid%col] > target { //说明
			r = mid - 1
		} else if matrix[mid/col][mid%col] < target {
			l = mid + 1
		} else if matrix[mid/col][mid%col] == target {
			return true
		}
	}

	//说明没找到
	return false
}

```

