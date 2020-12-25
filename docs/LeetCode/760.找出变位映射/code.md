# [760. 找出变位映射](https://leetcode-cn.com/problems/find-anagram-mappings/)

## 方法：哈希表

思路：

1. 使用哈希表存储B的元素以及对应的下标

2. 之后遍历A，从A中的元素找到map中存储元素对应的下标


> 执行用时：0 ms, 在所有 Go 提交中击败了100.00%的用户
> 		内存消耗：2.2 ms, 在所有 Go 提交中击败了44.44%的用户


```go
func anagramMappings(A []int, B []int) []int {
    
    mymap := make(map[int]int)
    ret := make([]int, len(A))
    for i, v := range B {
        mymap[v] = i
    }
    
    for i, v := range A {
        ret[i] = mymap[v]
    }
    
    return ret
}
```

