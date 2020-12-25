# [1154. 一年中的第几天](https://leetcode-cn.com/problems/day-of-the-year/)

## 方法：

步骤如下：

1. 分割字符串，截取到年份，月份，日期
2. 之后计算每个月的天数到当前月的上一个月，之后再计算当前月的天数
3. 最后进行判断，如果当前月大于2月并且是闰年，我们就要让最终结果+1
4. 返回最终结果

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.0 MB, 在所有 Go 提交中击败了18.75%的用户

```
func dayOfYear(date string) int {
   dateInfo := strings.Split(date, "-")
   year, _ := strconv.Atoi(string(dateInfo[0]))
   month, _ := strconv.Atoi(dateInfo[1])
   day, _ := strconv.Atoi(dateInfo[1])

   days := []int{31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31}

   ret := 0
   for i := 1; i < month; i++ {
      ret += days[i-1]
   }
   ret += day

   //如果月份大于3月并且是闰年就给加一天
   if month >= 3 && ((year%4 == 0 && year%100 != 0) || year%400 == 0){
      ret += 1
   }

   return ret
}
```