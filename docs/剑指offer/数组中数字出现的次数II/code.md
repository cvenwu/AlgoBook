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

