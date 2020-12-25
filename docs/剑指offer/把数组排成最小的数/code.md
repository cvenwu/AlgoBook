# [剑指 Offer 45. 把数组排成最小的数](https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/)

## 方法：

注意：Golang中整数转换为字符串不可以使用string(整数)，必须使用strconv.Itoa(一个int的数)来将int转换为字符串
思路：将数组中的所有元素转换为字符串，按照我们自定义的排序规则进行排序，

步骤：
1. 对传入的整数数组中的所有元素转换为字符串
2. 对该字符串数组使用排序规则进行排序，这里我们实现排序接口中的三个方法
3. 对排序后的结果拼接得到我们最终的结果并返回

返回的结果：排序之后对所有字符串中的元素进行拼接便是我们想要的结果

具体接口的实现[参考](https://leetcode-cn.com/problems/ba-shu-zu-pai-cheng-zui-xiao-de-shu-lcof/solution/golangzi-ding-yi-pai-xu-gui-ze-by-bbfat/)


> 执行用时：4 ms, 在所有 Go 提交中击败了 83.83% 的用户
> 		内存消耗：2.5 MB, 在所有 Go 提交中击败了 68.10% 的用户


```go
type myData []string

func (d myData) Len() int {
	return len(d)
}

func (d myData) Swap(i, j int) {
	d[i], d[j] = d[j], d[i]
}

func (d myData) Less(i, j int) bool {
	num1, _ := strconv.Atoi(d[i]+d[j])
	num2, _ :=  strconv.Atoi(d[j]+d[i])
	return num1 < num2
}

func minNumber(nums []int) string {
	numsString := make(myData, len(nums))

	for i := 0; i < len(numsString); i++ {
		numsString[i] = strconv.Itoa(nums[i])
	}

	//排序
	sort.Sort(numsString)

	return strings.Join(numsString, "")

}

```

