# 把字符串转换成整数


## 方法一：

题目中的文字**注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。**，也就是说我们在转换数字的时候一旦遇到一个不是数字字符的时候直接可以结束转换。

思路如下：
1. 去掉左边的空格，
2. 使用一个符号位变量默认为1，如果去掉空格第一个符号位是-或+分别设置对应的符号位的值，
3. 继续遍历后面的数字并且拼接我们的结果，一旦遇到非数字字符直接结束。（注意这个过程判断是否越界）
4. 返回我们的拼接的结果 乘以 符号位


```go
func strToInt(str string) int {
	var ret int
	sign := 1

	var i int
	for i = 0; i < len(str); i++ {
		if str[i] == '-' {
			sign, i = -1, i+1
			break
		} else if str[i] == '+' {
			i++
			break
		} else if str[i] >= '0' && str[i] <= '9' {
			break
		} else if str[i] == ' ' {
			continue
		} else { //如果第一个非空格字符既不是-又不是数字字符又不是+则直接返回默认结果
			return ret
		}
	}

	for ; i < len(str); i++ {

		//判断不是数字字符
		if str[i] < '0' || str[i] > '9' {
			break
		}

		//判断是否越界
		if ret > 214748364 || (sign == 1 && ret == 214748364 && str[i] >= '8') || (sign == -1 &&
			ret > 214748364 || ret == 214748364 && str[i] >= '9') {
			if sign == 1 {
				return math.MaxInt32
			} else {
				return math.MinInt32
			}
		}

		//如果遇到的不是数字字符，说明已经结束了
		ret = ret*10 + int(str[i]-'0')
	}

	return ret * sign
}

```