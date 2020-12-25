# [771. 宝石与石头](https://leetcode-cn.com/problems/jewels-and-stones/)

## 方法一：暴力解法

这里不再赘述

时间复杂度：O(len(J) * len(S))

空间复杂度：O(1)

## 方法二：使用map加速

步骤：

1. 将J中的每个元素放入到map中
2. 遍历S中的每个元素，判断每个元素是否出现在map中，出现则次数++
3. 最后返回次数即可

1. 时间复杂度：O(len(J) + len(S))
2. 空间复杂度：O(50) 相当于O(1)

> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户

> 内存消耗：2.1 MB, 在所有 Go 提交中击败了14.55%的用户

```go
//时间复杂度：O(len(J) + len(S))
//空间复杂度：O(50) 相当于O(1)
//暴力解法：O(len(J) * len(S))
func numJewelsInStones(J string, S string) int {
    mymap := make(map[rune]bool)
    //初始化map
    for _, c := range J {
        mymap[c] = true
    }
    ret := 0
    //对于S中的每个字符，判断是否在map中
    for _, c := range S {
        if _, ok := mymap[c]; ok {
            ret++
        }
    }
    return ret
}
```

