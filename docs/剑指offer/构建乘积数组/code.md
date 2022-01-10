# 构建乘积数组


## 方法一：
!> 思路：首先正向乘以，然后反向乘以(为了避免影响结果，反向的时候我们使用一个变量保存反向乘以的结果)
```go
func constructArr(a []int) []int {
	if len(a) <= 0 {
		return nil
	}

	ret := make([]int, len(a))
	ret[0] = 1
	//首先正着乘以
	for i := 1; i < len(a); i++ {
		ret[i] = ret[i-1] * a[i-1]
	}

	temp := 1
	//然后倒着乘以
	for i := len(a)-1; i >= 0; i-- {
		ret[i], temp = ret[i] * temp, temp * a[i]
	}

	return ret
}

```

```c++
class Solution {
public:
    vector<int> constructArr(vector<int>& a) {

        if (a.empty()) return {};
        
        vector<int> ret = {1};
        //正向遍历
        int tempRet = 1;
        for (int i = 1; i < a.size(); i++)
        {
            tempRet *= a[i-1];
            ret.push_back(tempRet);
        }

        //反向遍历
        tempRet = 1;
        for (int i = a.size()-2; i >= 0; i--)
        {
            tempRet *= a[i+1];
            ret[i] *= tempRet;
        }

        return ret;
    }
};
```