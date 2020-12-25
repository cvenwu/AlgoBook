# 二进制中1的个数

## 方法一：

```go
func hammingWeight2(num uint32) int {
    var flag uint32 = 1
    count := 0
    for i := 0; i < 32; i++ {
        if num & flag != 0 {
            count += 1
        }
        flag <<= 1
    }
    return count
}
```

## 方法二：

```go
func hammingWeight(num uint32) int {
    count := 0
    for num != 0{
        count += 1
        num = num & (num - 1)
    }
    return count
}
```

**Tips：**一个数字左移1位之后右边补0，但是进行右移时，如果是无符号的则在左边补0，否则补上符号位(整数补0，负数补1)，算术右移补上符号位，逻辑右移不用补符号位



这两个解法都是通用的，不仅仅针对于无符号整数，也针对有符号整数，方法一我们不希望右移，因为右移对于负数会补1，后面会陷入无限循环。所以我们可以设置一个flag不断左移进行判断每一位是否为1





## 拓展题目：

### 一条语句判断1个数是不是2的整数次方

如果这个数是2的整数次方，一定这个数不为0并且这个数的二进制表示中只有1个1

`(num != 0) && (num & (num - 1) == 0)`



```go
func isPowerOfTwo(num int) bool {
    if num != 0 && (num & (num - 1)) == 0 {
        return true
    }
    return false
}
```



### 输入两个整数m和n，计算需要改变m的二进制表示中的多少位才能得到n？

思路：

1. 采用异或求两个数字二进制表示中的不同位  
2. 之后统计异或结果二进制表示中1的数目

```go
func numOfChangeBits(m int, n int) int {
    //求两个数字的异或
    ret := m ^ n
    //统计ret中位1的个数
    count := 0
    for ret != 0 {
        count += 1
        ret &= (ret - 1)
    }
    return count
}
```

