# [566. 重塑矩阵](https://leetcode-cn.com/problems/reshape-the-matrix/)

## 方法一：数学思想，找到行列互换的关系式

步骤：

1. 首先检查参数是否合理，行数以及列数是否为0
   其次检查行列以及传入的行列乘积是否相等，如果不等直接返回原数组

2. 新建一个结果，同时将合法的内容添加进去
   假设原来有n行、
   新的数组的第i行第j列等同于原来的第i*j/n行，第i*j%n列

3. 假设原来的矩阵有oldRows行，有oldCols列
   新矩阵有r行，有c列

4. 转换关系如下：
   新矩阵的ret[i][j]：其实是第`i*c+j+1`个元素，`ret[i][j] = nums[所在原来矩阵的行][所在原来矩阵的列]`  所在原来矩阵的行：`(i*c+j)/oldCols`  所在原来矩阵的列：`(i*c+j)%oldCols`


> 执行用时：12 ms, 在所有 Go 提交中击败了87.18%的用户
> 		内存消耗：6.6 MB, 在所有 Go 提交中击败了7.41%的用户

```go

func matrixReshape(nums [][]int, r int, c int) [][]int {

	if len(nums) == 0 || len(nums[0]) == 0 || r*c != len(nums)*len(nums[0]) {
		return nums
	}

	oldCols := len(nums[0])
	ret := [][]int{}

	rowRet := []int{}

	for i := 0; i < r; i++ {
		rowRet = []int{}
		for j := 0; j < c; j++ {
			rowRet = append(rowRet, nums[(i*c+j)/oldCols][(i*c+j)%oldCols])
		}
		ret = append(ret, rowRet)
	}
	return ret
}
```

## 方法二：直接不断赋值然后新数组换行之后如果行满就新生成一行



> 执行用时：12 ms, 在所有 Go 提交中击败了87.18%的用户
> 		内存消耗：6.6 MB, 在所有 Go 提交中击败了7.41%的用户

```go

func matrixReshape(nums [][]int, r int, c int) [][]int {
	//能否reshape
	if len(nums) == 0 || len(nums[0]) == 0 || r*c != len(nums)*len(nums[0]) {
		return nums
	}


	//直接遍历进行reshape
	count := 0
	ret := [][]int{}
	rowRet := []int{}
	for _, row := range nums {
		for _, val := range row {
			rowRet = append(rowRet, val)
			count += 1
			if count % c == 0 {
				ret = append(ret, rowRet)
				rowRet = []int{}
			}
		}
	}

	return ret
}
```

## 方法三：

改进：直接make好之后进行赋值，不要使用append动态分配内存，达到节省空间的作用

>  执行用时：12 ms, 在所有 Go 提交中击败了87.18%的用户
> 		 内存消耗：6.6 MB, 在所有 Go 提交中击败了7.41%的用户

```go
func matrixReshape(nums [][]int, r int, c int) [][]int {
	if len(nums) == 0 || len(nums[0]) == 0 || r*c != len(nums)*len(nums[0]) {
		return nums
	}

	oldCols := len(nums[0])
	ret := make([][]int, r)

	for i := 0; i < r; i++ {
		ret[i] = make([]int, c)
	}

	for i := 0; i < r; i++ {
		for j := 0; j < c; j++ {
			ret[i][j] = nums[(i*c+j)/oldCols][(i*c+j)%oldCols]
		}
	}
	return ret
}

```

