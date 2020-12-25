# [205. 同构字符串](https://leetcode-cn.com/problems/isomorphic-strings/)

思路：题目中说明了：两个字符不能映射到同一个字符上，但字符可以映射自己本身。这里我们**采用两个map进行双向验证**，第一个字符串到第2个字符串使用mymap1进行映射，第2个字符串到第1个字符串使用mymap2进行映射


> 执行用时：4 ms, 在所有 Go 提交中击败了75.24%的用户
> 		内存消耗：2.6 MB, 在所有 Go 提交中击败了100.00%的用户

```go
func isIsomorphic(s string, t string) bool {

   //长度不等肯定替换不可以得到直接返回false
   if (s == "" && t == "") || len(s) != len(t) {
      return false
   }
   mymap := make(map[uint8]uint8)
   for i := 0; i < len(s); i++ {
      if val, ok := mymap[s[i]]; ok {
         if val != t[i] {
            return false
         }
      } else {
         mymap[s[i]] = t[i]
      }
   }
   mymap2 := make(map[uint8]uint8)
   for i := 0; i < len(t); i++ {
      if val, ok := mymap2[t[i]]; ok {
         if val != s[i] {
            return false
         }
      } else {
         mymap2[t[i]] = s[i]
      }
   }
   return true
}
```