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