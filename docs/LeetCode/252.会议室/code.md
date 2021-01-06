# [252.会议室](https://leetcode-cn.com/problems/meeting-rooms/)

## 方法一：

!> 本质：因为一个人在同一时刻只能参加一个会议，因此题目实质是判断是否存在重叠区间，这个简单，将区间按照会议开始时间进行排序，然后遍历一遍判断即可。

```go

//本质：因为一个人在同一时刻只能参加一个会议，因此题目实质是判断是否存在重叠区间，这个简单，将区间按照会议开始时间进行排序，然后遍历一遍判断即可。
//其实就是判断两个区间是否相交，如果相交直接返回true
//执行用时：12 ms, 在所有 Go 提交中击败了51.11%的用户
//内存消耗：4.3 MB, 在所有 Go 提交中击败了36.36%的用户
func canAttendMeetings(intervals [][]int) bool {
	//对intervals进行排序:按照起点进行升序排序
	sort.Slice(intervals, func(i, j int) bool {
		return intervals[i][0] < intervals[j][0]
	})

	//之后遍历我们的intervals
	for i := 1; i < len(intervals); i++ {
		//如果当前区间与上一区间起点相同，或者当前区间的起点小于上一区间的末尾，直接返回false
		if intervals[i][0] == intervals[i-1][0] || intervals[i][0] < intervals[i-1][1]{
			return false
		}
	}

	return true
}
```