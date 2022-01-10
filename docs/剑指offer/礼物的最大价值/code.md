# 礼物的最大价值


## 方法一：初级DP

!> 思路：问题是求最大价值(最值)，同时是遍历一个二维数组，涉及到了大量的重复计算，因而我们可以考虑dp，


状态定义：`dp[i][j]`表示第i, j位置的礼物最大价值

**dp公式如下(状态转移方程)：**
1. `dp[i][j] = grid[i][j])` 位于第一行第一列，只能赋值
2. `dp[i][j] = dp[i][j-1] + grid[i][j]` 位于第一行非第一列，只能从左边过来
3. `dp[i][j] = dp[i-1][j] + grid[i][j]` 位于其他行的第一列，只能从上边下来
4. `dp[i][j] = max(dp[i-1][j] + grid[i][j], dp[i][j-1] + grid[i][j])`  当不在第一行第一列时

```go

func maxValue(grid [][]int) int {
	//如果行和列两个维度只要有一个为0就返回0
	if len(grid) == 0 || len(grid[0]) == 0 {
		return 0
	}

	valueArray := make([]int, len(grid[0]))

	//遍历多少次
	for i := 0; i < len(grid); i++ {
		//修改多少列
		for j := 0; j < len(grid[0]); j++ {
			//第1个值
			if i == 0 && j == 0 {
				valueArray[j] = grid[i][j]
			} else if i == 0 {	//第1行
				valueArray[j] = valueArray[j-1] + grid[i][j]
			} else if j == 0 {  //第一列
				valueArray[j] = valueArray[j] + grid[i][j]
			} else {
				valueArray[j] = max(valueArray[j] + grid[i][j], valueArray[j-1] + grid[i][j])
			}
		}
	}
    return valueArray[len(valueArray)-1]
}

func max(a int, b int) int{
	if a > b {
		return a
	} 
	return b
}
```

## 【推荐】方法二：状态压缩后的DP

!> 最入门的dp可以使用二维数组来存储结果，这里我们进行状态压缩，使用一维数组来存储结果

```go

func maxValue(grid [][]int) int {

	//如果没有行或者没有列直接返回0
	if len(grid) == 0 || len(grid[0]) == 0 {
		return 0
	}

	//定义一个状态压缩之后的dp数组
	dp := grid[0]

	//行和列的数量
	rows, cols := len(grid), len(grid[0])

	//初始化我们第一行遍历结束之后的dp数组
	for i := 1; i < cols; i++ {
		dp[i] += dp[i-1]
	}

	//开始不断更新我们的dp数组
	for i := 1; i < rows; i++ {
		for j := 0; j < cols; j++ {
			if j == 0 {
				dp[j] += grid[i][j]
			} else {
				dp[j] = max(dp[j-1], dp[j]) + grid[i][j]
			}

		}
	}

	//返回我们的结果
	return dp[len(dp)-1]
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```


```c++
class Solution {
public:
    int maxValue(vector<vector<int>>& grid) {
        //如果一维为0或者二维为0直接返回-1
        if (grid.size() == 0 || grid[0].size() == 0) return -1;
        
        //状态压缩
        vector<int> dp = grid[0];

        //初始化第一行的数据
        for (int i = 1; i < dp.size(); i++)
            dp[i] += dp[i-1];
        
        //然后开始移动到下面每一行
        for (int i = 1; i < grid.size(); i++)
        {
            for (int j = 0; j < grid[0].size(); j++)
            {

                if (j == 0)
                    dp[0] += grid[i][0];
                else
                    dp[j] = max(dp[j], dp[j-1]) + grid[i][j];
            }
        }

        //返回数组的最后一个值
        return dp[dp.size()-1];
    }
};
```