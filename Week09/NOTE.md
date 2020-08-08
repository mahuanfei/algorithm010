
/****************字符串基础知识和引申题目****************/
> Python 中字符串 x = 'abbc'和Java中的String x = "abbc"  是imutable的,好处是线程安全的也就是说当将其加一个字符或者减一个字符其实都是新生成了一个String,原来的String还是原来的内容
> C++的话,字符串它是可变的在多线程环境
> 
[string immutable](https://lemire.me/blog/2017/07/07/are-your-strings-immutable/)

遍历字符串

```python
for ch in "abbc":
	print(ch)
```

```java
String x = "abbc";
for (int i = 0; i < x.size(); ++i) {
	char ch = x.charAt(i);
}
for ch in x.toCharArray() {
	System.out.println(ch);
}
```

```cpp
string s1 = "abbc";
for (int i = 0; i < s1.length(); i++) {
	count << x[i];
}
```
##### 字符串比较
Java: 
String x = new String("abb");
String y = new String("abb");
在Java中比较的是指针(地址),而不是比较字符串里的内容

x == y --> false  // x和y是不同的变量,即不同内存上的地址,所以它指向内存中的不同地址
x.equals(y) --> true  // equals判断里面的内容相同
x.equalsIgnoreCase(y) --> true // 同时忽略它的大小写


[387. 字符串中的第一个唯一字符](https://leetcode-cn.com/problems/first-unique-character-in-a-string/)

> 给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回 -1。
示例：
s = "leetcode"
返回 0
s = "loveleetcode"
返回 2
该题目印象非常深刻,大二参考复旦大学插班生入学考试类似于转校考试,复试时面试官出这个题目,很遗憾被刷

1.暴力法brute-force: O(n^2)
 i 枚举所有字符,
 	j 枚举i后面的所有字符: 笔记j里面有没有和i相同的,如果有相同的说明没有重复,如果没有相同的i的话,第一个找到了

2 map (hashmap哈希表实现O(1)查重复, treemap O(logN)查重复二叉树实现)

3 hash table

```java
public int firstUniqChar(String s) {
	HashMap<Character, Integer> hm = new HashMap();
	for (int i = 0; i < s.length(); i++) {
		ha.put(s.charAt(i), hm.getOrDefault(s.charAt(i), 0) + 1);
	}
	for (int i = 0; i < s.length(); i++) {
		if (hm.get(s.charAt(i)) == 1) {
			return i;
		}
	}
	return -1;
}
```
[8. 字符串转换整数 (atoi)](https://leetcode-cn.com/problems/string-to-integer-atoi/)

> 请你来实现一个 atoi 函数，使其能将字符串转换成整数。
首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。接下来的转化规则如下：
如果第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字字符组合起来，形成一个有符号整数。
假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成一个整数。
该字符串在有效的整数部分之后也可能会存在多余的字符，那么这些字符可以被忽略，它们对函数不应该造成影响。
注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换，即无法进行有效转换。
在任何情况下，若函数不能进行有效的转换时，请返回 0 。
提示：
本题中的空白字符只包括空格字符 ' ' 。
假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−231,  231 − 1]。如果数值超过这个范围，请返回  INT_MAX (231 − 1) 或 INT_MIN (−231) 

```java
public int myAtoi(String str) {
	int index = 0, sign = 1, total = 0;
	
	//1. Empty string
	if(str.length() == 0) return 0;
	
	//2. Remove Spaces
	while(str.charAt(index) == ' ' && index < str.length())
		 index ++;
		 
	//3. Handle signs
	if(str.charAt(index) == '+' || str.charAt(index) == '-'){
 		sign = str.charAt(index) == '+' ? 1 : -1;
 		index ++;
	}
	
	//4. Convert number and avoid overflow
	while(index < str.length()){
		int digit = str.charAt(index) - '0';
		if(digit < 0 || digit > 9) break;
		
		//check if total will be overflow after 10 times and add digit
		if(Integer.MAX_VALUE/10 < total ||
 		 Integer.MAX_VALUE/10 == total && Integer.MAX_VALUE %10 < digit)
			return sign == 1 ? Integer.MAX_VALUE : Integer.MIN_VALUE;
	     total = 10 * total + digit;
 		index ++;
	}
	return total * sign;
 }
```

> strip函数原型
声明：s为字符串，rm为要删除的字符序列. 只能删除开头或是结尾的字符或是字符串。不能删除中间的字符或是字符串。
s.strip(rm)        删除s字符串中开头、结尾处，位于 rm删除序列的字符
s.lstrip(rm)       删除s字符串中开头处，位于 rm删除序列的字符
s.rstrip(rm)      删除s字符串中结尾处，位于 rm删除序列的字符
注意：1. 当rm为空时，默认删除空白符（包括'\n', '\r',  '\t',  ' ')
2.这里的rm删除序列是只要边（开头或结尾）上的字符在删除序列内，就删除掉。
```python
class Solution(object):

	def myAtoi(self, s):

		 if len(s) == 0 : return 0
		 ls = list(s.strip())

		 sign = -1 if ls[0] == '-' else 1
		 if ls[0] in ['-','+'] : del ls[0]

 		ret, i = 0, 0
		while i < len(ls) and ls[i].isdigit() :
			 ret = ret*10 + ord(ls[i]) - ord('0')
 			 i += 1
		return max(-2**31, min(sign * ret,2**31-1))
```
[14. 最长公共前缀](https://leetcode-cn.com/problems/longest-common-prefix/description/)

> 编写一个函数来查找字符串数组中的最长公共前缀。
如果不存在公共前缀，返回空字符串 ""。
示例 1:
输入: ["flower","flow","flight"]
输出: "fl"
示例 2:
输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
说明:
所有输入只包含小写字母 a-z 。

1 暴力法:从长度最小的单词开始枚举,看这个单词是不是其他里面的前缀,然后长度减1
2 写两层嵌套循环, 将字符串对齐排在一起
"flower"
"flow"
"flight"
检查第1列是否相同,相同的话就看第2列 ...,

```java
    public static String longestCommonPrefix(String[] strs) {
        if (strs == null || strs.length == 0) return "";
        for (int i = 0; i < strs[0].length(); i++) {
            char c = strs[0].charAt(i);
            for (int j = 1; j < strs.length; j++) {
                if (i == strs[j].length() || strs[j].charAt(i) != c)
                    return strs[0].substring(0, i);
            }
        }
        return strs[0];
    }
```




[151. 翻转字符串里的单词](https://leetcode-cn.com/problems/reverse-words-in-a-string/)

> 给定一个字符串，逐个翻转字符串中的每个单词。
示例 1：
输入: "the sky is blue"
输出: "blue is sky the"
示例 2：
输入: "  hello world!  "
输出: "world! hello"
解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
示例 3：
输入: "a good   example"
输出: "example good a"
解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
 说明：
无空格字符构成一个单词。
输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。


必备知识点:

> java - 在Java中将字符串拆分为整数列表
我想拆分一个字符串形式：
" 42 2152 12 3095 2" 
到整数列表中，但是当我使用.split(" ")函数时，由于开头是空白，所以开头却是一个空的""元素。
有没有办法在没有空元素的情况下拆分它？
在对数组调用split之前，请使用String.trim（）函数。这将删除原始字符串之前和之后的所有空格
例如：
    String original = " 42 2152 12 3095 2"; 
    original = original.trim();
    String[] array = original.split(" ");
为了使代码更整洁，您还可以将其编写为：
  String original = " 42 2152 12 3095 2"; 
    String[] array = original.trim().split(" ");
如果我们打印出数组：
  for (String s : array) {
        System.out.println(s);
   }
输出为：
42
2152
12
3095
2
 public static void main(String[] args) {
      String[] myArray = { "Apple", "Banana", "Orange" };
      List<String> myList = new ArrayList<String>(Arrays.asList(myArray));
      myList.add("Guava");
   }

```java
// split, reverse, join
   public String reverseWords(String s) {
        // trim() 方法用于删除字符串的头尾空白符  
        // " +"指多个空格
        String[] words = s.trim().split(" +");
        // 将数组反序
        Collections.reverse(Arrays.asList(words));
        return String.join(" ", words);
    }
```

```java

/*
reverse 整个string ,然后在单独reverse每个单词
the sky is blue
eulb si yks eht
blue is sky the
*/
```
[242. 有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)

> 给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。
示例 1:
输入: s = "anagram", t = "nagaram"
输出: true
示例 2:
输入: s = "rat", t = "car"
输出: false
说明:
你可以假设字符串只包含小写字母。
进阶:
如果输入字符串包含 unicode 字符怎么办？你能否调整你的解法来应对这种情况？

要判断

 
[438. 找到字符串中所有字母异位词](https://leetcode-cn.com/problems/find-all-anagrams-in-a-string/)

> 给定一个字符串 s 和一个非空字符串 p，找到 s 中所有是 p 的字母异位词的子串，返回这些子串的起始索引。
字符串只包含小写英文字母，并且字符串 s 和 p 的长度都不超过 20100。
说明：
字母异位词指字母相同，但排列不同的字符串。
不考虑答案输出的顺序。
示例 1:
输入:
s: "cbaebabacd" p: "abc"
输出:
[0, 6]
解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的字母异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的字母异位词。
 示例 2:
输入:
s: "abab" p: "ab"
输出:
[0, 1, 2]
解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的字母异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的字母异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的字母异位词。

判断abc,在前一个字符串中出现过几次,因为abc本身长度是3,那么前面检测的长度
为3的就行,也就是说拥有一个长度为3的窗口,这个窗口从左边慢慢向右边滑
每次滑一步,就看窗口中的单词和abc是不是异位词,每次都是增加一个字符和减少一个字符
所以如果用map存,就可以方便的把出窗口字符减掉,进窗口字符加进来


/****************高级动态规划****************/
高级DP为什么复杂,
第一:状态定义
第二:状态转移方程
它的复杂度来源于什么地方
1 状态拥有更多维度(二维,三维,或者更多,甚至需要压缩(比如斐波那契只需要两个变量))每个维度是什么,逻辑清晰
2 状态转移方程


[编辑距离](https://leetcode-cn.com/problems/edit-distance/)

> 给你两个单词 word1 和 word2，请你计算出将 word1 转换成 word2 所使用的最少操作数 。
你可以对一个单词进行如下三种操作：
插入一个字符
删除一个字符
替换一个字符
 示例 1：
输入：word1 = "horse", word2 = "ros"
输出：3
解释：
horse -> rorse (将 'h' 替换为 'r')
rorse -> rose (删除 'r')
rose -> ros (删除 'e')
示例 2：
输入：word1 = "intention", word2 = "execution"
输出：5
解释：
intention -> inention (删除 't')
inention -> enention (将 'i' 替换为 'e')
enention -> exention (将 'n' 替换为 'x')
exention -> exection (将 'n' 替换为 'c')
exection -> execution (插入 'u')

优化点:
首先这两个单词它的长度假设为m和n的话,长度更长的单词只可能长度减少
长度少的单词只可能长度增加,也就是最后词的长度变化范围在m和n直接,长度向中间逼近
DP方程,
定义dp[i][j] 第一维i表示第一个字符串匹配的长度,第二维j表示第二个字符串匹配的长度即word1.substr(0, i), word2.substr(0, j)之间的编辑距离

> 如果 word1[i] 与 word2[j] 相同，显然 dp[i][j]=dp[i-1][j-1]
• 如果 word1[i] 与 word2[j] 不同，那么 dp[i][j] 可以通过
 1. 在 dp[i-1][j-1] 的基础上做 replace 操作达到目的
 2. 在 dp[i-1][j] 的基础上做 insert 操作达到目的
 3. 在 dp[i][j-1] 的基础上做 delete 操作达到目的
 4. 取三者最小情况即可


爬楼梯
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200804205510215.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200804205528112.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200804205545460.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200804205556909.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200804205600734.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200804205612206.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200804205618128.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)


/****************高级字符串算法笔记****************/
[72. 编辑距离](https://leetcode-cn.com/problems/edit-distance/)

> 给你两个单词 word1 和 word2，请你计算出将 word1 转换成 word2 所使用的最少操作数 。
你可以对一个单词进行如下三种操作：
插入一个字符
删除一个字符
替换一个字符
 示例 1：
输入：word1 = "horse", word2 = "ros"
输出：3
解释：
horse -> rorse (将 'h' 替换为 'r')
rorse -> rose (删除 'r')
rose -> ros (删除 'e')
示例 2：
输入：word1 = "intention", word2 = "execution"
输出：5
解释：
intention -> inention (删除 't')
inention -> enention (将 'i' 替换为 'e')
enention -> exention (将 'n' 替换为 'x')
exention -> exection (将 'n' 替换为 'c')
exection -> execution (插入 'u')

dp[i][j] // word1.substr(0, i)与word2.substr(0, j)之间的编辑距离
word1:""
word2:""
dp[i][j] 代表 word1 到 i 位置转换成 word2 到 j 位置需要最少步数

所以，
当 word1[i] == word2[j]，dp[i][j] = dp[i-1][j-1]；
当 word1[i] != word2[j]，dp[i][j] = min(dp[i-1][j-1], dp[i-1][j], dp[i][j-1]) + 1
其中，dp[i-1][j-1] 表示替换操作，dp[i-1][j] 表示删除操作，dp[i][j-1] 表示插入操作。
注意，针对第一行，第一列要单独考虑，我们引入 '' 下图所示：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200806060806624.png)
第一行，是 word1 为空变成 word2 最少步数，就是插入操作
第一列，是 word2 为空，需要的最少步数，就是删除操作

```python
class Solution(object):
    def minDistance(self, word1, word2):
        n1 = len(word1)
        n2 = len(word2)
        dp = [[0] * (n2 + 1) for _ in range(n1 + 1)]
        # 第一行
        for j in range(1, n2 + 1):
            dp[0][j] = dp[0][j-1] + 1
        # 第一列
        for i in range(1, n1 + 1):
            dp[i][0] = dp[i-1][0] + 1
        for i in range(1, n1 + 1):
            for j in range(1, n2 + 1):
                if word1[i-1] == word2[j-1]:
                    dp[i][j] = dp[i-1][j-1]
                else:
                    dp[i][j] = min(dp[i][j-1], dp[i-1][j], dp[i-1][j-1] ) + 1
        #print(dp)      
        return dp[-1][-1]
```
[1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)

> 给定两个字符串 text1 和 text2，返回这两个字符串的最长公共子序列的长度。
一个字符串的 子序列 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。
例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。两个字符串的「公共子序列」是这两个字符串所共同拥有的子序列。
若这两个字符串没有公共子序列，则返回 0。
示例 1:
输入：text1 = "abcde", text2 = "ace" 
输出：3  
解释：最长公共子序列是 "ace"，它的长度为 3。
示例 2:
输入：text1 = "abc", text2 = "abc"
输出：3
解释：最长公共子序列是 "abc"，它的长度为 3。
示例 3:
输入：text1 = "abc", text2 = "def"
输出：0
解释：两个字符串没有公共子序列，返回 0。
提示:
1 <= text1.length <= 1000
1 <= text2.length <= 1000

子序列和子串的区别:子序列之间可以有间隔,而子串是连续的
动态转移方程:如果s1和s2某个字符相同,则说明已经找到了一个共同的字符,那么test1和test2的下标都要减少一位,因为第i个字符和第j个字符已经被用,否则不同的话,要不就是i删掉一个字符,或者j删掉一个字符,注意这里不能同时删掉,因为两者是不相同的
dp[i][j] = dp[i-1][j-1] + 1 (if s1[i-1] == s2[j-1]) 
else dp[i][j] = max(dp[i-1][j], dp[i][j-1])


> substring
public String substring(int beginIndex,int endIndex)
返回一个新字符串，它是此字符串的一个子字符串。
该子字符串从指定的 beginIndex 处开始，一直到索引 endIndex - 1 处的字符。
因此，该子字符串的长度为 endIndex-beginIndex。
示例：
"hamburger".substring(4, 8) returns "urge"
"smiles".substring(1, 5) returns "mile"
参数：
beginIndex - 开始处的索引（包括）。
endIndex - 结束处的索引（不包括）。
返回：
指定的子字符串。

```python
 def longestCommonSubsequence(self, text1, text2):
        if not text1 or not text2:
            return 0
        m = len(text1)
        n = len(text2)
        dp = [[0]*(n+1) for _ in range(m+1)]
        for i in range(1, m+1):
            for j in range(1, n+1):
                if text1[i-1] == text2[j-1]:
                    dp[i][j] = 1 + dp[i-1][j-1]
                else:
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1])
        return dp[m][n]
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200806063511169.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)

2. Longest common substring (最长子串） 
dp[i][j] = dp[i-1][j-1] + 1 (if s1[i-1] == s2[j-1]) 
else dp[i][j] = 0 // 此位置没有公共子串,最后的值是在所有位置中找到最大值即可

[5. 最长回文子串](https://leetcode-cn.com/problems/longest-palindromic-substring/)

> 给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。
示例 1：
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
示例 2：
输入: "cbbd"
输出: "bb"

1 嵌套循环,枚举i(起点), j(终点),判断该子串是否回文
2 中间向两边扩张法
3 DP[i][j]


```java
public String longestPalindrome(String s) {
	int len = s.length();
	if (len < 2) {
		return s;
	}
	int maxLen = 1;// 第一个字符是自己的回文子串
	int begin = 0;
	// s.charAt(i)每次都会检查数组下标越界,因此先转换成字符数组,这一步非必须
	char[] charArray = s.toCharArray();
	// 枚举所有长度严格大于1的子串 charArray[i..j]
	for (int i = 0; i < len - 1; i++) {
		
		for (int j = i+1; j < len; j++) {
			if (j - i + 1 > maxLen && validPalindromic(charArray, i, j)) {
				maxLen = j - i + 1;	
				begin = i; // 必须写到这里(记录当前回文子串开始的位置),因为如果写到该循环外面的话,i是一直走的,所以begin = i肯定到最后i 为len - 1了,
			}
		}
	}
	return s.substring(begin, begin + maxLen);
}
// 验证子串s[left...right]是否为回文串
private boolean validPalindromic(char[] charArray, int left, int right) {
	while(left < right) {
		if (charArray[left] != charArray[right]) {
			return false;
		}
			left++;
			right--;	
	}
	return true;
}
```
// 中心扩散
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200806065042218.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200806071411246.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)

```java
public String longestPalindrome(String s) {
	int len = s.length();
	if (len < 2) {
		return s;
	}
	int maxLen = 1;// 第一个字符是自己的回文子串
	int begin = 0;
	// s.charAt(i)每次都会检查数组下标越界,因此先转换成字符数组,这一步非必须
	char[] charArray = s.toCharArray();
	// 枚举所有长度严格大于1的子串 charArray[i..j]
	for (int i = 0; i < len - 1; i++) {
		int oddLen = expandAroundCenter(charArray, i, i);
		int evenLen = expandAroundCenter(charArray, i, i + 1);
		int curMaxLen = Math.max(oddLen, evenLen);
		if (curMaxLen > maxLen) {
			maxLen = curMaxLen;
			// 这一步要在纸上画图发现规律:i为当前回文子串中心的下标,(maxLen - 1) / 2是回文子串长度所对应的中心下标,当前的下标减去实际的下标就是回文子串的起始点
			begin = i - (maxLen - 1) / 2;
		}
	}
	return s.substring(begin, begin + maxLen);
}


// 验证子串s[left...right]是否为回文串
/**
@param charArray 原始字符串的字符数组
@param left 起始左边界(可以取到)
@param right 起始有边界(可以取到)
@return 回文串的长度
*/
private int expandAroundCenter(char[] charArray, int left, int right) {
	// 当left == right 回文中心是一个字符,回文串的长度是奇数
	// 当right = left + 1 ,回文中心是两个字符,回文串的长度是偶数
	int len = charArray.length;
	int i = left;
	int j = right;
	while(i >= 0 && j < len) {
		if(charArray[i] == charArray[j]) {
			i--;
			j++;
		}else{
		   break;
		}
	}
	return j - i + 1 - 2;  // j - i + 1是回文串的长度包含了i和j所对应的元素.,
	// 跳出while循环,是满足s.charAt(i) != s.charAt(j),
	// 退出循环后i和j中间的部分是回文子串不包含i和j指向的字符, 回文串的长度是j - i + 1 - 2 = j - i - 1
}
```
方法三:DP

定义dp(i, j) = true;  s[i, j]是回文串;dp(i, j) = false;  s[i, j]不是回文串
如果i和j相等,或者i和j相差一位的话,说明子串长度为0,或者为1,肯定是回文串
if i ==j or j - i + 1
接下来只要是s[i] == sj[]是i和j不断的向外扩散, i每次+1,j每次-1,就是将ture传递出去
初始状态是空或者长度为1的子串,看能不能向上扩展到更长更大的子串

```java
/*
枚举i(起点), j(终点)
*/
public String longestPalindrome(String s) {
	int n = s.length();
	String res = "";
	boolean[][] dp = new boolean[n][n];
	for (int i = n - 1; i >= 0; i--) {
		for (int j = i; j < n; j++) {
		// 只要 s.charAt(i) == s.charAt(j)就可以吧dp[i][j]扩张出去 且(dp[i+1][j-1]也是true的情况下即子串也是回文串或者是空或者长度为1的子串)
			dp[i][j] = s.charAt(i) == s.charAt(j) && (j-i < 2 || dp[i+1][j-1]);
			if (dp[i][j] && j - i + 1 > res.length()) {
				res = s.substring(i, j+1);
			}
		}
	}
	return res;
}
```
[10. 正则表达式匹配](https://leetcode-cn.com/problems/regular-expression-matching/)

> 给你一个字符串 s 和一个字符规律 p，请你来实现一个支持 '.' 和 '*' 的正则表达式匹配。
'.' 匹配任意单个字符
'*' 匹配零个或多个前面的那一个元素
所谓匹配，是要涵盖 整个 字符串 s的，而不是部分字符串。
说明:
s 可能为空，且只包含从 a-z 的小写字母。
p 可能为空，且只包含从 a-z 的小写字母，以及字符 . 和 *。
示例 1:
输入:
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。
示例 2:
输入:
s = "aa"
p = "a*"
输出: true
解释: 因为 '*' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。因此，字符串 "aa" 可被视为 'a' 重复了一次。
示例 3:
输入:
s = "ab"
p = ".*"
输出: true
解释: ".*" 表示可匹配零个或多个（'*'）任意字符（'.'）。
示例 4:
输入:
s = "aab"
p = "c*a*b"
输出: true
解释: 因为 '*' 表示零个或多个，这里 'c' 为 0 个, 'a' 被重复一次。因此可以匹配字符串 "aab"。
示例 5:
输入:
s = "mississippi"
p = "mis*is*p*."
输出: false

思路: 
第一步:暂时不管正则符号,如果两个普通字符串进行比较

```cpp
bool isMatch(string text, string pattern) {
	if (text.size() != pattern.size()) {
		return false;
	}
	for (int j = 0; j < pattern.size(); j++) {
		if (pattern[j] != text[j])
			return false;
	}
	return true;
}
```
稍微改一下为

```cpp
bool isMatch(string text, string pattern) {
	int i = 0; // text的索引位置
	int j = 0; // pattern 的索引位置
	while (j < pattern.size()) {
		if (i >= text.size())
			return false;
		if (pattern[j++] != text[i++]) {
			return false;
		}
	}
	// 判断pattern和text是否一样长
	return j == text.size()
}
```
 递归
 

```python
def isMatch(text, pattern) -> bool:
	#如果pattern为空就返回text是否为空
	if pattern is empty: return text is empty  
	#如果第一个字符是一样的就要看剩余的字符是否一样
	first_match = (text not empty) and pattern[0] == text[0] 
	# text去掉第一个字符的子串和pattern去掉第一个字符的子串,两者是否match
	return first_match and isMatch(text[1:] , pattern[1:])
```
第二步:处理点号.通配符

```python
def isMatch(text, pattern) -> bool:
	# pattern为空 返回text为空
	if not pattern: return not text
	# 如果text不为空,且pattern[0]等于text[0]或者是pattern[0]是一个点的话, first_match为true
	first_match = bool(text) and pattern[0] in {text[0], '.'}
	return first_match and isMatch(text[1:] , pattern[1:])
	
```
第三步:处理*通配符
```python
def isMatch(text, pattern) -> bool:
	# pattern为空 返回text为空
	if not pattern: return not text
	# 如果text不为空,且pattern[0]等于text[0]或者是pattern[0]是一个点的话, first_match为true
	first_match = bool(text) and pattern[0] in {text[0], '.'}
	# *永远要跟它前面的字符一起出现,因为*本身是要把前面的字符复制0份或多份
		if len(pattern) >= 2 and pattern[1] == '*':
		#发现通配符*有两种情况:
		 # 匹配该字符0次,跳过该字符和'*';意思就是把前面字符和*直接当成是0,这时候pattern要略过2位,但text没有任何略过
		 # 或者 当pattern[0]和text[0]匹配后,移动text;如果第一位符合,text匹配到1位,向后走1位,pattern不需要动因为*可以再重复出来
			return isMatch(text, pattern[2:]) or first_match and isMatch(text[1:], pattern)
	return first_match and isMatch(text[1:] , pattern[1:])
	
```
第四步 动态规划

```python
# (带备忘录的递归)
def isMatch(text, pattern) -> bool:
	memo = dict() # 备忘录
	def dp(i, j):
		if (i, j) in memo: return memo[(i, j)]
		if j == len(pattern): return i == len(text)
		first = i < len(text) and pattern[j] in {text[i], '.'}

		#如果pattern的长度小于等于2的话且后面一个为*时,说明子串字符大于2且第二个字符是*
		if j <= len(pattern) - 2 and pattern[j+1] == '*':
			ans = dp(i, j+2) or first and dp(i+1, j)
		else:
			ans = first and dp(i+1, j+1)
		memo[(i, j)] = ans
		return ans
	return dp(0, 0)
```

```python
class Solution:
    def isMatch(self, s: str, p: str) -> bool:
        S = len(s)
        P = len(p)
        memo = {}
        def dp(i, j):
            if ((i, j) in memo):
                return memo[(i, j)]
            if (j==P):
                return i ==S
            pre=i<S and p[j] in {s[i], "."}
            if (j<=P-2 and p[j+1]=="*"):
                tmp = dp(i,j+2) or pre and dp(i+1,j)
            else:
                tmp = pre and dp(i+1,j+1)
            memo[(i,j)] = tmp
            return tmp
        return dp(0,0)
```

```python
#没有备忘录的递归
def isMatch(text, pattern) -> bool:
	if not pattern: return not text
	first_match = bool(text) and pattern[0] in {text[0], '.'}
	if len(pattern) >= 2 and pattern[1] == '*':
			return isMatch(text, pattern[2:]) or first_match and isMatch(text[1:], pattern)
	return first_match and isMatch(text[1:] , pattern[1:])
```