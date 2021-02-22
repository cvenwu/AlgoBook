# [48. 旋转图像](https://leetcode-cn.com/problems/rotate-image/)

## 方法：找规律

思路：很简单，如图所示![IMG_0430](https://cdn.jsdelivr.net/gh/sivanWu0222/ImageHosting@master/uPic/IMG_0430.PNG)
1. 需要对多少个边界的正方形进行反转，例如5*5需要对2个正方形进行反转
2. 每次反转该正方形，需要进行多少回合(每回合4个点顺时针切换即可)，发现需要该正方形边长-1个回合
3. 找到4个点每次移动后的坐标进行反转

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.2 ms, 在所有 Go 提交中击败了47.59%的用户

```go

func rotate(matrix [][]int) {

	if len(matrix) <= 1 || len(matrix) != len(matrix[0]) {
		return
	}

	rows := len(matrix)

	//一共有多少个正方形要去反转
	for i := 0; i < rows>>1; i++ {
		//每次反转一个正方形需要去对该正方形反转多少回合
		//此时该正方形的起始点为(i, i)
		for j := 0; j < rows-1-i*2; j++ {
			//每回合要反转4个点，依次对应顺时针的4个点
			matrix[i][i+j], matrix[i+j][rows-1-i], matrix[rows-1-i][rows-1-i-j],
				matrix[rows-1-i-j][i] = matrix[rows-1-i-j][i], matrix[i][i+j], matrix[i+j][rows-1-i],
				matrix[rows-1-i][rows-1-i-j]
		}

	}

}
```

