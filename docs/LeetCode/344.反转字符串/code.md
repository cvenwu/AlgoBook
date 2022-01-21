# [344. 反转字符串](https://leetcode-cn.com/problems/reverse-string/)

## 方法一：直接反转

```go
func reverseString(s []byte)  {
	for i := 0; i < (len(s) >> 1); i++ {
		s[i], s[len(s)-1-i] = s[len(s)-1-i], s[i]
	}
}
```


```cpp
class Solution {

public:
    void reverseString(vector<char>& s) {
        for ( int i = 0; i < s.size()>>1; i++ )
        {
            char temp = s[i];
            s[i] = s[s.size()-1-i];
            s[s.size()-1-i] = temp;
        }
        cout << "this is c++ code...." << endl;
    }
};
```

## 方法二：使用双指针进行反转


```go
func reverseString(s []byte)  {
	l, r := 0, len(s)-1
	for l < r {
		s[l], s[r] = s[r], s[l]
		l, r = l + 1, r - 1
	}
}
```

```c++
class Solution {
public:
    void reverseString(vector<char>& s) {
        int l = 0, r = s.size()-1;
        char temp;
        while(l < r)
        {
            temp = s[l];
            s[l] = s[r];
            s[r] = temp;

            l++;
            r--;
        }
    }
};
```