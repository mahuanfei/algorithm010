/****************深度优先搜索和广度优先搜索总结****************/
###### 思维
> 一定要在脑子里面多记
> 1 先把细节全部都记下来
> 2 不断的反复
> 3 反复完之后,化繁为简,浓缩成就一点



 动态规划的本质就是将一个复杂的问题,分解成重复性子问题.
  分治,回溯,递归,动态规划都差不多,只是一些小的细节问题
 分治 Divide & Conquer ;它是递归的一种特殊形式,因为复杂的问题,都有很多重复性子问题,分别运算,再返回结果

 

```java
//  递归模板
public void recur(int level, int param) {
// terminator
 if (level > MAX_VALUE) {
 	// process result
 	return
 }
// handle curr level loginc
process(level, param)
// drill down
recur(level: level + 1, newParam)
// restore current status
// 如果改变的状态都是在参数上面的话,因为递归调用这个参数会被复制的,就要恢复
}
```

```python
// 分治模板
def divide_conquer(problem, param1, param2,...)
# recursion terminator
if problem is None:
	print_result
	return
# prepare data
data = prepare_data(problem)
subproblem = split_problem(problem, data)
#conquer subproblem
subresult1 = self.divide_conquer(subproblem[0], p1, p2..)
subresult2 = self.divide_conquer(subproblem[1], p1, p2..)

#process and generate the final result
result = process_result(subresult1, subresult2,...)
#revert the current level states 当前层的状态需要进行恢复
```

> 本质:寻找重复性
> 动态规划Dynamic Programming:wiki中DP的定义是需要进行分治,DP和分治有内在联系,和分治的区别(动态规划问题是让求最优解,最大值,最少的方式, 所以在中间部分不需要保存所有的状态,只需要保存最优的状态,还要证明,如果每一步存最优的值,最后可以推导出全局最优的值, 所以引入了两个,一个是有所谓的缓存,或者是状态的存储数组,第二个就是每一步的话都会把次优的状态淘汰掉,只保留在这一步里面最优或者较优的一些状态推导出全局最优)
> Simplifying a complicated problem by break it down into simpler subproblem(in a recursive manner)
> Divide & Conquer + Optimal substructure 分治 + 最优子结构
>  动态规划DP和递归或者分治,没有根本的区别,(关键看有无最优子结构,如果没有最优子结构,说明所有的问题都需要计算一遍,同时把结果合并在一起,传统意义上就称之为分治)
>  共性:找重复子问题
>  差异性:最优子结构,中途可以淘汰次优解,当然也必须淘汰次优解
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200712143456590.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200712143509533.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200712143655402.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)

##### 在计算机竞赛的时候,只要写递归,所有竞赛型选手全部都是直接开始for循环,全部是自底向上的写循环,开始递推
这也是为什么DP很多时候我们把它翻译为动态递推就是这个原因

UniquePath
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200712144907816.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)
动态规划关键点
1.最优子结构opt[n] = best_of(opt[n - 1], opt[n -2])
2 存储中间状态:opt[i]
3递推公式(美其名曰:状态转移方程或者DP方程)
Fib:opt[i] = opt[n-1] +opt[n-2]
二维路径:opt[i, j] = opt[i + 1][j] + opt[i][j+1] (且判断a[i, j]是否空地)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200712150422597.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200712150428131.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)
字符串问题
[1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)
给定两个字符串text1和text2,返回这两个字符串的最长公共子序列
"ABAZDC", "BACBAD
 
> 动态规划小结
> 在脑子里面多记, 同时先把它的细节全部都记下来,再不断反复,反复之后,化繁为简,浓缩成一点
> 1 打破自己的思维惯性, 形成机器思维(找重复性,机器只会if else,循环,递归,机器就很傻,算法思维),
>  2 理解复杂逻辑的关键,
>  3  不要人肉递归,不要亲力亲为,而是哟啊放权,授权,信任下属
>  B站搜索:MIT algorithm course 
重点:
easy steps to DP:
1 define subproblems
2 guess(path of solution)
3 related subproblem solutions
4 recurse & memoize OR  build DP table bottom-up
5 solve original problem