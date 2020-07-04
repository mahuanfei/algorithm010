[200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

```java
/*
给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。
岛屿总是被水包围，并且每座岛屿只能由水平方向或竖直方向上相邻的陆地连接形成。
此外，你可以假设该网格的四条边均被水包围。
示例 1: 输入:
11110
11010
11000
00000
输出: 1

示例 2: 输入:
11000
11000
00100
00011
输出: 3
解释: 每座岛屿只能由水平或竖直方向上相邻的陆地连接而成。

思想:首先用一个嵌套的循环,循环数组中的每一个元素,如果碰到是1,说明岛屿的数量至少找到一个,所以岛屿数量加一,然后把和1左右上下相邻的所有点且相邻的无限递归下去的1全部打为0,也就是说和所有1相连接的其他的1全部都打掉,打掉的意思就是夷为平地变为0,直到循环到末尾,整个地图被我们夷为平地打成0,说明我们把所有的岛屿数量统计完了,代码实现如下:
*/
```

```java
private int n, m;

public int numIslands(char[][] grid) {
	int count = 0;
	n = grid.length;
	if (n == 0) return 0;
	m = grid[0].length;
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < m; j++) {
			if (grid[i][j] == '1') {
				DFSMarking(grid, i, j);
				++count;
			}
		}
	}
	return count;
}

private void DFSMarking(char[][] grid, int i, int j) {
	if (i < 0 || j < 0 || i >= n || j >= m || grid[i][j] != '1') return;
	grid[i][j] = '0';
	DFSMarking(grid, i + 1, j);
	DFSMarking(grid, i - 1, j);
	DFSMarking(grid, i, j + 1);
	DFSMarking(grid, i, j - 1);
}
```

###### 分发饼干 思想 先排序, 排序之后从小到大匹配两个升序的数组

```python
# 伪代码 时间复杂度为O(N), N表示小孩胃口和饼干长度较小者
res = 0;
q.sort()
s.sort()
g_length = len(g)
s_length = len(s)
i = 0
j = 0
while i < g_length and j < s_length:
	if g[i] <= s[j]:
		#可以满足胃口, 把小饼干喂给小朋友
		res += 1
		i += 1
		j += 1
	else: 
	    # 不满足胃口, 查看下一块小饼干
		j += 1
return res
```

```swift
  func findContentChildren(_ g: [Int], _ s: [Int]) -> Int {
        let g = g.sorted()
        let s = s.sorted()
        var startG = 0
        var startS = 0
        var resultCount = 0
        while startG < g.count && startS < s.count {
            if  s[startS] >= g[startG]{
                startG += 1
                resultCount += 1
            }
            startS += 1
        }
        return resultCount
    }
```

[121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)
方法一:暴力法: 时间复杂度O(n^2), 空间复杂度O(1)
> 给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。
如果你最多只允许完成一笔交易（即买入和卖出一支股票一次），设计一个算法来计算你所能获取的最大利润。
注意：你不能在买入股票前卖出股票。
示例 1:
输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
     示例 2:
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```swift
func maxProfit(_ prices: [Int]) -> Int {
	if prices.isEmpty {return 0}
	var maxProfit = 0
	for i in 1..prices.count - 1 {
		for j in i..prices.count {
			if prices[j] > prices[i - 1] {
				let difValue = prices[j] - prices[i - 1]
				maxProfit = maxProfit > difValue ? maxProfit : difValue
			}
		}
	}
	return maxProfit
}
```
方法二:时间复杂度O(n), 空间复杂度O(1)
```swift
   func maxProfit(_ prices: [Int]) -> Int {
        if prices.isEmpty {
            return 0
        }
        var maxProfit = 0
        var minValue = Int.max
        for i in 0..<prices.count {
            if prices[i] < minValue {
                minValue = prices[i]
            }else if prices[i] - minValue > maxProfit {
                maxProfit = prices[i] - minValue
            }
        }
        return maxProfit
    }
```
[122. 买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)
> 给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。
设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。
注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
示例 1:
输入: [7,1,5,3,6,4]
输出: 7
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6-3 = 3 
示例 2:
输入: [1,2,3,4,5]
输出: 4
解释: 在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。
     因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
示例 3:
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。

方法一
```swift
//[7,1,5,3,6,4]
   func maxProfitII(_ prices: [Int]) -> Int {
         if prices.isEmpty {
             return 0
         }
         var maxProfit = 0
         var minPrice = Int.max
         for i in 0..<prices.count {
             if prices[i] < minPrice {
                minPrice = prices[i]
             }else if (prices[i] > minPrice) {
                 maxProfit += prices[i] - minPrice
                 minPrice = prices[i]
             }
         }
         return maxProfit
     }
```
方法二
```swift
func maxProfitII1(_ prices: [Int]) -> Int {
        var maxProfit = 0
        for i in 1..<prices.count {
            if prices[i] - prices[i - 1] > 0 {
                maxProfit += prices[i] - prices[i - 1]
            }
        }
        return maxProfit
    }
```
[55. 跳跃游戏](https://leetcode-cn.com/problems/jump-game/)

> 给定一个非负整数数组，你最初位于数组的第一个位置。
数组中的每个元素代表你在该位置可以跳跃的最大长度。
判断你是否能够到达最后一个位置。
示例 1:
输入: [2,3,1,1,4]
输出: true
解释: 我们可以先跳 1 步，从位置 0 到达 位置 1, 然后再从位置 1 跳 3 步到达最后一个位置。
示例 2:
输入: [3,2,1,0,4]
输出: false
解释: 无论怎样，你总会到达索引为 3 的位置。但该位置的最大跳跃长度是 0 ， 所以你永远不可能到达最后一个位置。


```swift
/*
伪代码
int reach = 0, n = size()
for (int i = 0; i < n; i++) {
	if (i > reach) { //  假如当前遍历的i已经超过了reach, 表明这个位置是我们跳不到的,因为i已经遍历到了我们已经到达最远位置更靠后的i的位置上, 直接返回false, 如果不是这种情况,就表示当前位置是我们可以到的,并且我们可以在这个位置上继续向前跳远
		return false;
	}
	reach = Math.max(i + A[i], reach);
}  // 从i = 0到i = 1全部都遍历到了
return true
*/

 func canJump(_ nums: [Int]) -> Bool {
        if nums.isEmpty {
            return false
        }
       var reach = 0
       for i in 0..<nums.count {
           if i > reach { return false }
           reach = max(i + nums[i], reach)
       }
       return true
}
```
方法二:
```java
  func canJumpI(_ nums: [Int]) -> Bool {
      if nums.count <= 1 {
          return true
      }
      var lastPosition = nums.count - 1
      for i in (0..<nums.count).reversed() {
          if i + nums[i] >= lastPosition {
              lastPosition = i
          }
      }
      return lastPosition == 0
   }
```
方法三(1):低效的回溯

> 思想,比如[2,3,1,1,4],从第0个位置开始,最远跳(该位置索引 + 该位置元素值),比如当前索引为0,值为2,那么最远可以跳2步,当然也可以跳1步,如果选择跳1步(相同的回溯步骤)返回false那么就选择跳2步的直到返回true,如果跳1步和跳2步都返回false,那么表示无法到达最后的位置
> if canJumpFromPosition(nextPosition, nums) {
				return true
	}
			注意这一步不能写为 
  if canJumpFromPosition(nextPosition, nums) {
				return true
 }else {
 			  return false
 }
如果这样写了,那么表示选择了第一步返回false,后面的第二步,第三步,就不能再试了,所以这不返回false
就表示,可以通过其他选择路径看看是否可以返回true,有一种位置返回true的那么就一肯定能到达,
如果全部选择都试过都不能返回true,最后就返回false,表示任何种走法都不能到达nums.count -1这个位置
```swift
func canJump_backtrace(_ nums: [Int]) -> Bool {
	return canJumpFromPosition(0, nums);
}

func canJumpFromPosition(_ position: Int, _ nums: [Int]) -> Bool {
	if position == nums.count - 1 {
		return true
	}
	let furthestJump = min(position + nums[position], nums.count - 1)
	for nextPosition in stride(from: position + 1, through: furthestJump, by: 1) {
			if canJumpFromPosition(nextPosition, nums) { // 注意点
				return true
			}
	}
	return false
}
```

方法三(2)
```swift
class JampGame {
    enum Index {
              case Good
              case Bad
              case Ugly
    }
    var memo = [Index]()
          
   func canJumpFormUpToBottom_dp(_ nums: [Int]) -> Bool {
       memo = [Index](repeating: .Ugly, count: nums.count)
       memo[nums.count - 1] = .Good
//           return canJumpFormPosition_dp(0, nums)
       return canJumpFormBottomToUp_dp(nums)
   }
   

 //MARK: 自顶向下动态规划
   func canJumpFormPosition_dp(_ position: Int, _ nums: [Int]) -> Bool {
       if memo[position] != .Ugly {
           return memo[position] == .Good
       }
       let furthestPosition = min(position + nums[position], nums.count - 1)
       for nextPosition in stride(from: position + 1, to: furthestPosition, by: 1) {
           if canJumpFormPosition_dp(nextPosition, nums) {
               memo[nextPosition] = .Good
               return true
           }
       }
       memo[position] = .Bad
       return false
       
   }
   // [2,3,1,1,4]
 //MARK: 自底向上动态规划
   func canJumpFormBottomToUp_dp(_ nums: [Int]) -> Bool {
      
       for i in stride(from: nums.count - 2, to: 0, by: -1) {
           let furthestPosition = min(i + nums[i], nums.count - 1)
           for j in stride(from: i + 1, to: furthestPosition, by: 1) {
               if memo[j] == .Good {
                   memo[i] = .Good
                   break
               }
           }
       }
       return memo[0] == .Good
   }
}
```


