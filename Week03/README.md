
#### 子集
方法一: 递归
```java
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        if (nums == null) {return ans;}
        dfs(ans, nums, new ArrayList<Integer>(), 0);
    }
    private void dfs(List<List<Integer>> ans, int[] nums, List<Integer> list, int index) {
        // teminator
        if (index == nums.length) {
            ans.add(new ArrayList<Integer>(list));
            return;
        }
        // not pick the number at this index,list不需要发生改变
        dfs(ans, nums, list, index + 1); 
        list.add(nums[index]);
        // pick the number at this index list发生改变了
        dfs(ans, nums, list, index + 1);

        // reverse the current state 
        /*这里为什么要reverse,因为我们在上面add给了list的一个数,改变了list参数,这个地方随着递归一直在变,它不是本层的本地变量,不是本层的局部变量Local variable(每一层是隔开的),但这个参数改变了会影响上面几层的函数,所以这里就把刚才添加到List中的数去掉,达到一致的.也可以不用reverse the current state 但是要把结果的每一层都复制出来通过这种办法每次改变当前层list不会影响到上下面一层,因为是每层拷贝一份出来的,这样的话,因为List为容器相等于把里面的每一个元素的reference都浅拷贝即指针,引用拷贝了一次类似于下面伪代码
        */
        list.remove(list.size() - 1);
    }
```


方法二:迭代思想:
一开始是空数组,之后都是往之前已经产生过的子集合里面加,
[[]]  遍历所有nums 后  [[], [1], [2], [1, 2], [3], [1, 3], [2, 3], [1, 2, 3]]
```java
 public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        backtrack(0, nums, res, new ArrayList<Integer>());
        return res;

    }

    private void backtrack(int i, int[] nums, List<List<Integer>> res, ArrayList<Integer> tmp) {
        res.add(new ArrayList<>(tmp));
        for (int j = i; j < nums.length; j++) {
            tmp.add(nums[j]);
            backtrack(j + 1, nums, res, tmp);
            tmp.remove(tmp.size() - 1);
        }
    }
```
#### 括号生成
```java
public List<String> generateParenthese(int n) {
// 2n个格子
	_generate(0, 0, n, “”);
}

List<String> result = new ArrayList<String>();
private void _generate(int left, int right, int n, String s) {
	 // terminator
	if (left == n && right == n) {
		System.out.println(s);
		result.add(s);
		return;
	}
	// process current logic: left, right
	if left < n {
		String s1 = s + “(”;
	}
	
	if (left > right && right < n) {
		String s2 = s + “)”;
	}
	
	
	// drill down
	_generate(left + 1, right, n,  s1);
	_generate(left, right + 1, n,  s2);

	// reverse states  不需要因为都是局部变量, 本地变量,自己清除
	

}

// left:随时可以加,只要别超标
// right: 必须之前有左括号,且左个数 > 右个数


```
#### 验证二叉树
 方法一: 递归
```java
public boolean isValidBST(TreeNode root) {
	return helper(root, null, null);
}

public boolean helper(TreeNode node, Integer lower, Integer upper) {
	// terminator
	if (node == null) return true;
	// process current logic 
	// 如果当前值越界,则当前方法栈返回false, 且不用继续深搜子树
	int value = node.val;
	if (lower != null && value <= lower) return false;
	if (upper != null && value >= upper) return false;

	// drill down
	if (!helper(node.right, value, upper)) return false;
	if (!helper(node.left, lower, value)) return false;
	
	// restore current status
	// 无
	return true;; // 当前层有效,并且左右子树有效
}

```
方法二:迭代
```java
public boolean isValidBST(TreeNode root) {
	if (null == root) return true;
	Integer inorder = null;
	Stack<TreeNode> stack = new Stack<>();
	TreeNode curr = root;
	while (!stack.isEmpty() || curr != null) {
	 // 迭代左子树入栈
		while (curr != null) {
			stack.push(curr);
			curr = curr.left;
		}
		curr = stack.pop();
		if (inorder != null && inorder >= curr.val) return false;
		inorder = curr.val;
		// 迭代右子树入栈
		curr = curr.right;
	}
	return true;
}

```

#### Pow(x, n)
```java
public double myPow(double x, int n) {
    if (n < 0) return 1.0 / myPow(x, -n);
    double result = 1.0;
    for (int i = n; i != 0; i /= 2) {
        if (n % 2 == 1) {
            res *= x;
        }
        x *= x;
    }
    return res;
    

}
```

方法一: 时间 O(logN)
```java
  public double myPow(double x, int n) {
        long N = (long)n;
        if (N < 0) {
            x = 1/x;
            N = -N;
        }
        double res = 1.0;
        while (N > 0) {
            if ((N & 1) == 1) {
                res *= x;
            }
            x *= x;
            N >>= 1;
        }
        return res;
    }
```
方法二 递归 :时间 O(logN)
```java
double myPow(double x, int n) {
    long long N = n;
    if (N < 0) {
       x = 1 / x;
       N = -N;
    }
    return fastPow(x, N);
}

double fastPow(double x, long long n){
    if (n == 0) return 1.0;
    double half = fastPow(x, n / 2);
    if (n % 2 == 0) {
         return half * half; 
    }else {
        return half * half * x;
    }
}

```
#### 两数相加
```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummyHead = new ListNode(0);
    ListNode p = l1, q = l2, curr = dummyHead;
    int carry = 0;
    while (p != null || q != null) {
        int x = (p != null) ? p.val : 0;
        int y = (q != null) ? q.val : 0;
        int sum = carry + x + y;
        carry = sum / 10;
        curr.next = new ListNode(sum % 10);
        curr = curr.next;
        if (p != null) p = p.next;
        if (q != null) q = q.next;
    }
    if (carry > 0) {
        curr.next = new ListNode(carry);
    }
    return dummyHead.next;
}
```
#### 二叉树的最近公共祖先
##### 方法一：递归时间复杂度：O(N)，其中 N 是二叉树的节点数。二叉树的所有节点有且只会被访问一次，因此时间复杂度为 O(N)。 空间复杂度：O(N) ，其中 NN 是二叉树的节点数。递归调用的栈深度取决于二叉树的高度，二叉树最坏情况下为一条链，此时高度为 N，因此空间复杂度为 O(N)。
        
```java
/*
给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。
百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”
例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]
示例 1: 输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出: 3     解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
示例 2: 输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出: 5
解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。
说明:
所有节点的值都是唯一的。
p、q 为不同节点且均存在于给定的二叉树中。
*/
  TreeNode ans = null;
 public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
  	dfs(root, p, q);
  	return ans;      
 }
 private boolean dfs(TreeNode root, TreeNode p, TreeNode q) {
  	if (root == null) return false;
  	/*
  	其中lson 和 rson 分别代表 x 节点的左孩子和右孩子。
  	说明左子树和右子树均包含 p 节点或 q 节点，如果左子树包含的是 p 节点，那么右子树只能包含 q 节点，反之亦然，因为 p 节点和 q 节点都是不同且唯一的节点，因此如果满足这个判断条件即可说明 x 就是我们要找的最近公共祖先。再来看第二条判断条件，这个判断条件即是考虑了 x 恰好是 p 节点或 q 节点且它的左子树或右子树有一个包含了另一个节点的情况，因此如果满足这个判断条件亦可说明 x 就是我们要找的最近公共祖先
  	*/
  	boolean lson = dfs(root.left, p, q);
  	boolean rson = dfs(root.right, p, q);
  	if ((lson && rson) || ((root.val == p.val || root.val == q.val) && (lson || rson))) {
  		 ans = root;
  	}
  	return lson || rson || (root.val == p.val || root.val == q.val);
 }
```
###### 方法二：存储父节点 复杂度分析 时间复杂度：O(N)，其中 NN 是二叉树的节点数。二叉树的所有节点有且只会被访问一次，从 p 和 q 节点往上跳经过的祖先节点个数不会超过 N，因此总的时间复杂度为 O(N)。空间复杂度：O(N)，其中 N是二叉树的节点数。递归调用的栈深度取决于二叉树的高度，二叉树最坏情况下为一条链，此时高度为 N，因此空间复杂度为 O(N)，哈希表存储每个节点的父节点也需要 O(N) 的空间复杂度，因此最后总的空间复杂度为 O(N)。
```java
 Map<Integer, TreeNode> parent = new HashMap<Integer, TreeNode>();
 Set<Integer> visited = new HashSet<Integer>();

    public void dfs(TreeNode root) {
        if (root.left != null) {
            parent.put(root.left.val, root);
            dfs(root.left);
        }
        if (root.right != null) {
            parent.put(root.right.val, root);
            dfs(root.right);
        }
    }
  public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        dfs(root);
        while (p != null) {
            visited.add(p.val);
            p = parent.get(p.val);
        }
        while (q != null) {
            if (visited.contains(q.val)) {
                return q;
            }
            q = parent.get(q.val);
        }
        return null;
    }
```

#### 全排序
###### 递归回溯
```java
public List<List<Integer>> permute(int[] nums) {
	int len = nums.length;
	List<List<Integer>> res = new ArrayList<>();
	if (len == 0) {
		return res;
	}
	Deque<Integer> stack = new ArrayDeque<Integer>();
	boolean[] used = new boolean[len]; // 默认初始化为false
	dfs(nums, len, 0, stack, used, res);
	return res;
}
public void dfs(int[] nums, int len, int depth, Deque<Integer> stack, boolean[] used, List<List<Integer>> res) {
	if (len == depth) {
		res.add(new ArrayList<>(stack));
		return;
	}
	for (int i = 0; i < len; i++) {
		if (used[i]) {
			continue;
		}
		stack.addLast(nums[i]);
		used[i] = true;
		dfs(nums, len, depth + 1, stack, used, res);
		 // 注意：这里是状态重置，是从深层结点回到浅层结点的过程，代码在形式上和递归之前是对称的
		stack.removeLast();
		used[i] = false;
	}

}

```
###### 递归不回溯
```java



public List<List<Integer>> permute(int[] nums) {
	int len = nums.length;
	List<List<Integer>> res = new ArrayList<>();
	if (len == 0) {
		return res;
	}
	List<Integer> stack = new ArrayList<Integer>();
	boolean[] used = new boolean[len]; // 默认初始化为false
	dfs(nums, len, 0, stack, used, res);
	return res;
 }
public void dfs(int[] nums, int len, int depth, List<Integer> stack, boolean[] used, List<List<Integer>> res) {
	if (len == depth) {
	   // 3、不用拷贝，因为每一层传递下来的 path 变量都是新建的
		res.add(stack);
		return;
	}
	for (int i = 0; i < len; i++) {
		if (used[i]) {
			continue;
		}
		 // 1、每一次尝试都创建新的变量表示当前的"状态"
		List<Integer> newStack = new ArrayList<>(stack);
		newStack.add(nums[i]);
		 boolean[] newUsed = new boolean[len];
         System.arraycopy(used, 0, newUsed, 0, len);
		newUsed[i] = true;
		dfs(nums, len, depth + 1, newStack, newUsed, res);
		// 2、无需回溯	
	}

}
```