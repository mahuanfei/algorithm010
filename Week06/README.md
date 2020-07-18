[64. 最小路径和](https://leetcode-cn.com/problems/minimum-path-sum/)

> 给定一个包含非负整数的 m x n 网格，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。
说明：每次只能向下或者向右移动一步。
示例:
输入:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
输出: 7
解释: 因为路径 1→3→1→1→1 的总和最小。

```swift
 func minPathSum(_ grid: [[Int]]) -> Int {

         var result = [[Int]](repeating: [Int](repeating: 0, count: grid[0].count), count: grid.count)
                // 注意4处这里的是reuslt不是grid
                 for i in (0..<grid.count).reversed() {
                     for j in (0..<grid[i].count).reversed() {
                         if i == grid.count - 1 && j == grid[i].count - 1 {
                             result[i][j] = grid[i][j]
                         }else if (i == grid.count - 1 && j != grid[i].count - 1) {
                             result[i][j] = grid[i][j] + result[i][j+1]
                         }else if (i != grid.count - 1 && j == grid[i].count - 1) {
                             result[i][j] = grid[i][j] + result[i+1][j]
                         }else if (i != grid.count - 1 && j != grid[i].count - 1) {
                             result[i][j] = grid[i][j] + min(result[i+1][j], result[i][j+1])
                         }
                     }
                 }
                 return result[0][0]
    }
```

```swift
  func minPathSum2(_ grid: [[Int]]) -> Int {

        var result = grid[grid.count - 1]
        for i in (0..<grid.count).reversed() {
                for j in (0..<grid[i].count).reversed() {
                    if i == grid.count - 1 && j == grid[i].count - 1 {
                           result[j] = grid[i][j]
                       }else if (i == grid.count - 1 && j != grid[i].count - 1) {
                           result[j] = grid[i][j] + result[j+1]
                       }else if (i != grid.count - 1 && j == grid[i].count - 1) {
                           result[j] = grid[i][j] + result[j]
                       }else if (i != grid.count - 1 && j != grid[i].count - 1) {
                           result[j] = grid[i][j] + min(result[j], result[j+1])
                       }
                }
        }        
        return result[0]        
    }
```

```swift
func minPathSum3(_ grid: [[Int]]) -> Int {

        var grid = grid
        for i in (0..<grid.count).reversed() {
                for j in (0..<grid[i].count).reversed() {
                    if i == grid.count - 1 && j == grid[i].count - 1 {
                           
                       }else if (i == grid.count - 1 && j != grid[i].count - 1) {
                           grid[i][j] +=  grid[i][j+1]
                       }else if (i != grid.count - 1 && j == grid[i].count - 1) {
                           grid[i][j] +=  grid[i+1][j]
                       }else if (i != grid.count - 1 && j != grid[i].count - 1) {
                           grid[i][j] +=  min(grid[i+1][j], grid[i][j+1])
                       }
                }
        }      
        return grid[0][0]        
    }
```
[91. 解码方法](https://leetcode-cn.com/problems/decode-ways/)

> 一条包含字母 A-Z 的消息通过以下方式进行了编码：
'A' -> 1
'B' -> 2
...
'Z' -> 26
给定一个只包含数字的非空字符串，请计算解码方法的总数。
示例 1:
输入: "12"
输出: 2
解释: 它可以解码为 "AB"（1 2）或者 "L"（12）。
示例 2:
输入: "226"
输出: 3
解释: 它可以解码为 "BZ" (2 26), "VF" (22 6), 或者 "BBF" (2 2 6) 。


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200718171845906.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)


```java
public int numDecodings(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        int len = s.length();
        int[] dp = new int[len + 1];
        dp[len] = 1;
        if (s.charAt(len - 1) == '0') {
            dp[len - 1] = 0;
        } else {
            dp[len - 1] = 1;
        }
        for (int i = len - 2; i >= 0; i--) {
            if (s.charAt(i) == '0') {
                dp[i] = 0;
                continue;
            }
            if ((s.charAt(i) - '0') * 10 + (s.charAt(i + 1) - '0') <= 26) {
                dp[i] = dp[i + 1] + dp[i + 2];
            } else {
                dp[i] = dp[i + 1];
            }
        }
        return dp[0];
    }
```

```swift
class Solution {
    func numDecodings(_ s: String) -> Int {
            if s.count == 0 {return 0}
            let len = s.count
            var dp = [Int](repeating: 0, count: len + 1)
            dp[len] = 1
            if s[len - 1] == "0" {
                dp[len - 1] = 0
            }else{
                dp[len - 1] = 1
            }
            
//      for i in (0...len - 2).reversed()  //当 len = 1时报错,[0...-1]
        for i in stride(from: len - 2, through: 0, by: -1) {
            if s[i] == "0" {
               dp[i] = 0
               continue
           }
            if Int(String(s[i]))! * 10 + Int(String(s[i + 1]))! <= 26 {
               dp[i] = dp[i+1] + dp[i+2];
           }else {
               dp[i] = dp[i+1]
           }
        }
    
        return dp[0]
    }
}
extension String {
    subscript(i: Int) -> Character {
        return self[index(startIndex, offsetBy:i)]
    }
}
```
细心的话，会发现我们其实并不需要申请一个长度为len+1的数组来存储中间过程。其实dp[i]只和dp[i+1]以及dp[i+2]相关。
因此，此处可以继续空间压缩。

```java
 public int numDecodings(String s) {
        if (s == null || s.length() == 0) { return 0;}
        int len = s.length();
        int help = 1;
        int res = 0;
        if (s.charAt(len - 1) != '0') { res = 1;}
        for (int i = len - 2; i >= 0; i--) {
            if (s.charAt(i) == '0') {
                help = res;
                res = 0;
                continue;
            }
            if ((s.charAt(i) - '0') * 10 + (s.charAt(i + 1) - '0') <= 26) {
                res += help;
                //help用来存储res以前的值
                help = res-help;
            } else {
                help = res;
            }
        }
        return res;
    }
```

```swift
   func numDecoding1(_ s: String) -> Int {
       if s.count == 0 {return 0}
       let len = s.count
       var next = 1
       var current = 0       
       if s[len - 1] != "0" {
           current = 1
       }
       for i in stride(from: len - 2, through: 0, by: -1) {
           if s[i] == "0" {
              next = current
              current = 0
              continue
          }
           if Int(String(s[i]))! * 10 + Int(String(s[i + 1]))! <= 26 {       
              current = next + current
              next = current - next
          }else {
              next = current
          }
       }
       return current
    }
```

[120. 三角形最小路径和](https://leetcode-cn.com/problems/triangle/)

> 给定一个三角形，找出自顶向下的最小路径和。每一步只能移动到下一行中相邻的结点上。
相邻的结点 在这里指的是 下标 与 上一层结点下标 相同或者等于 上一层结点下标 + 1 的两个结点。
例如，给定三角形：
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
自顶向下的最小路径和为 11（即，2 + 3 + 5 + 1 = 11）。
说明：
如果你可以只使用 O(n) 的额外空间（n 为三角形的总行数）来解决这个问题

```python
def minmumTotal(self, triangle):
	f = [0] * (len(triangle) + 1)  #traingle的行数为4,初始化一个数组中5个元素为0 f  = [0, 0, 0, 0, 0] 
	for row in triangle[::-1]:   #反向 从倒数第一行开始
		for i in xrange(len(row)):
			f[i] = row[i] + min(f[i], f[i + 1]) 
	return f[0]
```

> 倒数第一层: f[0] = 4 + min(f[0], f[1]) = 4, f[1] = 1 + min(f[1], f[2])  = 1, f[2] = 8 + min(f[2], f[3]) = 8, f[3] = 3 + min(f[3], f[4]) = 3
倒数第二层:f[0] = 6 + min(f[0], f[1]) = 7, f[1] = 5 + min(f[1], f[2]) = 6, f[2] = 7 + min(f[2], f[3]) = 10
倒数第三层: f[0] = 3 + min(f[0], f[1]) = 9, f[1] = 4 + min(f[1], f[2]) = 10
第一层: f[0] = 2 + min(f[0], f[1]) = 11

```python
def minmumTotal(self, triangle) {
	dp = triangle
	for i in range(len(triangle) -2, -1,-1):#从倒数第二层走,反向,每次步长为-1
		for j in range(len(triangle[i])):
			dp[i][j] += min(dp[i + 1][j], dp[i+1][j+1])
	return dp[0][0]
}
```
方法二:递归

```swift
 // 递归:
    func minimumTotal(_ triangle: [[Int]]) -> Int {
        return recursion(triangle, 0, 0)
    }
    
    func recursion(_ triangle: [[Int]], _ i: Int, _ j: Int) -> Int {
        if i == triangle.count - 1 {return triangle[i][j]}
        return min(recursion(triangle, i + 1, j), recursion(triangle, i + 1, j + 1)) + triangle[i][j]
    }
```
方法三:DP
```swift
   func minimumTotalDP(_ triangle: [[Int]]) -> Int {
          let rowLen = triangle.count
          var minLen = [Int](repeating: 0, count: rowLen + 1)
          for row in (0..<rowLen).reversed() {
              for i in 0..<triangle[row].count {
                  minLen[i] = triangle[row][i] + min(minLen[i], minLen[i + 1])
              }
          }
          return minLen[0]
      }
```
方法四: Top-Down 

```python
def minimumTotal1(self, triangle):
	if not triangle:
		return
	res = [[0 for i in xrange(len(row))] for row in triangle]
	res[0][0] = triangle[0][0]
	for i in xrange(1, len(triangle)):
		for j in xrange(len(triangle[i])):
			if j == 0:
				res[i][j] = res[i-1][j] + triangle[i][j]
			elif j == len(triangle[i])-1:
				res[i][j] = res[i-1][j-1] + triangle[i][j]
			else:
				res[i][j] = min(res[i-1][j-1], res[i-1][j]) + triangle[i][j]
	return min(res[-1])
```

```python
def minimumTotal2(self, triangle):
	if not triangle:
		return
	for i in xrange(1, len(triangle)):
		for j in xrange(len(triangle[i])):
			if j == 0:
				triangle[i][j] += triangle[i-1][j]
			elif j == len(triangle[i]) - 1:
				triangle[i][j] += triangle[i-1][j-1]
			else:
				triangle[i][j] += min(triangle[i-1][j-1], triangle[i-1][j])
	return min(triangle[-1])		
			
```
方法五:bottom-up
```python
def minimumTotal2(self, triangle):
	if not triangle:
		return
	for i in xrange(len(triangle)-2, -1, -1):
		for j in xrange(len(triangle[i])):
			triangle[i][j] += min(triangle[i-1][j-1], triangle[i-1][j])
	return triangle[0][0]

```

```python
def minimumTotal2(self, triangle):
	if not triangle:
		return
	res = triangle[-1]
	for i in xrange(len(triangle) - 2, -1, -1):
		for j in xrange(len(triangle[i])):
			res[j] = min(res[j], res[j+1]) + triangle[i][j]
	return res[0]
```
[53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)

> 给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。
示例:
输入: [-2,1,-3,4,-1,2,1,-5,4],
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
进阶:
如果你已经实现复杂度为 O(n) 的解法，尝试使用更为精妙的分治法求解。

1.暴力 n^2
2 DP
	a. 分治(子问题) :定义子问题,首先经验是从后往前走,假设要取4且以4结尾,前面的话最大的子序列是多少, max_sum(i)就等于max_sum(i - 1)包含进去的最大的和,或者是不包含i-1就是0,他们之间取一个最大值,加上自己的值
		max_sum(i) = Max(max_sum(i - 1), 0) + a[i]
	b.状态数组定义: f[i]
	c.DP方程: f[i] = MAX(f[i-1],0) + a[i]
    也可以这么思考,不断的用一个sum进行累加,如果累加到一个地方为负数,就丢掉,直到每一步都找一个最大值
```python
"""
1 dp问题 公式 dp[i] = max(nums[i], nums[i] + dp[i - 1])
2最大子序列和 = 当前元素自身最大, 或者当前元素之前的加上自身元素最大
"""
def maxSubArray(nums):
	dp = nums
	for i in range(1, len(nums)):
		dp[i] = max(nums[i], nums[i] + dp[i - 1])
	return max(dp)
```
直接复用nums
```python
def maxSubArray(nums):
	for i in range(1, len(nums)):
		nums[i] = max(nums[i], nums[i] + nums[i - 1])
	return max(nums)
```

```python
def maxSubArray(nums):
	for i in range(1, len(nums)):
		nums[i] = max(0, nums[i - 1]) + nums[i]
	return max(nums)
```
[152. 乘积最大子数组](https://leetcode-cn.com/problems/maximum-product-subarray/)

> 给你一个整数数组 nums ，请你找出数组中乘积最大的连续子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。
示例 1:
输入: [2,3,-2,4]
输出: 6
解释: 子数组 [2,3] 有最大乘积 6。
示例 2:
输入: [-2,0,-1]
输出: 0
解释: 结果不能为 2, 因为 [-2,-1] 不是子数组。
思路:
遍历数组时计算当前最大值，不断更新
令imax为当前最大值，则当前最大值为 imax = max(imax * nums[i], nums[i])
由于存在负数，那么会导致最大的变最小的，最小的变最大的。因此还需要维护当前最小值imin，imin = min(imin * nums[i], nums[i])
当负数出现时则imax与imin进行交换再进行下一步计算
时间复杂度：O(n)

```python3
    def maxProduct(self, nums: List[int]) -> int:
        mi = ma = res = nums[0]
        for i in range(1, len(nums)):
            if nums[i] < 0: mi, ma = ma, mi
             ma = max(ma * nums[i], nums[i])
             mi = min(mi * nums[i], nums[i])
            res = max(res, ma)
        return res
```
[198. 打家劫舍](https://leetcode-cn.com/problems/house-robber/)

> 你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。
给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。
示例 1：
输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
示例 2：
输入：[2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
 提示：
0 <= nums.length <= 100
0 <= nums[i] <= 400

```swift
 func rob(_ nums: [Int]) -> Int {
        if nums.count <= 0 {return 0}
        if nums.count == 1 {return nums[0]}
        var result = [Int](repeating: 0, count: nums.count)
        result[0] = nums[0]
        result[1] = max(nums[0], nums[1])
        for i in 2..<nums.count {
            result[i] = max(result[i - 1], nums[i] + result[i - 2])
        }
        return result[nums.count - 1]
    }
```

```swift
 func rob(_ nums: [Int]) -> Int {
        if nums.count <= 0 {return 0}
        if nums.count == 1 {return nums[0]}
        var result = [Int](repeating: 0, count: nums.count)
        var first = nums[0]
        var second = max(nums[0], nums[1])
        for i in 2..<nums.count {
            var temp = second
            second = max(second, nums[i] + first)
            first = temp
        }
        return second
    }
```