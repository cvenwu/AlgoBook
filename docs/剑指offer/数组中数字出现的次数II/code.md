# 数组中数字出现的次数II

>  思路：所有数字出现了3次，那么我们可以将二进制位每一位的结果进行无进位相加，如果哪一位可以被3整除，则说明只出现一次的数字在该位上为0，否则为1

## 方法一

```go
//自己的解法，也是和剑指offer一样的解法
func singleNumber(nums []int) int {
    if len(nums) <= 0 {
        return 0
    }

    //首先定义返回的结果
    ret, bitMask, temp := 0, 1, 0
    for i := 0; i < 31; i++ {
        temp = 0
        for j := 0; j < len(nums); j++ {
            //如果该位不为1，则加上
            if bitMask & nums[j] != 0 {
                temp += 1
            }
        }
        //判断最后的结果是否可以被3整除
        //能被3整除，所以我们可以说，那个出现一次的数字在该位置上为0
        if temp % 3 != 0 {
            //不能被3整除，所以我们可以说，那个出现一次的数字在该位置上为1
            ret += bitMask
        }
        bitMask <<= 1
    }
    return ret
}
```

**自己在2021-04-12改进的解法：代码便于理解**


```go
/*
步骤：
1. 建立一个长度为32的int类型的数组，数组中的每一位表示k进制的一位上的值
2. 我们遍历传入的数组：假设当前元素为cur
	2.1 将当前元素的k进制表示加入到我们创建的数组中
3. 遍历我们的数组，然后计算每一位的元素对k的余数，并乘以该位表示的进制的数然后加上我们之前的结果
4. 返回我们的结果
 */
func singleNumber(nums []int) int {
	//转换成k进制
	ret := make([]int, 32)
	for i := 0; i < len(nums); i++ {
		cur, index := nums[i], 0
		for cur/3 != 0 {
			ret[31-index], index, cur = ret[31-index]+cur%3, index+1, cur/3
		}
		ret[31-index] += cur % 3
	}

	//遍历ret
	temp := 1
	result := 0
	for i := 0; i < len(ret); i++ {
		result += (ret[31-i] % 3) * temp
		temp *= 3
	}
	return result
}
```

## 方法二：有限状态自动机+位运算

[参考](*https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/solution/mian-shi-ti-56-ii-shu-zu-zhong-shu-zi-chu-xian-d-4/)

```go
func singleNumber2(nums []int) int {
    one, two := 0, 0
    for _, e := range nums {
        one = ^two & (one^e)
        two = ^one & (two^e)
    }
    return one
}
```

总结：两个方法时间复杂度都为o(1)，**但整体上推荐方法二，详细解答点击上面的参考**

