# 旋转数组的最小数字

## 【推荐】方法一：
!> 思路：题目要求的是最小值，也就是旋转点的值

首先我们明确l,r这个范围里面一定包含旋转点，初始化的时候l是0，r是数组长度减去1

按照如下步骤进行：
1. 初始化我们的l和r
2. for 循环，一直到l和r相等时返回numbers[l]，此时就是我们的旋转点的值
	1. 首先找到mid位置
	2. 如果我们l对应的值小于r的值，直接返回l的值即可，因为l到r这个区间一定是包括我们的旋转点的
	3. 如果我们l对应的值大于mid的值，说明旋转点在mid及其左侧，让r=mid，这里最后不会有死循环，因为我们for循环结束条件是for l < r
	4. 如果我们mid对应的值大于r对应的值，说明旋转点在mid右侧，让l=mid+1
	5. 我们此时无法判断旋转点的位置，只能一个一个去挪动，因此让l++
3. 最后返回numbers[l]

```go
/*
思路：题目要求的是最小值，也就是旋转点的值
首先我们明确l,r这个范围里面一定包含旋转点，初始化的时候l是0，r是数组长度减去1

按照如下步骤进行：
1. 初始化我们的l和r
2. for 循环，一直到l和r相等时返回numbers[l]，此时就是我们的旋转点的值
	2.1 首先找到mid位置
	2.2 如果我们l对应的值小于r的值，直接返回l的值即可，因为l到r这个区间一定是包括我们的旋转点的
	2.3 如果我们l对应的值大于mid的值，说明旋转点在mid及其左侧，让r=mid，这里最后不会有死循环，因为我们for循环结束条件是for l < r
	2.4 如果我们mid对应的值大于r对应的值，说明旋转点在mid右侧，让l=mid+1
	2.5 我们此时无法判断旋转点的位置，只能一个一个去挪动，因此让l++
3. 最后返回numbers[l]
 */
func minArray(numbers []int) int {
	l, r := 0, len(numbers)-1

	for l < r {
		mid := l + (r-l)>>1

		//如果左小于右，直接返回左边的值，因为我们l到r这个区间一定有旋转点，如果numbers[l] < numbers[r]直接返回numbers[l]
		if numbers[l] < numbers[r] {
			return numbers[l]
		} else if numbers[l] > numbers[mid] { //旋转点在mid以及mid的左侧
			r = mid
		} else if numbers[mid] > numbers[r] { //说明旋转点在mid右侧
			l = mid + 1
		} else { //因为可能左==mid==右，此时我们就要一个一个去找，不要错过任何一个
			l++
		}
	}

	return numbers[l]
}
```