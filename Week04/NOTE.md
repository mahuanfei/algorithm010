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
