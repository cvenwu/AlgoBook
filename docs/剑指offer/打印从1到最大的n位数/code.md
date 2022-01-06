# 打印从1到最大的n位数


## 方法1
!> 构造一个最大的n位十进制数，然后循环添加到结果中

```go
func printNumbers(n int) []int {

    end := 9
    for i := 0; i < n-1; i++ {
        end = end * 10 + 9
    }

    ret := make([]int, end)
    for i := 1; i <= end; i++ {
        ret[i-1] = i
    }
    
    return ret;
}
```

```c++
class Solution {
public:
    vector<int> printNumbers(int n) {
        //获取n位最大的十进制数
        int end = 9;
        for (int i = 1; i <= n-1; i++)
            end = end * 10 + 9;
        //构造一个结果的动态数组
        vector<int> ret;
        //每次将当前的结果加入到动态数组里面
        for (int i = 1; i <= end; i++)
            ret.push_back(i);
        //直接返回我们的结果
        return ret;
    }
};
```