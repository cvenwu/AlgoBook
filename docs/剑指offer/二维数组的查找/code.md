# 二维数组的查找


## 方法一：迭代
!> 思路：每次从二维数组的右上角开始搜索，注意数组第一个维度或第二个维度为空的情况
```go
func findNumberIn2DArray(matrix [][]int, target int) bool {
	if len(matrix) == 0 || len(matrix[0]) == 0 {
		return false
	}
	rows, cols := len(matrix), len(matrix[0])
	i, j := 0, cols-1
	for i < rows && j >= 0 {
		if matrix[i][j] > target { //向左一列
			j--
		} else if matrix[i][j] < target { //向下一行
			i++
		} else {
			return true
		}
	}

	return false
}

```

!> c++代码是每次从左下方开始比较，如果大于左下方排除左下方所在列，

注意：vector的size()返回的是一个无符号的整形，如果减去一个比size()得到的结果还大的数字将会溢出，此时溢出的结果是一个很大的整形。

```c++
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        //假设当前在i,j位置
        int i = matrix.size()-1, j = 0;
        
        //确保不会越界
        while(i >= 0 && j+1 <= matrix[0].size())
        {
            if (matrix[i][j] > target) //排除当前行
                i--;
            else if (matrix[i][j] < target) //排除当前列
                j++;
            else
                return true;
        }

        //说明找不到目标值
        return false;
    }
};
```