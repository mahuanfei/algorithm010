学习笔记

/****************递归学习总结****************/
递归 Recursion
递归 - 循环
通过函数体来进行的循环

因为计算机本身用的汇编,而汇编有个特点,就是没有所谓的循环嵌套这么一说,很多时候用的
更多的是你之前有一段函数写在什么地方,或者一段指令写在什么地方,就会直接不断地反复跳到
之前的那段指令,不断的执行,其实这就是所谓的递归
循环本身编译处理后的汇编代码,其实和递归本身有异曲同工之处

现实生活中也有所谓的重复性 电影盗梦空间
主线:从飞机开始,然后到城市,再往下不断地递归, 最后在一个雪上的屋子里
每进入一层递归就是 一个新的世界,或者互不干扰的世界,在里面做些事情
再下到另外一层再做些事情, 如果要返回现实世界,必须一层一层再回来
每次下去和回来的时候自身发生改变(把自己的状态带到下一层,发生改变后再带回来)(参数)
但是环境里面的东西是不受影响, 下一层的环境不会影响这一层的环境和人物,那么主角
和主角相关的人是函数的参数
盗梦空间
递归要点:
1 向下进入到不同梦境中:向上回到原来一层 (一层一层下, 一层一层回来,不能跳跃 对称性,归去来兮的感觉)
2 通过声音同步回到上一层(所谓同步就是用参数来进行函数不同层之间传递变量)
3 每一层环境和周围的人都是一份拷贝,也就是进入每一层的房子建筑,电脑,无关的人物,其实就是
创造了一份新 的世界,当吧这个世界全部打坏了,去到下一层和上一层它们的建筑都是不受影响的
主角可以穿越不同的梦境同时把自身和自己所有携带的东西,都可以带到不同梦境发送变化且可以把变化携带回来(类似于函数参数)
递归运行方式递归栈
factorial(3)
3 *(factorial(2))
3 *(2 * factorial(1))
3 *(2 *1)
3 *2
6
系统就给我们做了这样一个调用栈(一层一层展开,更像剥洋葱.类似于栈的形式,一层一层进去,再剥开),而栈本身就是递归调用的时候系统做了这样一个调用栈
```java
public void recur(int level, int param) {
	// terminator
	if (level > MAX_LEVEL) {
		// process result
		return;
	}
	
	// process current logic
	process(level, param);
	
	//  drill down
	recur(level+1, newPara…)

	// restore current status	
}

```
写递归,
第一部分,一定要先把递归终止条件写上(这点不注意就会造成结果是无线递归或者死循环)
第二部分, 这个递归层次, 处理这一层的逻辑(完成业务代码,逻辑代码)
第三部分, 这层解决完了,下到下一层去 (用参数标记当前层,是哪一层)将level变成 level + 1同时
传递相应的参数
第四部分: 递归完了 清理当前层(有些时候不需要清理,因为这一层本身环境是拷贝处理的,但很多时候会有一些全局变量进行清理)

递归思维要点:
1 抵制人肉进行递归(最大误区) 应该直接看函数本身写 // 主要是针对熟练的人 初学者要花递归状态树
2 找最近重复性(找到最近最简方法,将其拆解成可重复解决的问题)
3 数学归纳法思维 (最开始条件成立,比如n=1,n=2的时候成立,且可以证明当n成立的时候可以推导出n+1也成立)

递归脑子里这四部分一定要清晰
验证二叉搜索树
BST(binary search tree) :中序遍历是递增的

理解起来一遍看不明白很正常(要反复看几次)这个过程要掌握
解题与学习过程
错误思路:不看左子树也不看右子树,只看了左节点和右节点,这个是错误的,不仅仅要看它的左儿子和右儿子和根节点的大小,还要看整个左子树右子树的大小

二叉树最大深度是什么(重复项)
最大深度来自两个地方
1 左子树深度 + 根
2 右子树深度 + 跟

/****************分治和回溯总结****************/
 (Divide & Conquer)分治和回溯本质上就是特殊的递归, 找重复性  重复性有最近重复项(分治,回溯),和最优重复项(动态规划);
不论分治,递归,回溯本质都是找重复性, 分解问题, 最后组合每个子问题的结果.
##### Python 递归代码模板
```python
def recursion(level, param1, param2, ...):
	#recursion terminator
	if level > MAX_LEVEL:
	process_result
	return
	
	# process logic in current level
	process(level, data..)
	
	# drill down
	self.recursion(level + 1, par1, ...)
	
	#reverse the current level status if needed
	
```

##### 分治 代码模板
```python
def divide_conquer(problem, param1, param2, ...):

# recursion terminator
if problem is None:
print_result
return

# prepare data
data = prepare_Data(problem)
subproblems = split_problem(problem, data)
处理当前层逻辑:
如果是求n的阶乘这里就是写成n * fac(n - 1),
如果是斐波那契写成n - 1的递归结果加上n - 2的递归结果,
如果是组合左右括号,组装左括号存在一个变量中,或者组装右括号存起来,当然要判断左右括号是否用完


# conquer subproblems
subresult1 = self.divide_conquer(subproblem[0], p1, ...)
subresult1 = self.divide_conquer(subproblem[1], p1, ...)
subresult1 = self.divide_conquer(subproblem[2], p1, ...)
调用函数下探到下一层解决更细节的子问题

# process and generate the final result
result = process_result(subresult1, subresult2, subresult3, ...)

#reverse the current level  states 
```
