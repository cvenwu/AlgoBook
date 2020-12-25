# [1118. 一月有多少天](https://leetcode-cn.com/problems/number-of-days-in-a-month/)

## 方法：


首先判断是否是闰年，
		使用一个数组存储非闰年的时候每月的天数

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.0 MB, 在所有 Go 提交中击败了12.00%的用户

```

func numberOfDays(Y int, M int) int {
   days := []int{31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31}
   if M != 2 {
      return days[M-1]
   } else if (Y%4 == 0 && Y%100 != 0) || Y%400 == 0 { //如果是闰年
      return 29
   } else {
      return 28
   }
}
```