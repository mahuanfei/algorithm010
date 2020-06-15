
#### 算法


##### 移动零    下面四种方法 TC O(n), SC O(1)

```java

//方法一:   双指针
public void moveZeroes(int[] nums) {
        if (null == nums || nums.length <= 0) return 0;
  		int i = 0;
        for (int j = 0; j < nums.length; j++) {
            if (nums[j] != 0) {
                nums[i++] = nums[j];
            }
        }
        for (int j = i; j < nums.length; j++) {
            nums[j] = 0;
        }   
}
```

```java


// 方法二: 双指针 对上一个优化
public void moveZeroes(int[] nums) {
    if (null == nums || nums.length <= 0) return 0;  
		int i = 0;
        for (int j = 0; j < nums.length; j++) {

            if (nums[j] != 0) {	
	
 	  	        if (i != j) {        
		 	        nums[i] = nums[j];   
	         	    nums[j] = 0;     
                 }

                 i++;
	                
            }
        }
}

```

```java


// 方法三: 双指针
public void moveZeroes(int[] nums) {
        int i = 0;
        for (int j = 0; j < nums.length; j++) {
            if (nums[j] != 0) {
                //交互非0值为0, 0为非0
                this.swap(nums, i++, j);
            }
        }   
}
public void swap(int[] nums, int i, int j) {
        if (i == j) return;
        nums[i] = nums[i] ^ nums[j];
        nums[j] = nums[i] ^ nums[j];
        nums[i] = nums[i] ^ nums[j];
}
```

```java


// 方法四:覆盖 通过count值计算前面有多少个0,从而达到非0位置 思路:统计0的个数,如果count 大于0 ,就将非零元素移动到当前位置减去0元素个数的位置上,将当前位置用0填充
 public void moveZeroes(int[] nums) {
   
        int count = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == 0) {
                count++;
            }else if (count > 0) {
                nums[i - count] = nums[i];
                nums[i] = 0;
            }
        }
 }
```


##### 两数之和 

```java


// 方法一:暴力 TC O(n^2)  SC o(1)
 public int[] twoSum(int[] nums, int target) {
        int[] resultArray = new int[2];
        for (int i = 0; i < nums.length - 1; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] + nums[j] == target) {
                    resultArray[0] = i;
                    resultArray[1] = j;
                    return resultArray;
                }
            }
        }
        return resultArray;
}

```

```java


// 方法二: TC O(n), SC O(n)
public int[] twoSum(int[] nums, int target) {
        
        Map<Integer, Integer> map = new HashMap<>();
        int[] result = new int[2];
        for (int i = 0; i < nums.length; i++) {
            int comp = target - nums[i];
            if (map.containsKey(comp)) {
                result[0] = map.get(comp);null
                result[1] = i;
                return result;
            }
            map.put(nums[i], i);
        }
        return result;
}

```

##### 合并两个有序数组

```java

/*
须知java的arraycopy
int nums1 = [1, 2, 3, 0, 0, 0], m = 3;
int nums2 = [2, 5, 6], n = 3;
System.arraycopy(src, srcPos, dest, destPos, length)
System.arraycopy(nums2, 0, nums1, m, n)
nums1 = [1, 2, 3, 2, 5, 6];
*/
```

```java


// 方法一:暴力 TC O((n+m)log(n+m)) SC O(1)
public void merge(int[] nums1, int m, int[] nums2, int n) {
	System.arraycopy(nums2, 0, nums1, m, n);
	Arrays.sort(nums1);
}
```

```java


// 方法二: 双指针
public void merge(int[] nums1, int m, int[] nums2, int n) {
        
         int p1 = m - 1;
         int p2 = n - 1;
         int p = m + n - 1;
         while (p1 >= 0 && p2 >= 0) {
             nums1[p--] = nums1[p1] < nums2[p2] ? nums2[p2--] : nums1[p1--];
         }
         // 考虑p1排完后剩下p2时出现的问题   
         System.arraycopy(nums2, 0, nums1, 0, p2 + 1);
}
```



##### 合并两个有序链表
```java


// 方法一: CT O(M + N) ST O(1)
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode prehead = new ListNode(-1);
        ListNode prev = prehead;
        while (l1 != null && l2 != null) {
            if (l1.val <= l2.val) {
                prev.next = l1;
                l1 = l1.next;
            } else {
                prev.next = l2;
                l2 = l2.next;
            }
            prev = prev.next;
        }
        // 合并后 l1 和 l2 最多只有一个还未被合并完，我们直接将链表末尾指向未合并完的链表即可
        prev.next = l1 == null ? l2 : l1;
        return prehead.next;
}
```

```java


// 方法二: 递归 时间O(M+N) 空间O(M+N)递归深度
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null) {
            return l2;
        }
        else if (l2 == null) {
            return l1;
        }
        else if (l1.val < l2.val) {
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        }
        else {
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }

}
```


##### 删除排序数组中的重复项

```java
 /*
   [1, 1, 2] 
*/
 // 方法一: TC O(n) SC O(1)  总是和已去重数组的最后一个对比    — 慢指针位置
   public int removeDuplicates(int[] nums) {
        if (null == nums || nums.length <= 0) return 0;
        int i = 0;
        int j = 1;
        int length = nums.length;
        while(j < length) {
            if (nums[j] != nums[i]) {
                i++;
                nums[i] = nums[j];
            }
            j++;
        }
        return i + 1;
    }

```

```java
public int removeDuplicates(int[] nums) {
    if (nums.length == 0) return 0;
    int i = 0;
    for (int j = 1; j < nums.length; j++) {
        if (nums[j] != nums[i]) {
            i++;
            nums[i] = nums[j];      
        }    
    }
    return i + 1;
}
```

```java
// 方法二 : TC O(n) SC O(1)
 public int removeDuplicates(int[] nums) {
        if (null == nums || nums.length <= 0) return 0;
        int j = 0;
        int count = 1;
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] == nums[j]) {
                count++;
            }else {
                count = 1;
            }
            if (count <= 2) {
                j++;
                nums[j] = nums[i];
            }
        }
        return j + 1;
 }
```

```java
// 方法三: 重复项计数法:
/*
k 表示最多重复次数
 k = 1 该算法处理后 最多出现一次
 k = 2 该算法处理后 最多出现两次
 ...
 k = n 该算法处理后  最多出现n次

 当k = 1时 ，可作为本题的题解
k = 2
[1, 1, 1, 2, 2, 3]
和倒数第k个对比   可以实现k次重复  -- 慢指针 - k


*/
public int removeDuplicatesII(int[] nums) {
    int i = 0;
    int k = 1;
    for (int n : nums) {
        if (i < k || n > [i - k])
            nums[i++] = n;
   }
    return i;
}

```

##### 二叉树的最大深度
```java
public int maxDepth(TreeNode root) {
    // terminator
    if (null == root) {
        return -;
    }   
    
    // process current logic
    
    // drill down 递归计算左右子树最大高度
    int left_height = maxDepth(root.left);
    int right_height = maxDepth(root.right);

    // restore current status 得到并返回 当前子树的最大高度
    return Math.max(left_height, right_height) + 1;

}
```

##### 反转链表  两两交换链表中的节点 对比
###### 递归 TC O(n) SC O(n)
```java
public ListNode reverseList(ListNode head) {
    if (null == head || null == head.next) return head;
// 1 2 3 4   -> 1 2 4 3   node -> 4 -> 3 -> null
    ListNode node = reverseList(head.next);
    head.next.next = head;
    head.next = null;
    return node;
}

public ListNode swapPairs(ListNode head) {
    if (null == head || null == head.next) return head;
    // 1 2 3 4 -> 1 2 4 3
    ListNode firstNode = head;
    ListNode secondNode = head.next;
    
    firstNode.next = swapPairs(secondNode.next);
    secondNode.next = firstNode;
    return secondNode;

}
```
###### 迭代 TC O(n) SC O(1)
```java
public ListNode reverseList(ListNode head) {   
    if (null == head) return head;
    ListNode pre = null;
    while (head != null) {
        ListNode temp = head.next;
        head.next = pre;
        pre = head;
        head = temp;
    
    }
    return pre;
}

```
```java
public ListNode swapPairs(ListNode head) {
     if (null == head) return head;
     ListNode dummy = new ListNode(-1);
    dummy.next = head;
    ListNode pre = dummy;
    //-1 1 2 3 4  -> -1 2 1 3 4
    
    while (head != null && head.next.next != null) {
        ListNode first = head;
        ListNode second = head.next;

        pre.next = second;
        first.next = second.next;
        second.next = first;
        
        
        pre = head;
        head = head.next;
    }
    return dummy.next;
}


```
##### 链表是否有环
```java
public boolean hasCycle(ListNode head) {
    Set<ListNode> nodesSeen = new HashSet<>();
    while (head != null) {
        if (nodesSeen.contains(head)) {
            return true;
        } else {
            nodesSeen.add(head);
        }
        head = head.next;
    }
    return false;
}

```
```java
public boolean hasCycle(ListNode head) {
    if (head == null || head.next == null) {
        return false;
    }
    ListNode slow = head;
    ListNode fast = head.next;
    while (slow != fast) {
        if (fast == null || fast.next == null) {
            return false;
        }
        slow = slow.next;
        fast = fast.next.next;
    }
    return true;
}

```
##### k个一组翻转链表
```java
/*
给你这个链表：1->2->3->4->5

当 k = 2 时，应当返回: 2->1  ->4->3  ->5
作业 
###### TC 代表 Time Complexity  SC 代表 Space Complextiy
当 k = 3 时，应当返回: 3->2->1->4->5

*/

public ListNode reverseKGroup(ListNode head, int k) {
    ListNode dummy = new ListNode(-1);
    dummy.next = head;
    ListNode pre = dummy;
    ListNode end = dummy;
    
    while (end.next != null) {
        for (int i = 0; i < k && end != null; i++) {
            end = end.next;
        }
        if (end == null) break;
        ListNode next = end.next;
        ListNode start = pre.next;
        
        end.next = null;
        pre.next = reverse(start);

        start.next = next;
        pre = start;
        end = pre;
        
    }
    return dummy.next;
}

public ListNode reverse(ListNode head) {
        ListNode pre = null;
        ListNode  curr = head;
        while (curr != null) {
            ListNode temp = curr.next;
            curr.next = pre;
            pre = curr;
            curr = temp;
        
        } 
        return pre;  
}




```