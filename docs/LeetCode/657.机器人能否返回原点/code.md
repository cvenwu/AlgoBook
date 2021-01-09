# [657.机器人能否返回原点](https://leetcode-cn.com/problems/robot-return-to-origin/)

## 方法一
!> 使用两个变量模拟x,y坐标进行运算，同方法二的代码，只是将l,u改成x,y即可s

## 方法二
!> 使用两个变量统计向左移动以及向上移动的次数
1. 如果遇到向右移动，则抵消一次向左移动，
2. 如果遇到向下移动，则抵消一次向上移动，

如果最终都等于0那么说明都没有移动直接返回true否则返回false

```go
//执行用时：4 ms, 在所有 Go 提交中击败了87.29%的用户
//内存消耗：3.2 MB, 在所有 Go 提交中击败了61.86%的用户
func judgeCircle(moves string) bool {
	//要不长度小于等于0要不长度为奇数，直接返回false
	if len(moves) <= 1 || len(moves)&1 != 0 {
		return false
	}

	l, u := 0, 0
	for i := 0; i < len(moves); i++ {
		switch moves[i] {
		case 'L':
			l++
		case 'R':
			l--
		case 'U':
			u++
		case 'D':
			u--
		}
	}

	return l == 0 && u == 0
}
```

## 方法三：使用标准库中的计数代码

```go
// 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
// 内存消耗：3.2 MB, 在所有 Go 提交中击败了61.86%的用户
func judgeCircle(moves string) bool {
	return strings.Count(moves, "L") == strings.Count(moves, "R") &&
		strings.Count(moves, "U") == strings.Count(moves, "D")
}
```

