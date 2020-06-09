
#### 算法作业 
####### TC 代表 Time Complexity  SC 代表 Space Complextiy


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
                result[0] = map.get(comp);
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