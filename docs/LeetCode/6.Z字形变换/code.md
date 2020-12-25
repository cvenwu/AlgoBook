# [6. Z 字形变换](https://leetcode-cn.com/problems/zigzag-conversion/)

## 方法一：

虽然看起来比较难，但是是**一道规律题**

1. 因为每一行都是有规律可循，所以最外层需要的循环次数就是我们的行数。

2. 对于每一行，除了第一行和最后一行只有一个间距，其他都有两个间距，其中第一个间距一定是`rows<<1 - (i+1)<<1`，第2个间距假设为j，那么一定有j + 第1个间距的和为`rows<<1 - 2 `，对于除了第一行和最后一行的任意一行都适用。

3. 所以使用一个变量不断切换两个间距即可。

```go
package main

/**
 * @Author: yirufeng
 * @Date: 2020/11/29 12:00 下午
 * @Desc: 6.Z字形变换
 **/

func convert(str string, rows int) string {
	if rows <= 1 {
		return str
	}

	index := 0
	ret := make([]byte, len(str))

	//加入第一行
	for j := 0; j < len(str); j += rows<<1 - 2 {
		ret[index] = str[j]
		index++
	}

	//加入中间几行
	for i := 1; i < rows-1; i++ {
		step := rows<<1 - (i+1)<<1
		for j := i; j < len(str); {
			ret[index] = str[j]
			j += step
			step = rows<<1 - 2 - step
			index++
		}
	}

	//加入最后一行
	for j := rows - 1; j < len(str); j += rows<<1 - 2 {
		ret[index] = str[j]
		index++
	}

	return string(ret)
}

```

