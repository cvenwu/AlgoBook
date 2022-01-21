# 左旋转字符串

## 方法一：剑指offer

!> 思路：剑指offer思路，先旋转前n个字符串，之后旋转剩下的，最后旋转整个字符串

```go
func reverseLeftWords(s string, n int) string {
	if len(s) <= n {
		return s
	}

	//首先反转左边
	Reverse(&s, 0, n-1)
	Reverse(&s, n, len(s)-1)
	Reverse(&s, 0, len(s)-1)

	return s
}

func Reverse(s *string, start, end int) {

	tempS := []rune(*s)
	for start < end {
		tempS[start], tempS[end] = tempS[end], tempS[start]
		start, end = start+1, end-1
	}
	*s = string(tempS)
}
```

## 方法二：利用go的切片语言特性


```go
func reverseLeftWords(s string, n int) string {
	return s[n:] + s[:n]
}
```


## 【推荐】方法三：
思路：
1. 首先判断n的合理性
2. 反转前n个字符，以及剩余的字符
3. 反转整个字符串
4. 返回结果


```go
func reverseLeftWords(s string, n int) string {
	if n < 0 || n >= len(s) {
		return s
	}
	content := []byte(s)
	//反转前n个字符
	reverseWords(content, 0, n-1)
	//反转后面的字符
	reverseWords(content, n, len(content)-1)
	//将全部字符进行反转
	reverseWords(content, 0, len(content)-1)

	return string(content)
}


func reverseWords(content []byte, start, end int) {
	for start < end {
		content[start], content[end] = content[end], content[start]
		start++
		end--
	}
}
```

```c++

class Solution {
public:
    //字符串长度为7
    string reverseLeftWords(string s, int n) {
        if (s.size() <= 1 || n <= 0) return s;
        //首先反转字符串的前n个字符
        for (int i = 0; i < n>>1; i++)
        {
            char temp = s[i];
            s[i] = s[n-1-i];
            s[n-1-i] = temp;
        }
        //反转字符串剩下的字符
        //反转多少次
        for (int i = 0; i < (s.size()-n)>>1; i++)
        {
            char temp = s[n+i];
            s[i+n] = s[s.size()-1-i];
            s[s.size()-1-i] = temp;
        }
        //最后将整个字符串进行反转  
        for (int i = 0; i < s.size()>>1; i++)
        {
            char temp = s[i];
            s[i] = s[s.size()-1-i];
            s[s.size()-1-i] = temp; 
        }
        //最后返回我们现在的结果
        return s;
    }
};
```
