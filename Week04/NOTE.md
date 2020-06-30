学习笔记
/****************深度优先搜索和广度优先搜索总结****************/
#### 深度优先搜索和广度优先搜索

#### 搜索-遍历

 - 每个节点都要访问一次
 - 每个节点仅访问一次
 - 对于节点的访问顺序不同
  深度优先: depth first search
  广度优先: breadth first search
 优先级优先搜索(启发式搜索,推荐算法)适用于现实中业务场景



##### 深度优先搜索 depth first search
一开始是递归的终止条件, 处理当前层,然后递归到下一层,
 处理当前层,相等于访问了node, 将node节点加到已访问的节点去,所以终止条件就是,如果这个节点已经访问过了,就直接返回.
递归到下一层, 就是处理子节点, 二叉树(左孩子,右孩子), 多叉树(children),图(联通的相邻节点)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200629065039744.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)
多叉树和二叉树两种遍历模板基本相同, 不同是二叉树遍历模板把判断一个节点是否被访问过放在函数的最开始, 而多叉树遍历模板是把判断一个节点是否被访问过放在遍历children节点的过程中,但是这里要保证,在多叉树最开始递归的那个节点是没有被访问过的.
针对上述多叉树 的遍历模板为了和树的遍历模板一致,就改为下面DFS代码-递归写法
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200629084155103.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)
这个也支持非递归写法,就是手动维护一个栈,将其加进去,其实是手动模拟了一个递归,用栈来进行表示.
######  树的深度优先遍历顺序
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200629082859847.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)
 树遍历顺序过程: 
      比如刚开始访问的是root,这样root就先放到visited中,root在visited之后就从root.children里面找next_node, 所以它的next_node都没有被访问过,所以会先访问最左边结点, 这里当最左边的节点被拿出来,判断没有被visited,因为处了root visited过其他节点都还未visited过,那么就会直接调用dfs, next_node就把最左边节点放进去,再把visited也一起放进去, 递归调用的特色是不会等循环跑完,就直接进入到下一层,也就是当前梦境,写了一层循环,在第一层循环的时候,我就要开始下到新的一层梦境中.
所以 当访问1之后马上在1这层循环中就会访问2,访问2的时候再马上继续下层3,同理4,看4, 3, 2依次有无儿子,发现没有,返回到1, 1有其他儿子5,然后6, 7...这么一个顺序.

######  图的深度优先遍历顺序
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200629082703420.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)
假设从S节点开始走,先走A,选择走A之后,它不会继续走B和C,走A后马上走更深一层的下一个节点D,依次走D,G,E,B,在走S时发现走过了,所以就不走了就会回到G之后再走下一个分支F,再走C...发现都走完了.

##### 广度优先搜索 breadth first search
广度优先遍历用队列.
广度优先遍历顺序是一层一层向下
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200629085202785.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)

```python
def BFS(graph, start, end):
	queue = []
	queue.push([start])  // 或者 queue.append([start])  ???? 

	while queue:
		node = queue.pop() 
		visited.add(node)

		process(node) 
		nodes = generate_related_nodes(node) 
		queue.push(nodes)

	# other processing work 
```
有一个排队的队列queue,先把最开始的节点放到队列中,每次从队列里面取之后,把儿子节点再放到队列中,依次处理, 先入先出.


##### 对比两种遍历方式
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200629090155993.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)
实战 二叉树的层序遍历
[102. 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

```java
/*
给你一个二叉树，请你返回其按 层序遍历 得到的节点值。 （即逐层地，从左到右访问所有节点）。
示例：
二叉树：[3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回其层次遍历结果：

[
  [3],
  [9,20],
  [15,7]
]
*/
```

方法一:BFS(按层来一层一层直接遍历)
######  标准的层序遍历BFS使用队列数据结构
```java
void bfs(TreeNode root) {
    Queue<TreeNode> queue = new ArrayDeque<>();
    queue.add(root);
    while (!queue.isEmpty()) {
        TreeNode node = queue.poll(); // Java 的 pop 写作 poll()
        if (node.left != null) {
            queue.add(node.left);
        }
        if (node.right != null) {
            queue.add(node.right);
        }
    }
}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200629102637175.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)
稍微修改一下代码，在每一层遍历开始前，先记录队列中的结点数量 n（也就是这一层的结点数量），然后一口气处理完这一层的 n 个结点。

```java
// 二叉树的层序遍历
void bfs(TreeNode root) {
    Queue<TreeNode> queue = new ArrayDeque<>();
    queue.add(root);
    while (!queue.isEmpty()) {
        int n = queue.size();
        for (int i = 0; i < n; i++) { 
            // 变量 i 无实际意义，只是为了循环 n 次
            TreeNode node = queue.poll();
            if (node.left != null) {
                queue.add(node.left);
            }
            if (node.right != null) {
                queue.add(node.right);
            }
        }
    }
}
```
这样，我们就将 BFS 遍历改造成了层序遍历
最终我们得到的题解代码为:
```java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> res = new ArrayList<>();
    Queue<TreeNode> queue = new ArrayDeque<>();
    if (root != null) {
        queue.add(root);
    }
    while (!queue.isEmpty()) {
        int n = queue.size();
        List<Integer> level = new ArrayList<>();
        for (int i = 0; i < n; i++) { 
            TreeNode node = queue.poll();
            level.add(node.val);
            if (node.left != null) {
                queue.add(node.left);
            }
            if (node.right != null) {
                queue.add(node.right);
            }
        }
        res.add(level);
    }
    return res;
}

```
方法一(2):利用层序遍历
```python
def levelOrder(self, root: TreeNode) -> List[List[int]]:
	if root is None:
		return []
	queue = [root]  #把root初始化进去
	out = []
	while queue:
		child = []
		nodeQueue = []
		for item in queue:
			child.append(item.val)
			if item.left:
				nodeQueue.append(item.left)
			if item.right:
				nodeQueue.append(item.right)
			out.append(child)
			queue = nodeQueue
	return out
			
```

方法二:DFS(把元素加到它相应所在的层即可,虽然DFS访问的时候是按照深度来访问的,但是访问的时候都有level,它处在那一深度,所以每次把相应的节点加到深度所在的数组中即可)

```java
List<List<Integer>> levels = new ArrayList<List<Integer>>();
public List<List<Integer>> levelOrder(TreeNode root) {
	if (root == null) return levels;
	helper(root, 0);
	return levels;
}
public void helper(TreeNode node, int level) {
	if (levels.size() == level) {
		levels.add(new ArrayList<Integer>());
	}
	levels.get(level).add(node.val);
	if (node.left != null) {
		helper(node.left, level + 1);
	}
	if (node.right != null) {
		helper(node.right, level + 1);
	}
}
```




/****************贪心算法Greedy总结****************/
贪心算法是一种在每一步选择中都采用在当前状态下最好或最优(即最有利)的选择, 从而希望导致结果是全局最好或者
最优的算法.(用贪心算法,尤其是最基础的贪心,每次当下情况下找到最优不一定能够达到全局最优的情况)
贪心算法与动态规划的不同在于它对每个子问题的解决方案都做出选择,不能回退.动态规划会保存以前的运算结果, 并根据以前的结果对当前进行选择, 有回退功能.
###### 对比:
贪心: 当下做局部最优判断
回溯: 能够回退
动态规划: 最优判断 + 回退 (带最优判断的回溯,我们叫做动态规划,动态规划会保存以前的运算结果, 并根据以前保存的结果, 对当前进行选择有回退功能)

贪心法可以解决一些最优化问题, 如: 求图中的最小生成树,求哈夫曼编码等. 然而对于工程和生活中的问题, 贪心法一般不能得到我们想要的答案, 一旦一个问题可以通过贪心法来解决,那么贪心法一般是解决这个问题的最好办法.由于贪心法的高效性以及其所求得的答案比较接近最优结果, 贪心法也可以用作辅助算法,或者解决一些要求结果不特别精确的问题

###### 应用实例 零钱兑换
[322 零钱兑换](https://leetcode-cn.com/problems/coin-change/)
```java
/*
给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 -1。

示例 1: 输入: coins = [1, 2, 5], amount = 11
输出: 3 
解释: 11 = 5 + 5 + 1

示例 2: 输入: coins = [2], amount = 3 
输出: -1
说明: 你可以认为每种硬币的数量是无限的。
*/
```
当硬币可选集合固定: Coins = [20, 10, 5, 1]
求最少可以几个硬币拼出总数, 比如total = 36
首先用20先去匹配, 20最多可以匹配多少个,我们就把20能匹配的个数全部都用总数减去,现在总数为36,所以总数为1个,也就是36除以20,整除除得多少表示20可以用多少个,这里只有一个,那么得到16, 再看10,  在看5, 在看1, 所以这里贪心法是先用最大的去匹配, 再用次大的再匹配
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200701053228721.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)
为什么可以用贪心法,是因为硬币的话20 10 5 1前面的硬币依次是后面这些硬币的倍数, 所以如果你需要用两个10,或者4个5的话,肯定还不如用一个20,因为后面都是整除前面最大的硬币,贪心法每次用最大的即可, 既然用20,那么肯定后面这些会不优于直接选20的情况, 贪心法有它的特殊性, 这个硬币有整除的关系,所以用贪心法, 但在大部分情况下
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200701053814878.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)
###### 适用贪心算法的场景
简单地说, 问题能够分解成子问题来解决, 子问题的最优解能递推到最终问题的最优解,这种子问题最优解称为最优子结构.
贪心算法与动态规划的不同在于它对每个子问题的解决方案都做出选择,不能回退.动态规划则会保存以前的运算结果, 并根据以前结果对当前进行选择, 有回退功能
 
 在现实生活中很少用到贪心, 你要是每天只满足自己当下的需求, 你觉得游戏好就打游戏,或者饿了饱吃一顿, 每天都吃很多类似于这样的东西, 最后发现长期是达不到最优解的,比如时间都浪费在一些可能提高自己的短期比较痛苦但长期的话,对你有益的事情上面,以及要自律,保持体重,或者是要去健身这些, 这些都是短期让你痛苦,长期让你受益的行为

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
###### 最佳买卖股票时期 输入[7, 1, 5, 3, 6 ,4] 输出7, 在1这买入,在5这卖出,赚4元, 然后在3买入,在6卖出,赚3元 , 4+3 = 7这就是所谓的结果, 输入[1, 2, 3, 4, 5]在第一天买入, 一直是前一天买后一天抛直到最后,赚4元.所以这个思路就是只要后一天比前一天大,我们就在前一天买入,后一天抛
```java
// 一
public int maxProfit(int[] prices) {
	int maxProfit = 0;
	for (int i = 1; i < prices.length; i++) {
		if (prices[i] > prices[i - 1]) {
			maxProfit += prices[i] - prices[i - 1];
		}	
	}
	return maxProfit;
}

// 二
public int maxProfit(int[] prices) {
	int maxProfit = 0;
	for (int i = 0; i < prices.length - 1; i++) {
		if (prices[i + 1] > prices[i]) {
			maxProfit += prices[i + 1] - prices[i];
		}	
	}
	return maxProfit;
}
```
###### 跳跃游戏 从后往前贪心

```java
// 例如输入为 [2, 3, 1, 1, 4]   或者输入为[3, 2, 1, 0, 4]
public boolean canJump(int[] nums) {
	if (nums == null) {
		return false;
	}
	int endReachable = nums.length - 1;
	for (int i = nums.length - 1; i >= 0; i--) {
	 // 表示从i这个点最多可以跳多远, 如果它最多可以跳的距离大于最后的一个能够跳到最终点的下标的话, 说明i的话是可以跳到最后点的, 那么就把i更新到endReachable, endReachable始终指定是最前面一个下标它能够跳到最后点, 那么这个从最后往前不断进行循环,且每时候都检查,如果当前的i能够跳到最后的话, 那么则把i更新到endReachable,  最后判断endReachable是不是为0, 是0的话表示第一个坐标的位置也可以跳到最后, 为什么这用到贪心,是因为只要记能够跳到最后的那个位置的第一个值就是记这么一个最前者的值, 就节省了一个数组来记录重要的结果, 当然也可以节省一层循环
		if (nums[i] + i >= endReachable) {
			endReachable = i;
		}
	}
	return endReachable == 0;
}
```
