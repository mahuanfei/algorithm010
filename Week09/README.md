[44. 通配符匹配](https://leetcode-cn.com/problems/wildcard-matching/)

> 给定一个字符串 (s) 和一个字符模式 (p) ，实现一个支持 '?' 和 '*' 的通配符匹配。
'?' 可以匹配任何单个字符。
'*' 可以匹配任意字符串（包括空字符串）。
两个字符串完全匹配才算匹配成功。
说明:
s 可能为空，且只包含从 a-z 的小写字母。
p 可能为空，且只包含从 a-z 的小写字母，以及字符 ? 和 *。
示例 1:
输入:
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。
示例 2:
输入:
s = "aa"
p = "*"
输出: true
解释: '*' 可以匹配任意字符串。
示例 3:
输入:
s = "cb"
p = "?a"
输出: false
解释: '?' 可以匹配 'c', 但第二个 'a' 无法匹配 'b'。
示例 4:
输入:
s = "adceb"
p = "*a*b"
输出: true
解释: 第一个 '*' 可以匹配空字符串, 第二个 '*' 可以匹配字符串 "dce".
示例 5:
输入:
s = "acdcb"
p = "a*c?b"
输出: false

动态规划思路

> 给定的模式p中,只会有三种类型的字符出现:
> (1)小写字母a-z,匹配对应的一个小写字母
> (2)问号?,可以匹配任意一个小写字母
> (3)星号*,可以匹配任意字符串,可以为空,也可以匹配0或任意多个小写字母
其中小写字母和问号的匹配是确定的,星号匹配不确定因此我们需要枚举所有的匹配情况。为了减少重复枚举，我们可以使用动态规划来解决本题。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200808103005452.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200808103746177.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)

```python
class Solution(object):
    def isMatch(self, s, p):
       m, n = len(s), len(p)
       dp = [[False] * (n + 1) for _ in range(m + 1)]
       dp[0][0] = True
       for i in range(1, n+1):
           if p[i-1] == '*':
               dp[0][i] = True
           else:
                break
        
       for i in range(1, m+1):
            for j in range(1, n+1):
                if p[j-1] == '*':
                    dp[i][j] = dp[i-1][j] | dp[i][j-1]
                elif p[j-1] == '?' or s[i-1] == p[j-1]:
                    dp[i][j] = dp[i-1][j-1]
       return dp[m][n]
```
[115. 不同的子序列](https://leetcode-cn.com/problems/distinct-subsequences/)

> 给定一个字符串 S 和一个字符串 T，计算在 S 的子序列中 T 出现的个数。
一个字符串的一个子序列是指，通过删除一些（也可以不删除）字符且不干扰剩余字符相对位置所组成的新字符串。（例如，"ACE" 是 "ABCDE" 的一个子序列，而 "AEC" 不是）
题目数据保证答案符合 32 位带符号整数范围。
示例 1：
输入：S = "rabbbit", T = "rabbit"
输出：3
解释：
如下图所示, 有 3 种可以从 S 中得到 "rabbit" 的方案。
(上箭头符号 ^ 表示选取的字母)
rabbbit
^^^^ ^^
rabbbit
^^  ^^^^
rabbbit
^^^  ^^^
示例 2：
输入：S = "babgbag", T = "bag"
输出：5
解释：
如下图所示, 有 5 种可以从 S 中得到 "bag" 的方案。 
(上箭头符号 ^ 表示选取的字母)
babgbag
^^  ^
babgbag
^^        ^
babgbag
^        ^^
babgbag
 	^     ^^
babgbag
   		 ^^^

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200808112127930.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200808114208275.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)

暴力法:
通过记录当前走到的下标 i 和 j 来判断是否相等，若 s[ j ] == t[ i ]， 则可以将 目标字符串的下标加一然后继续匹配（即选中当前字符）。其中，无论 s[ j ] 是否等于 t[ i ] 都需要不选择当前字符，然后继续进行递归。递归的终止条件为字符串  s 或  t  的下标走到最后的位置，当字符串 t 走到最终位置时，发生了一次匹配， res++ 


```cpp
class Solution {
public:
    int numDistinct(string s, string t) {
        int res = 0;
        dfs(res, s, t, 0, 0);
        return res;
    }
    void dfs(int& res, string s, string t, int i, int j){
        if(i==t.size()){
            res++;
            return ;
        }
        if(j==s.size()) return ;
        // 是否相等都要走这一步
        dfs(res, s, t, i, j+1);
        // 发生匹配时，字符串 t 的下标 +1 然后进行匹配
        if(s[j]==t[i]) dfs(res, s, t, i+1, j+1);
    }
};

```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200808115454806.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)
```python
class Solution:
    def numDistinct(self, s: str, t: str) -> int:
        n1 = len(s)
        n2 = len(t)
        dp = [[0] * (n1 + 1) for _ in range(n2 + 1)]
        for j in range(n1 + 1):
            dp[0][j] = 1
        for i in range(1, n2 + 1):
            for j in range(1, n1 + 1):
                if t[i - 1] == s[j - 1]:
                    dp[i][j] = dp[i - 1][j - 1]  + dp[i][j - 1]
                else:
                    dp[i][j] = dp[i][j - 1]
        #print(dp)
        return dp[-1][-1]


```

```java
   public int numDistinct(String s, String t) {
        int[][] dp = new int[t.length()+1][s.length()+1];
        for (int i = 0; i < s.length()+1; i++) {
            // 只要前者为空,后者随便s有多少个字符,那么出现的个数只有一个,只能全部删除变为空串
            dp[0][i] = 1;
        }
        for(int i = 1; i<t.length()+1;i++){
            for(int j =i; j<s.length()+1;j++){
                if(t.charAt(i-1) == s.charAt(j-1)) {
                    dp[i][j] = dp[i][j-1] + dp[i-1][j-1];
                }else{
                    dp[i][j] = dp[i][j-1];

                }
            }
        }
        return dp[t.length()][s.length()];
    }
```
