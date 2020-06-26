
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

