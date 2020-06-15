##### 两数之和    

##### 方法一:暴力 时间复杂度：O(n^2) 空间复杂度：O(1) (throw new IllegalArgumentException)
```java
public int[] twoSum(int[] nums, int target) {
	if (null == nums || nums.length < 2) throw new IllegalArgumentException("invalid argument");
	for (int i = 0; i < nums.length - 1; i++) {
		for (int j = i + 1; j < nums.length; j++) {
			if (nums[i] + nums[j] == target) {
				return new int[]{i, j};
			}
		}
	}
	 throw new IllegalArgumentException("no twoSum solution ");
}

```  
##### 方法二: 时间复杂度：O(n) 空间复杂度：O(n) HashMap (map.get(), map.put(), map.containsKey())
```java
public int[] twoSum(int[] nums, int target) {
	HashMap<Integer, Integer> map = new HashMap<>();
	for (int i = 0; i < nums.length; i++) {
		int num = target - nums[i];
		if (map.containsKey(num)) {
			return new int[]{map.get(num), i};
		}
		map.put(nums[i], i);
	}
	throw new IllegalArgumentException("no twoSum solution");
}

```   
##### 三数之和    
##### 方法一: 暴力法O(n^3)
```java
/*
为什么一定要给数组排序?
最初不太明白为何一定要先给数组排序的, 尝试完暴力解法后, 发现先给数组排好序是最简便的处理去重的方法
*/
/*
示例 nums:  [-1, -1, 0, 1, 2, -1, -4]
不得不一开始就给数组排序, 这样好去掉重复的三元组
不然就会出现这种情况: [ -1, 0, 1 ], [ 0, 1, -1 ],  还是得排序后去重
排好序后, 重复的元素一定在一起, 当和前边的数相等时, 说明这个数已经查找过了, 就不必查找了

*/
  public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
         if (null == nums || nums.length < 3) {return result;}
        Arrays.sort(nums);
      
       
        if (nums[0] > 0 || nums[nums.length - 1] < 0 )return result;
        for (int i = 0; i < nums.length - 2; i++) {
       /* 一开始排序后,没有经过下面的去除重复的处理,依然会有重复排序的
                经过排序后Arrays.sort(nums)之后仍然会有重复的
   				 [-1 0 1 - 1 0 1]
					[-1 -1 0 0 1 1]
		*/
        //去重，假如给的用例是[0, 0, 0, 0],当i第一次遇到0时，为首次遇见没有关系，可如果这次i的循环结束了
            //i+1进入第二次循环，那么这时候还是遇到0，为连续的第二次遇见，这就造成了前后重复，必须要跳过
            if (i > 0 && nums[i] == nums[i - 1])continue;
            for (int j = i + 1; j < nums.length - 1; j++) {
            //去重原理同i，j必须大于i + 1，因为j是从i + 1开始的，假如这时开始就后退一步，就去到i的地盘了
                if (j > i + 1 && nums[j] == nums[j - 1]) continue;
                for (int k = j + 1; k < nums.length; k++) {
                    if (k > j + 1 && nums[k] == nums[k - 1]) continue;
                    if (nums[i] + nums[j] + nums[k] == 0) {
                        List<Integer> list = new ArrayList<>();
                        list.add(nums[i]);
                        list.add(nums[j]);
                        list.add(nums[k]);
                        result.add(list);
                  
                    }

                }
            }
        }
        return result;
    }

```
##### 方法二: 双指针O(n^2)
```java
// 边界 1 nums 为null或nums.length < 3 return;
// 2 排序后nums[0] > 0 或者 nums[length - 1] < 0 return; 排序后最小的值肯定不大于0, 最大的值肯定 不小于0,出现这两种情况结果不可能相加为0
// 3 for循环中
// 最左侧的数大于 0 , 那么后边两个数也必然大于 0 , 三数之和必然大于 0,
//相等的跨过去,不然,最后结果会有重复的三元组
// sum > 0 while(j < k && nums[k] == nums[–k]);
// sum < 0 while( j < k && nums[j] == nums[++j]);
// sum == 0
//while (j < k && nums[j] == nums[++j]);
//while (j < k && nums[k] == nums[–k]);

public int[] threeSum(int[] nums, int target) {
	List<List<Integer>> list = new ArrayList<List<Integer>>();
        if (null == nums || nums.length < 3) {
        	return list;
        }
      
		Arrays.sort(nums);
	 if (nums[0] > 0 || nums[nums.length - 1] < 0) return list;
	for (int i = 0; i < nums.length; i++) {
		//最左侧的数大于 0 , 那么后边两个数也必然大于 0 , 三数之和必然大于 0, 
        //所以不符合条件
        if (nums[i] > 0) break;
        // 相等的跨过去,不然,最后结果会有重复的三元组
        if (i > 0 && nums[i] == nums[i - 1]) continue;
		// 双指针,不断缩小
		int j = i + 1;
		int k = nums.length - 1;
		while (j < k) {
			int sum = nums[i] + nums[j] + nums[k];
			if (sum > 0) {
			// 大于0, 就让k--到k 和--k不相等的下标
			   while(j < k && nums[k] == nums[--k]);
			}else if (sum < 0) {
				while(j < k && nums[j] == nums[++j]);
			}else if (sum == 0) {
				List<Integer> result = new ArrayList<>();
                        result.add(nums[i]);
                        result.add(nums[j]);
                        result.add(nums[k]);
                        list.add(result);
               while (j < k && nums[j] == nums[++j]);
               while (j < k && nums[k] == nums[--k]);
			}
		}
	}
	return list;
}

```