# [406.根据身高重建队列](https://leetcode-cn.com/problems/queue-reconstruction-by-height/)

## 方法一：

> 执行用时：24 ms, 在所有 Go 提交中击败了53.20%的用户
> 内存消耗：7.9 MB, 在所有 Go 提交中击败了5.09%的用户

思路：
1. 根据（身高降序）与（比他高和他身高相等的人数升序）排序
2. 新建一个结果数组，不断遍历排序后的数组。当前遍历到的为当前要加入的元素，如果不是在数组中的k位置上，则加入到数组的k位置，后面的元素都后移(并不会改变数组的顺序，因为当前元素后面k都比他大，而且前面身高都比他大)
3. 返回我们的结果数组


```go
func reconstructQueue(people [][]int) [][]int {

	if len(people) == 0 {
		return nil
	}

	sort.Slice(people, func(i, j int) bool {
		//身高降序
		if people[i][0] > people[j][0] {
			return true
		}
		if people[i][0] < people[j][0] {
			return false
		}
		//比他的高和等于的人数升序
		if people[i][1] > people[j][1] {
			return false
		}
		return true
	})

	ret := [][]int{people[0]}
	for i := 1; i < len(people); i++ {
		temp := make([][]int, len(ret)-people[i][1])
		copy(temp, ret[people[i][1]:])
		// 0~people[i][1]-1, people[i], people[i]~原来的内容
		ret = append(ret[:people[i][1]], people[i])
		ret= append(ret, temp...)
	}
	return ret
}

```

## 【推荐】方法二：参考其他大神的代码

```go
//其他大神的代码：参考LeetCode题解
func reconstructQueue(people [][]int) [][]int {
	sort.Slice(people, func(i, j int) bool {
		return (people[i][0] > people[j][0]) || (people[i][0] == people[j][0] && people[i][1] < people[j][1])
	})

	for from, p := range people {
		to := p[1]
		//将我们要放置的位置以及到最后一个元素这个区间整体后移一个单位
		copy(people[to+1:from+1], people[to:from])
		//然后放置该元素到我们要防止的位置
		people[to] = p
	}

	return people
}
```