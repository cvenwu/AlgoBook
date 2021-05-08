# 排序模板


## 冒泡排序

```go

func BubbleSort(nums []int) {
	bubbleSortV2(nums)
}

//最基本的
func bubbleSortV1(nums []int) {
	for i := 1; i < len(nums); i++ { //冒泡的次数
		for j := 0; j < len(nums)-i; j++ {
			//如果前面比后面大就交换
			if nums[j] > nums[j+1] {
				nums[j], nums[j+1] = nums[j+1], nums[j]
			}
		}
	}
}

//如果某次冒泡的时候没有交换元素，那么说明这一趟以及后面的元素都是有序的，之后后面我们就不再需要进行冒泡，可以直接退出
func bubbleSortV2(nums []int) {
	//表示某一趟是否有元素交换
	flag := true
	for i := 1; i < len(nums); i++ { //冒泡的次数
		flag = true
		for j := 0; j < len(nums)-i; j++ {

			//如果前面比后面大就交换
			if nums[j] > nums[j+1] {
				flag, nums[j], nums[j+1] = false, nums[j+1], nums[j]
			}
		}
		//如果没有元素交换，说明已经有序，直接退出即可
		if flag {
			break
		}
	}
}

//在第二种改进的基础上再使用一个变量，表示我们最后交换的位置，下次遍历到这个位置就可以了
//使用一个变量记录最后发生交换的位置，说明该位置之后都是有序的
func bubbleSortV3(nums []int) {
	//表示某一趟是否有元素交换
	flag := true
	//表示最后发生交换的位置
	//注意点1:这里必须再额外使用临时变量，因为如果只是用一个变量lastSwapPos的话，
	//lastSwapPos的值会一直改变，因此如果一旦变小，我们的for循环就会推出
	lastSwapPos := len(nums) - 1     //注意点2：初始值赋值是len(nums) - 1
	for i := 1; i < len(nums); i++ { //冒泡的次数
		lastSwapPosTemp := len(nums) - i //注意点3：初始值赋值是len(nums) - i
		flag = true
		//下次遍历的时候遍历到我们上次最后发生交换的位置就可以了
		for j := 0; j < lastSwapPos; j++ {
			//如果前面比后面大就交换
			if nums[j] > nums[j+1] {
				flag, nums[j], nums[j+1], lastSwapPosTemp = false, nums[j+1], nums[j], j //注意点4：这里lastSwapPosTemp应该赋值j，因为j与j+1比较过了，所以就认为j之后是已经有序的了
			}
		}
		//如果没有元素交换，说明已经有序，直接退出即可
		if flag {
			break
		}

		lastSwapPos = lastSwapPosTemp
	}
}
```

## 直接插入排序


```go
/**
 * @Author: yirufeng
 * @Date: 2021/4/13 8:28 下午
 * @Desc:

时间复杂度：O(n^2)
空间复杂度：O(1)
稳定的：因为是到前面找到一个小于等于自己的后面那个位置
 **/

func InsertSort(nums []int) {
	insertSort(nums)
}

func insertSort(nums []int) {
	var j int
	//第一个元素已经有序，只需要插入后面的元素即可
	for i := 1; i < len(nums); i++ {
		//当前要插入的元素目前位于i位置
		cur := nums[i]
		//从后往前找，直到前面元素小于当前元素就停，并且遍历的过程中不断将前面的元素移动到后面
		for j = i - 1; j >= 0 && nums[j] > cur; j-- {
			nums[j+1] = nums[j]
		}
		//此时j指向的元素就是前面那个小于或者等于当前元素的位置
		//但是我们应该在j+1位置插入
		nums[j+1] = cur
	}
}
```


## 选择排序

```go
/**
 * @Author: yirufeng
 * @Date: 2021/4/13 8:28 下午
 * @Desc: 选择排序

时间复杂度：假设数组中有n个数字
	大循环需要遍历n-2趟，然后每一个大循环中，
							我们都需要从开始的数字一直到最后选出最小的数字(时间复杂度：O(n))，然后填充到当期那还没有排好序的起始位置

最后总的时间复杂度：O(n^2)，
空间复杂度：O(1)。
稳定的排序算法（因为两个相等的数字前面那个一定是排序的时候第一个会被放置到前面的）
 **/

func SelectedSort(nums []int) {
	selectedSort(nums)
}

func selectedSort(nums []int) {
	var min int
	for i := 0; i < len(nums)-1; i++ {
		//每次从nums[i]开始起
		min = i
		//依次与后面比对来选择最小的
		for j := i + 1; j < len(nums); j++ {
			if nums[min] > nums[j] {
				min = j
			}
		}
		//交换nums[min]与nums[i]
		nums[i], nums[min] = nums[min], nums[i]
	}
}
```


## 快速排序


```go
func QuickSort(nums []int) {
	quickSort(nums, 0, len(nums)-1)
}

//表示对[l,r]的元素进行快排
func quickSort(nums []int, l, r int) {
	if l < r {
		//在[l,r]区间内生成一个随机数作为划分
		index := rand.Intn(r-l+1) + l
		//每次都把我们随机选择的元素交换到最后
		nums[index], nums[r] = nums[r], nums[index]
		//进行划分，得到两个相等的区间
		less, more := partition(nums, l, r)
		//继续对小于和大于的两个区间进行快速排序
		quickSort(nums, l, less-1)
		quickSort(nums, more+1, r)
	}
}

//返回两个结果是等于我们随机数的区间的左端点和右端点
func partition(nums []int, l, r int) (int, int) { //使用nums[r]作为一个划分
	less, more := l-1, r
	num := nums[r]
	cur := l
	for cur < more {
		if nums[cur] < num {
			nums[less+1], nums[cur], less, cur = nums[cur], nums[less+1], less+1, cur+1
		} else if nums[cur] > num {
			nums[more-1], nums[cur], more = nums[less+1], nums[more-1], more-1
		} else if nums[cur] == num { //注意点：不交换，直接当前元素到下一个
			cur++
		}
	}
	//注意点：
	nums[more], nums[r] = nums[r], nums[more]
	//注意点：
	return less + 1, more - 1
}

func main() {
	nums := util.GenerateRandomNums(0, 100, 10)
	fmt.Println(nums)
	temp := make([]int, 10)
	copy(temp, nums)
	sort.Ints(temp)
	fmt.Println(temp)
	QuickSort(nums)
	fmt.Println(nums)

	nums2 := []int{884688278, 387052570, 77481420, 537616843, 659978110, 215386675, 604354651, 64838842, 830623614, 544526209, 292446044, 570872277, 946770900, 411203381, 445747969, 480363996, 31693639, 303753633, 261627544, 884642435, 978672815, 427529125, 111935818, 109686701, 714012242, 691252458, 230964510, 340316511, 917959651, 544069623, 193715454, 631219735, 219297819, 151485185, 986263711, 805374069, 915272981, 493886311, 970466103, 819959858, 733048764, 393354006, 631784130, 70309112, 513023688, 17092337, 698504118, 937296273, 54807658, 353487181, 82447697, 177571868, 830140516, 536343860, 453463919, 998857732, 280992325, 13701823, 728999048, 764532283, 693597252, 433183457, 157540946, 427514727, 768122842, 782703840, 965184299, 586696306, 256184773, 984427390, 695760794, 738644784, 784607555, 433518449, 440403918, 281397572, 546931356, 995773975, 738026287, 861262547, 119093579, 521612397, 306242389, 84356804, 42607214, 462370265, 294497342, 241316335, 158982405, 970050582, 740856884, 784337461, 885254231, 633020080, 641532230, 421701576, 298738196, 918973856, 472147510, 169670404}
	QuickSort(nums2)
	fmt.Println(nums2)
}
```

## 归并排序

```go

/**
 * @Author: yirufeng
 * @Date: 2021/4/14 3:31 下午
 * @Desc: 归并排序
时间复杂度：O(nlogN) 最好最坏都是O(NlogN)
空间复杂度：O(n)。需要一个辅助空间
是否稳定：稳定的（因为一旦两个数相等我们就不比较，并且每次都是前半部分与后半部分进行比较，所以前面同样的内容与后面同样的内容的相对位置不会改变）
 **/

func MergeSort(nums []int) {
	mergeSort(nums, 0, len(nums)-1)
}

func mergeSort(nums []int, l, r int) {
	if l == r {
		return
	}

	//找到中间点，一分为二
	mid := l + (r-l)>>1
	//划分成左右两个有序的空间
	mergeSort(nums, l, mid)
	mergeSort(nums, mid+1, r)
	//将左右两部分以mid为划分轴进行合并
	merge(nums, l, mid, r)

}

//合并nums[l:mid+1] 与 nums[mid+1:r+1]两部分内容
func merge(nums []int, l, mid, r int) {
	i, j := l, mid+1
	cur := 0
	//注意点1：这里必须借助一个数组，否则我们前面可能还没有被排序的结果会被我们后半部分的内容覆盖掉，导致前面就没有内容
	help := make([]int, r-l+1)
	for i <= mid && j <= r {
		if nums[i] < nums[j] {
			help[cur], cur, i = nums[i], cur+1, i+1
		} else {
			help[cur], cur, j = nums[j], cur+1, j+1
		}
	}

	for i <= mid {
		help[cur], cur, i = nums[i], cur+1, i+1
	}
	for j <= r {
		help[cur], cur, j = nums[j], cur+1, j+1
	}

	//最后记得我们的help数组中的内容要放回到我们的原来的数组中
	for i := 0; i < len(help); i++ {
		nums[l+i] = help[i]
	}
}
```
## 堆排序

### 从小到大的堆排序
```go

func HeapSort(nums []int) {
	heapSort(nums)
}

func heapSort(nums []int) {
	if nums == nil || len(nums) < 2 {
		return
	}

	for i := 0; i < len(nums); i++ {
		heapPush(nums, i)
	}

	log.Println("建堆完成--------------->", nums)
	heapSize := len(nums)

	//之后不断吐出元素
	for heapSize > 0 {
		nums[0], nums[heapSize-1] = nums[heapSize-1], nums[0]
		heapSize--
		heapify(nums, heapSize)
	}
}

//表示当前要加入的是nums中的索引为index的元素
func heapPush(nums []int, index int) {
	for (index-1)>>1 >= 0 && nums[index] > nums[(index-1)>>1] {
		nums[index], nums[(index-1)>>1], index = nums[(index-1)>>1], nums[index], (index-1)>>1
	}
}

//length 表示堆的元素个数
func heapify(nums []int, length int) {
	index := 0
	// (index << 1 + 1) < length 说明是有左子树的
	for (index<<1 + 1) < length { //思路：每次找到左右孩子中更大的与父进行交换
		left := index<<1 + 1
		//如果有右子树并且右子树比左子树大
		if left+1 < length && nums[left+1] > nums[left] {
			left++
		}
		//如果当前节点比左右孩子中大的大，就不交换
		if nums[left] <= nums[index] {
			break
		}
		//如果当前节点比左右孩子中大的小，交换当前节点与左右孩子中较大的
		//同时让index变为left
		nums[left], nums[index], index = nums[index], nums[left], left
	}
}
```

### 从大到小的堆排序

```go
func HeapSort(nums []int) {
	heapSort(nums)
}

func heapSort(nums []int) {

	//如果nums为空或者元素个数小于两个，可以直接退出
	if nums == nil || len(nums) < 2 {
		return
	}

	//此时我们需要不断将元素加入我们的堆中
	for i := 1; i < len(nums); i++ {
		heapInsert(nums, i)
	}

	//获取堆的大小
	heapSize := len(nums)

	//此时我们不断将堆里面的元素弹出放到应该放置的位置
	for heapSize > 0 {
		//将堆的最后一个元素与堆顶交换
		nums[0], nums[heapSize-1] = nums[heapSize-1], nums[0]
		//堆的元素个数减去1
		heapSize--
		//开始从堆顶从上往下开始调整堆
		heapAdjustFromTopToBottom(nums, heapSize-1) // 从下到上的堆调整
	}
}

//第二个参数表示插入的元素位于nums的位置
func heapInsert(nums []int, index int) {
	//不断比较当前插入的位置的元素与父节点的大小，如果比父节点小就交换
	for (index-1)>>1 >= 0 && nums[(index-1)>>1] > nums[index] {
		nums[index], nums[(index-1)>>1], index = nums[(index-1)>>1], nums[index], (index-1)>>1
	}
}

//第二个参数表示当前堆的元素个数
func heapAdjustFromTopToBottom(nums []int, length int) {
	index := 0
	for index<<1+1 < length {
		left := index<<1 + 1

		//如果有右孩子并且右比左小
		if left+1 < length && nums[left+1] < nums[left] {
			left++
		}

		//当前节点小于等于两个孩子较小的
		if nums[index] <= nums[left] {
			break
		}
		nums[index], nums[left], index = nums[left], nums[index], left
	}
}
```