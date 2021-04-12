# 和为s的连续正数序列


## 【推荐】方法一：


思路：采用两个指针l与r，表示区间[l,r]上这个范围的和sum

循环从1开始到target的一半+1,因为可能target为奇数比如9，最后拼凑的两个和为[4,5]
1. `sum > target`：`sum-=l`，然后`l++`
2. `sum < target`：`r++`，然后`sum+=r`
3. `sum == target`：将`[l,r]`加入到结果，然后`sum-=l`，`l++`

```go

func findContinuousSequence(target int) [][]int {
	l, r := 1, 1
	sum := 1 //sum表示[l,r]这个连续区间范围上的和
	var ret [][]int
	for r <= target>>1+1 && l <= r {
		if sum > target {
			sum -= l
			l++
		} else if sum < target {
			r++
			sum += r
		} else if sum == target {
			temp := make([]int, r-l+1)
			for i := l; i <= r; i++ {
				temp[i-l] = i
			}
			ret = append(ret, temp)
			sum -= l
			l++
		}
	}
	return ret
}

```