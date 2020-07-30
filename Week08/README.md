
[146. LRU缓存机制](https://leetcode-cn.com/problems/lru-cache/#/)
> 运用你所掌握的数据结构，设计和实现一个  LRU (最近最少使用) 缓存机制。它应该支持以下操作： 获取数据 get 和 写入数据 put 。
获取数据 get(key) - 如果关键字 (key) 存在于缓存中，则获取关键字的值（总是正数），否则返回 -1。
写入数据 put(key, value) - 如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字/值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。
进阶:
你是否可以在 O(1) 时间复杂度内完成这两种操作？
#### JAVA实现
```java
import java.util.HashMap;

public class LRUCache {
    HashMap<Integer, Node> map;
    private int capacity;
    // 虚拟头尾结点
    private Node first;
    private Node last;

    public LRUCache(int capacity){
        map = new HashMap<>(capacity);
        first = new Node();// Node内部创建一个空的初始化方法
        last = new Node();
        first.next = last;
        last.pre = first;
        this.capacity = capacity;
    }

    public int get(int key) {
         Node node = map.get(key);
        
         if(node != null) {
             removeNode(node);
             addAfterFirst(node);
         }
        return  (node != null) ? node.value : -1;
    }

    private void removeNode(Node node) {
        node.pre.next = node.next;
        node.next.pre = node.pre;
    }

    private void addAfterFirst(Node node) {
        // node 与first.next 关系
        node.next = first.next;
        first.next.pre = node;

        // first 与 node 关系
        first.next = node;
        node.pre = first;

    }
    
    public  void put(int key, int value) {
        Node node = map.get(key);
        if (node != null) { // 新值覆盖旧值
            node.value = value;
            removeNode(node);
            addAfterFirst(node);
        }else {// 添加一对新的key value
            if (map.size() == capacity) {
                // 两件事情,第一件把key从map上删除, 第二件事情节点从双向链表中删掉
                map.remove(last.pre.key);
                removeNode(last.pre);
            }

            Node newNode = new  Node(key, value);
            map.put(key, newNode);
            addAfterFirst(newNode);
        }
    }

     private static class Node {
        public int key;
        public int value;
        public Node pre;
        public Node next;

        public Node(int key, int value) {
            this.key = key;
            this.value = value;
        }
        public Node(){} // 保留空的构造方法给first和next使用
    }
}
```

[56 合并区间](https://leetcode-cn.com/problems/merge-intervals/)
> 给出一个区间的集合，请合并所有重叠的区间。
示例 1:
输入: [[1,3],[2,6],[8,10],[15,18]]
输出: [[1,6],[8,10],[15,18]]
解释: 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
示例 2:
输入: [[1,4],[4,5]]
输出: [[1,5]]
解释: 区间 [1,4] 和 [4,5] 可被视为重叠区间。

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        if not intervals: return []
	n = len(intervals)
	intervals.sort()
	res =[]
	left = intervals[0][0]
	right = intervals[0][1]
	for i in range(1, n):
		if(intervals[i][0] <= right):
			if(intervals[i][1] > right):
				right=intervals[i][1]
		else:
			res.append([left, right])
			left = intervals[i][0]
			right = intervals[i][1]
      res.append([left, right])
      return res
```

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        if not intervals: return []
	n = len(intervals)
	intervals = sorted(intervals)
	res =[]
	i=0
	while(i<n):
		left = intervals[i][0]
		right = intervals[i][1]
		while(i<n-1 and intervals[i+1][0]<=right):
			i+=1
			right=max(intervals[i][1],right)
		res.append([left, right])
		i+=1
      return res
```

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        if not intervals: return []
        intervals.sort()
        res = [intervals[0]]
        for x, y in intervals[1:]:
            if res[-1][1] < x:
                res.append([x, y])
            else:
                res[-1][1] = max(y, res[-1][1])
        return res
```
