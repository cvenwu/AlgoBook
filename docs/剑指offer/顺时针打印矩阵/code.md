# [剑指 Offer 29. 顺时针打印矩阵](https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)

## 方法：四个角不断定边界不断缩小范围

[参考](https://leetcode-cn.com/leetbook/read/illustration-of-algorithm/5vfb5v/)

思路：通过左上角，右上角，左下角，右下角4个点不断收缩来确定打印范围
![xG2SN1](https://cdn.jsdelivr.net/gh/sivanWu0222/ImageHosting@master/uPic/xG2SN1.png)




```go
func spiralOrder(matrix [][]int) []int {
	if len(matrix) == 0 || len(matrix[0]) == 0 {
		return nil
	}

	l, r, t, d := 0, len(matrix[0])-1, 0, len(matrix)-1
	res := []int{}
	for {
		//打印上面一行
		for i := l; i <= r; i++ {
			res = append(res, matrix[t][i])
		}
		//上边向下移动一行
		t += 1
		//说明打印完，退出
		if t > d {
			break
		}
		for i := t; i <= d; i++ {
			res = append(res, matrix[i][r])
		}
		//打印完右边一列
		r -= 1
		//说明打印完，退出
		if l > r {
			break
		}
		for i := r; i >= l; i-- {
			res = append(res, matrix[d][i])
		}
		d -= 1
		if t > d {
			break
		}
		for i := d; i >= t; i-- {
			res = append(res, matrix[i][l])
		}
		l += 1
		if l > r {
			break
		}
	}

	return res
}

```

