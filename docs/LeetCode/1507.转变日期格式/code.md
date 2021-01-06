

# [1507.转变日期格式](https://leetcode-cn.com/problems/reformat-date/)


## 方法一:

!> 思路：字符串分割切分然后使用哈希表获得对应的月份之后拼接返回

```go
// 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
// 内存消耗：2 MB, 在所有 Go 提交中击败了37.88%的用户
func reformatDate(date string) string {

    monthDict := map[string]string{
        "Jan": "01", 
        "Feb": "02", 
        "Mar": "03", 
        "Apr": "04", 
        "May": "05", 
        "Jun": "06", 
        "Jul": "07", 
        "Aug": "08", 
        "Sep": "09", 
        "Oct": "10", 
        "Nov": "11",
        "Dec": "12",
    }

    dateStr := strings.Split(date, " ")
    day := dateStr[0][:len(dateStr[0])-2]
    if len(day) == 1 {
        day = "0" + day
    }
    //转换成结果并返回
    return dateStr[2] + "-" + monthDict[dateStr[1]] + "-" + day
}

```