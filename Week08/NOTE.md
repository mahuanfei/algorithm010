/****************位运算基础及实战要点****************/
[如何从十进制转换为二进制](https://zh.wikihow.com/%E4%BB%8E%E5%8D%81%E8%BF%9B%E5%88%B6%E8%BD%AC%E6%8D%A2%E4%B8%BA%E4%BA%8C%E8%BF%9B%E5%88%B6)
位运算符
左移 <<  譬如 0011 => 0110
右移 >> 譬如 0110 => 0011
这里作为演示用4个二进制位,在计算机中老式计算机是32位,新计算机基本都是64位
按位或								   | 
按位与 							   &
按位取反 							   ~
按位异或(相同为0不同为1) ^

> XOR-异或 :相同为0, 不同为1. 也可用"不进位加法"来理解
> 异或操作的特点:
> x ^ 0 = x  // x和0相同为0, 不同为1, 所以x ^ 0 就是x 本身, x异或全0 等于x
> x ^ 1s = ~x //  x异或全1   这里的1s指的全1, 全1就是0取反  注意 1s = ~0, x异或全1等于x取反
> x ^ (~x) = 1s // x异或x取反(所有二进制位0变1,1变0), 那么x与 ~x所有二进制位都不同,异或出来的结果为全1
> x ^ x = 0 // x异或x它们的所有二进制位都相同,结果为全0
>  c = a ^ b =>  a ^ c = a ^ a ^ b =>  a ^ c = b ,  => b ^ c = a // 交换两个数

指定位置的位运算
1. 将x最右边的n位清零: x & (~0 << n) 
 // 0取反就是全部为1之后向左移n位了之后可以想象变为11111后面跟n个0 跟x与之后是指将x右边n位全部清零
2. 获取x的第n位值(0或者1): (x >> n) & 1 
// 把x先右移n次,那么x第n位就变成最后一位,然后再& 1就得到了第n位到底是0还是1
3. 获取x的第n位的幂值:  x & (1<< n)
// 把1往左移,移到高位,然后和x与之后 就是第n位的幂值
4. 仅将第n位置为1: x | (1<< n)
5. 仅将第n位置为0: x & (~(1<<n))
6. 将x最高位至第n位(含)清零: x &((1<<n) -1)

实战位运算要点
判断奇偶数
  x&2== 1 ⇒ (x & 1) == 1
  x&2== 0 ⇒ (x & 1) == 0

x>>1 ⇒ x/2; //x右移一位,相等于把它的最后一位二进制位清空,整个数往右挪了一位,相等于除2
即x = x/2; ⇒ x = x>>1;
mid = (left + right) /2; ⇒ mid = (left + right) >>1;

X = X & (X-1) 清零最低位的1

X & -X ⇒ 得到最低位的1 // 注意这里不是~,而是取反

> 可知在计算机中数是以补码的形式储存的。
比如7，为111。-7为11111001。
其中求解-7的补码形式我们是怎么求解的呢？
负数原码转换为补码的方法之一：
符号位保持1不变，数值位按位求反，末位加1。
负数原码转换为补码的方法之二：
符号位保持1不变，在数值位中从低位向高位找1，第一个1及其右边的0保持不变，数值位的其余部分求反。
这两种方法大家试一试就会发现其实是一样的。
第一种方法是按照定义来设计的求解方式。
解释：一个正数的补码为原码自己，设为a，其负数形式的补码设为b，
可知a+b=0。方法一的求解方式就是先把这个数二进制中为0的哪些位给加上来，再加1，使得其进位，溢出为0.
如6=0000 0110，-6 = 1111 1001 + 1 = 1111 1010
可见最低位的1右边的0，在取反之后就会全部变为1，再加一，进位后，取反获取的1全部进位了，最低位的1又被进位上来了，即最低位的1保持不变。
方法二就是利用了这个性质，简化了求解过程中加1进位这一步.
于是我们可以利用补码正数和负数的这一点来快速求解一个数的最低位的1在哪里。
即n&-n
这样最低位的1被与运算保留下来，其他位全部置零。


X & ~X => 0


###### 位运算的实战题目

> 剑指 Offer 15. 二进制中1的个数
> 请实现一个函数，输入一个整数，输出该数二进制表示中 1 的个数。例如，把 9 表示成二进制是 1001，有 2 位是 1。因此，如果输入 9，则该函数输出 2。
示例 1：
输入：00000000000000000000000000001011
输出：3
解释：输入的二进制串 00000000000000000000000000001011 中，共有三位为 '1'。
示例 2：
输入：00000000000000000000000010000000
输出：1
解释：输入的二进制串 00000000000000000000000010000000 中，共有一位为 '1'。
示例 3：
输入：11111111111111111111111111111101
输出：31
解释：输入的二进制串 11111111111111111111111111111101 中，共有 31 位为 '1'。

首先假设是32位的在java中
方法一 循环用一个for loop,从0到32,循环32次,每次先看第0位,再看第1位,再看第2位,
一直看到31位是否为二进制 的1,如果是二进制的1就count加1,不然的话就不加1
方法二  %2, /2, 先模2就看它最低位到底是否是1,然后再除以2,相等于把它最后一位打掉,最后看这个数是否为0,为0结束
方法三 &1, x = x>>1;上面的办法都要循环次数32位
方法四:while (x > 0) x = x& (x-1)清零最低位的1判断x是否为0,为0退出,循环次数不再是32次,而是有多少个1就循环多少次


> 231. 2的幂
> 给定一个整数，编写一个函数来判断它是否是 2 的幂次方。
示例 1:
输入: 1
输出: true
解释: 2^0 = 1
示例 2:
输入: 16
输出: true
解释: 2^4 = 16
示例 3:
输入: 218
输出: false

思想:一句话总结是否是2的幂次,这个数它的二进制表示形式里面有且仅有一个二进制位是1
首先n必须大于0,如果n==0,就没什么说的
return  (n > 0) & (n & (n - 1)) == 0  或者 return  (n != 0) & (n & (n - 1)) == 0

> 231. 颠倒给定32位无符号整数的二进制位
> 
方法一: int ---> "010101" string -->reverse  --> int
因为把Int转为二进制且是String的形式来表示,然后在reverse再转换成int,巨慢无比,且不符合计算机高效运算法则
方法二:位运算,写for循环把每一位拿出来如果是第1位,就移动到第32位,第i位就移动到第32-i位置上

```javascript
var reverseBits = function(n) {
	let ans = 0
	for (let i = 0; i < 32; i++) {
		ans = (ans << 1) + (n & 1)
		n >>=1
	}
	return ans >>> 0
}
```

/****************布隆过滤器实现及应用****************/
##### 布隆过滤器 BloomFilter![在这里插入图片描述](https://img-blog.csdnimg.cn/20200728062327185.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)
上图为哈希表:哈希表如果有重复元素采用拉链存储法,本身任何一个字符串进来经过一个哈希函数映射到整数下标位置index,比如 Lisa Smith映射到001,就存在001这个位置,她的电话是521-8976, Johe Smith和Sandra Dee都被哈希到152的位置,采用的冲突办法就是在152位置开一个链表,将多个元素存在相同位置的链表处,对于哈希表不仅有哈希函数得到这么一个index值,且会把整个要存的元素,全部放在哈希表中,这是个没有误差的数据结构,有多少个元素,那么这些元素需要占的内存空间,在哈希表中都要找相应的内存大小给存进来,但是在很多时候,我们并不需要存元素本身,而只需要存一个信息就是判断下这个元素到底有没有在这个表中,如果只查询有没有,我们就需要一种更高效的数据结构
更高效的数据结构可以导致一个结果就是有很多元素存的话,但是表本身所需要的内存空间很少,同时不需要把元素本身String名称电话全部都
存下来,只需要说这个东西到底有还是没有, 
哈希表可以存很多信息,所以哈希表不只是能判断是否在集合中,同时还可以存元素本身和元素各种额外信息
而布隆过滤器只用来检索一个元素在还是不在集合中这样的信息
##### Bloom Filter vs Hash Table
一个很长的二进制向量和一系列随机映射函数.布隆过滤器可以用于检索一个元素是否在一个集合中
优点是空间效率和查询时间都远远超过一般的算法(模糊查询)
缺点是有一定的误识别率和删除困难
布隆过滤器示意图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200728075339143.png)
假设要存三个元素依次存进去,每一个元素会分配到一系列二进制位中,假设x就被分配到3个二进制位(蓝色线)中, 把x插入到布隆过滤器就是把x对应的三个位置置为1,同理依次存入y,z,这里这个二进制数组用来表示所有已经存入的xyz是否在索引中
这时候有个w中有1个0位说明,w未在索引中,如果一个元素所对应的二进制位,只要有1个为0就说明这个元素不在布隆过滤器的索引中且可以肯定不在,但是如果重新元素d对应都二进制位都为1,我们也不一定说元素一定在索引中
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200728080359758.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)

结论:当布隆过滤器把元素全部都插入完了之后,对于测试元素来验证它是否存在也就是它对应的二进制位都为1的时候,我们只能说可能
存在布隆过滤器里面
在布隆过滤器中查,如果不在,肯定不在,如果在,不一样存在
布隆过滤器只是挡在一台机器前面的快速查询的缓存,真正要确定测试元素一定存在的话,必须再访问这个机器
里面的一个完整的存储数据结构,一般来说就是数据库

##### 案例
1 比特币网络
(比特币是分布式系统,在分布式系统中,一个地址是否在这个节点里或者transaction是否在node中经常用到布隆过滤器进行快速查询)
2 分布式系统(Map-Reduce) Hadoop search engine之类的东西
比如搜索引擎做的事情就是把大量的网页信息,还有图片信息都存在整个服务器里面,一般来说
不同网页是存在不同的集群中,当search时,查到一个东西之后,就会根据索引,知道应该在那个集群
就先在集群布隆过滤器中查询看是否存在,如果存在就再去集群的DB里面进行访问,如果不存在直接过滤
所以在大型分布系统中,很多人使用布隆过滤器同时的话,Redis缓存以及垃圾邮件或者一个评论它到底是不是
涉黑 的不正当言论用布隆过滤器很快判断不是的话,肯定不是,是的话再去DB中查
[布隆过滤器实现原理](https://www.cnblogs.com/cpselvis/p/6265825.html)
[使用布隆过滤器解决缓存击穿、垃圾邮件识别、集合判重](https://blog.csdn.net/tianyaleixiaowu/article/details/74721877)
[布隆过滤器 Python 实现示例](https://www.geeksforgeeks.org/bloom-filters-introduction-and-python-implementation/)
[高性能布隆过滤器 Python 实现示例](https://github.com/jhgg/pybloof)
[布隆过滤器 Java 实现示例 1](https://github.com/lovasoa/bloomfilter/blob/master/src/main/java/BloomFilter.java)
[布隆过滤器 Java 实现示例 2](https://github.com/Baqend/Orestes-Bloomfilter)
```python
# Python  布隆过滤器
from bitarray import bitarray  #bitarray系统的数据结构就是一个数组,数组中都存的二进制位
import mmh3 
class BloomFilter: 
	def __init__(self, size, hash_num): 
		self.size = size  #总共有多少个元素,hash_num指一个元素进来分成多少个二进制位存储
		self.hash_num = hash_num 
		self.bit_array = bitarray(size) 
		self.bit_array.setall(0) #将二进制位数组索引全部都为0, 
	def add(self, s): 
		for seed in range(self.hash_num): 
		 # 循环hash_num次,假设hash_num为3就生成3个二进制位,每次将seed和元素s进行一次hash再模上bitarray的size,因为不能让下标超出去
			result = mmh3.hash(s, seed) % self.size 
			self.bit_array[result] = 1  #把相应的二进制位的索引处置为1
	def lookup(self, s): 
		for seed in range(self.hash_num): 
			result = mmh3.hash(s, seed) % self.size 
			if self.bit_array[result] == 0: #只要下标result为0就确定肯定不存在
				return "Nope" 
		return "Probably" # 全部都为1,输出可能
bf = BloomFilter(500000, 7) 
bf.add("dantezhao") 
print (bf.lookup("dantezhao")) 
print (bf.lookup("yyj")) 
```

```java
//Java
public class BloomFilter {
    private static final int DEFAULT_SIZE = 2 << 24;
    private static final int[] seeds = new int[] { 5, 7, 11, 13, 31, 37, 61 };
    private BitSet bits = new BitSet(DEFAULT_SIZE);
    private SimpleHash[] func = new SimpleHash[seeds.length];
    public BloomFilter() {
        for (int i = 0; i < seeds.length; i++) {
            func[i] = new SimpleHash(DEFAULT_SIZE, seeds[i]);
        }
    }
    public void add(String value) {
        for (SimpleHash f : func) {
            bits.set(f.hash(value), true);
        }
    }
    public boolean contains(String value) {
        if (value == null) {
            return false;
        }
        boolean ret = true;
        for (SimpleHash f : func) {
            ret = ret && bits.get(f.hash(value));
        }
        return ret;
    }
    // 内部类，simpleHash
    public static class SimpleHash {
        private int cap;
        private int seed;
        public SimpleHash(int cap, int seed) {
            this.cap = cap;
            this.seed = seed;
        }
        public int hash(String value) {
            int result = 0;
            int len = value.length();
            for (int i = 0; i < len; i++) {
                result = seed * result + value.charAt(i);
            }
            return (cap - 1) & result;
        }
    }
}
```
/****************LRU Cache的实现,应用和题解****************/
[CPU Socket中Cache的理解](https://www.sqlpassion.at/archive/2018/01/06/understanding-the-meltdown-exploit-in-my-own-simple-words/)
LRU Cache 最近最少使用的缓存放在淘汰的位置
两个要点: 大小,替换策略
实现: Hash Table + Double LinkedList
复杂度: O(1)查询, O(1)修改,更新
![LRUCache缓存机制图](https://img-blog.csdnimg.cn/20200504071610638.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)
#### JAVA实现
题目:
运用你所掌握的数据结构，设计和实现一个  LRU (最近最少使用) 缓存机制。它应该支持以下操作： 获取数据 get 和 写入数据 put 。
获取数据 get(key) - 如果密钥 (key) 存在于缓存中，则获取密钥的值（总是正数），否则返回 -1。
写入数据 put(key, value) - 如果密钥已经存在，则变更其数据值；如果密钥不存在，则插入该组「密钥/数据值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。
进阶:
你是否可以在 O(1) 时间复杂度内完成这两种操作？

>  /*思考过程
    要存储key,value我们能想到的就是哈希表,HashMap<Integer, Integer> map, 那么在put的时候发现如果map.size() ==capacity,就要淘汰最近最少的key,那么如何记录淘汰的key呢,比如用List<Integer> keys;List的keys就是用来存储最近比较频繁使用的key,现在假设访问了key,如果key是存在的,就将key插入到数组前面.
    * 如果List为ArrayList,那么get的时候如果key是存在的也就是这个key现在被访问了,就要把key这个放到前面去,
     但是数组先是要遍历查找key,然后删除,再插入到数组的第1个位置,这样就是O(n),同理如果List从ArrayList变为LinkedArray链表,但是这里也是虽然删除插入为O(N),但是在查找key的过程中要从头遍历到尾依然是O(n),所以List为ArrayList何LinkedList都不可以,所以我们要想get和put都是O(1),自己实现双向链表
     整体框架如下
     *
     * LRUCache
     *
     *                                      value为链表的节点
     * map          HASHMap{key: value}     ---------------> Node:{key, value, prev, next}   map中的value都是存储的Node且pre,next相互指向前面和后面的Node
     *                     {key: value}
     *                     {key: value}
     *                     {key: value}
     *
     * capacity
     * first  // 头结点
     * last   // 尾结点
> 
> 
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
/****************初级排序和高级排序****************/
###### 初级排序 - O(n^2)
1. 选择排序（Selection Sort）
每次找最小值，然后放到待排序数组的起始位置。

```javascript
function selectionSort(arr) {
    var len = arr.length;
    var minIndex, temp;
    for(var i = 0; i < len - 1; i++) {
        minIndex = i;
        for(var j = i + 1; j < len; j++) {
            if(arr[j] < arr[minIndex]) {     // 寻找最小的数
                minIndex = j;                 // 将最小数的索引保存
            }
        }
        temp = arr[i];
        arr[i] = arr[minIndex];
        arr[minIndex] = temp;
    }
    return arr;
} 
```

```swift
struct SelectionSort {
    
    func selectionSort(_ nums: [Int]) -> [Int] {
        var nums = nums
        _selectionSort(&nums)
        return nums
    }
    
    func _selectionSort(_ nums: inout [Int]) {
       
        for i in 0..<nums.count {
            var minIndex = i
            for j in i + 1..<nums.count {
                if nums[minIndex] > nums[j] {
                    minIndex = j
                }
            }
            if minIndex != i {
                (nums[minIndex], nums[i]) = (nums[i], nums[minIndex])
            }
        }
    }
}
```


2. 插入排序（Insertion Sort）
从前到后逐步构建有序序列；对于未排序数据，在已排序序列中从后
向前扫描，找到相应位置并插入。

```javascript
function insertionSort(arr) {
    var len = arr.length;
    var preIndex, current;
    for(var i = 1; i < len; i++) {
        preIndex = i - 1;
        current = arr[i];
        while(preIndex >= 0 && arr[preIndex] > current) {
            arr[preIndex + 1] = arr[preIndex];
            preIndex--;
        }
        arr[preIndex + 1] = current;
    }
    return arr;
}
```

```swift
class InsertionSort {
    func insertionSort(_ nums: [Int]) -> [Int] {
        var nums = nums
        self._insertionSort(&nums)
        return nums
    }
    
    func _insertionSort(_ nums: inout [Int]) {
        for i in stride(from: 1, to: nums.count, by: 1) {
            let temp = nums[i]
            var j = i - 1
            while j >= 0 && nums[j] > temp {
                nums[j + 1] = nums[j]
                j -= 1
            }
            nums[j + 1] = temp
            
        }
    }
    
    func _insertionSort2(_ nums: inout [Int]) {
       for i in stride(from: 1, to: nums.count, by: 1) {
                
           var j = i - 1
           while j > 0 && nums[j] < nums[j - 1] {
               (nums[j], nums[j - 1]) = (nums[j - 1], nums[j])
               j -= 1
           }
       }
   }
}
```

3. 冒泡排序（Bubble Sort）
嵌套循环，每次查看相邻的元素如果逆序，则交换。

```javascript
function bubbleSort(arr) {
    var len = arr.length;
    for(var i = 0; i < len - 1; i++) {
        for(varj = 0; j < len - 1 - i; j++) {
            if(arr[j] > arr[j+1]) {        // 相邻元素两两对比
                var temp = arr[j+1];        // 元素交换
                arr[j+1] = arr[j];
                arr[j] = temp;
            }
        }
    }
    return arr;
}
```

```swift
class BubbleSort {
    
    
        func bubbleSort(_ nums: [Int]) -> [Int] {
           var nums = nums
           self._bubbleSort2(&nums)
//           self._bubbleSort(&nums)
           return nums
       }
       
       func _bubbleSort(_ nums: inout [Int]) {
           for i in 0..<nums.count {
               var flag = false
            // 将最小的放在了最左边
            for j in stride(from: nums.count - 2, through: i, by: -1) {
                   if nums[j] > nums[j + 1] {
                       swap(&nums, i: j, j: j + 1)
                       flag = true
                   }
               }
               if flag == false {
                   break
               }
           }
       }
       
    // 利用异或交换
       func swap(_ nums: inout [Int], i: Int, j: Int) {
           if i == j {return}
           nums[i] ^= nums[j];
           nums[j] ^= nums[i];
           nums[i] ^= nums[j];
       }
    
    
       
    func _bubbleSort2(_ nums: inout [Int]) {
        for i in 0..<nums.count {
            var flag = false
            // 将最大的放到最右边
            for j in 0..<nums.count - i - 1 {
                if nums[j] > nums[j + 1] {
                    // 利用元祖交换
                    (nums[j], nums[j + 1]) = (nums[j + 1], nums[j])
                    flag = true
                }
            }
            if flag == false {
                break
            }
        }
    }
}

```

###### 高级排序 - O(N*LogN)
• 快速排序（Quick Sort）
数组取标杆 pivot，将小元素放 pivot左边，大元素放右侧，然后依次
对右边和右边的子数组继续快排；以达到整个序列有序

快排代码 - Java
```java
// 调用方式:quickSort(a, 0, a.length-1)

public static void quickSort(int[] array, int begin, int end) {
 if (end <= begin) return;
 int pivot = partition(array, begin, end);
 quickSort(array, begin, pivot - 1);
 quickSort(array, pivot + 1, end);
}
static int partition(int[] a, int begin, int end) {
 	// pivot: 标杆位置，counter: ⼩于pivot的元素的个数
 	int pivot = end, counter = begin;
 	for (int i = begin; i < end; i++) {
 		if (a[i] < a[pivot]) {
 			int temp = a[counter]; a[counter] = a[i]; a[i] = temp;
			 counter++;
		 }
 	}
 	int temp = a[pivot]; a[pivot] = a[counter]; a[counter] = temp;
 	return counter;
}
```

```cpp
//C/C++
int random_partition(vector<int>& nums, int l, intr) {
  int random_pivot_index = rand() % (r - l +1) + l;  //随机选择pivot
  int pivot = nums[random_pivot_index];
  swap(nums[random_pivot_index], nums[r]);
  int i = l - 1;
  for (int j=l; j<r; j++) {
    if (nums[j] < pivot) {
      i++;
      swap(nums[i], nums[j]);
    }
  }
  int pivot_index = i + 1;
  swap(nums[pivot_index], nums[r]);
  return pivot_index;
}
void random_quicksort(vector<int>& nums, int l, int r) {
  if (l < r) {
    int pivot_index = random_partition(nums, l, r);
    random_quicksort(nums, l, pivot_index-1);
    random_quicksort(nums, pivot_index+1, r);
  }
}
```

```python

def quick_sort(begin, end, nums):
    if begin >= end:
        return
    pivot_index = partition(begin, end, nums)
    quick_sort(begin, pivot_index-1, nums)
    quick_sort(pivot_index+1, end, nums)
    
def partition(begin, end, nums):
    pivot = nums[begin]
    mark = begin
    for i in range(begin+1, end+1):
        if nums[i] < pivot:
            mark +=1
            nums[mark], nums[i] = nums[i], nums[mark]
    nums[begin], nums[mark] = nums[mark], nums[begin]
    return mark
```

```javascript
// JavaScript
const quickSort = (nums, left, right) => {
  if (nums.length <= 1) return nums
  if (left < right) {
    index = partition(nums, left, right)
    quickSort(nums, left, index-1)
    quickSort(nums, index+1, right)
  }
}
      
const partition = (nums, left, right) => {
  let pivot = left, index = left + 1
  for (let i = index; i <= right; i++) {
    if (nums[i] < nums[pivot]) {
      [nums[i], nums[index]] = [nums[index], nums[i]]
      index++
    }
  }
  [nums[pivot], nums[index-1]] = [nums[index-1], nums[pivot]]
  return index -1
}
```

```swift
class QuickSort {
    
    
    func quickSort(_ arr: [Int]) -> [Int] {
        var result = arr
//        quickSort1(&result, 0, arr.count)

        quickSort(&result, 0, arr.count - 1)
        return result
    }
    
    
     private func quickSort(_ arr: inout [Int], _ start: Int, _ end: Int) {
        if start == end {
            return
        }
        let pivot = partitionByQuickSort(&arr, start, end)
        quickSort(&arr, start, pivot - 1)
        quickSort(&arr, pivot + 1, end)
    }
    
    private func partition1ByQuickSort(_ arr: inout [Int], _ start: Int, _ end: Int) -> Int {
        var start = start, end = end
        let temp = arr[start]
        let preStart = start
        // [3, 2, 1]  => [1, 2, 3]
        while start < end {
            while start < end && arr[end] > temp {
                end -= 1
            }
            while start < end && arr[start] < temp {
                start += 1
            }
            
            (arr[start], arr[end]) = (arr[end], arr[start])
        }
        (arr[preStart], arr[start]) = (arr[start], arr[preStart])
        return start
    }
    
    
    private func partitionByQuickSort(_ arr: inout [Int], _ start: Int, _ end: Int) -> Int {
        //[ 2, 1]
        let pivotElement = arr[end]
        var i = start
        for j in stride(from: start, to: end, by: 1) {
            if arr[j] <= pivotElement {
                (arr[i], arr[j]) = (arr[j], arr[i])
                i += 1
            }
        }
        (arr[i], arr[end]) = (arr[end], arr[i])
        return i
    }
    
    
    private func quickSort1(_ arr: inout [Int], _ start: Int, _ end: Int) {
        if  end - start == 1 {
            return
        }
        let mid = findPivotByQuickSort1(&arr, start, end)
        quickSort1(&arr, start, mid)
        quickSort1(&arr, mid + 1, end)
        
    }
    
    private func findPivotByQuickSort1(_ arr: inout [Int], _ start: Int, _ end: Int) -> Int {
        
        let pivotElement = arr[start]
        var begin = start
        var over = end
        while begin < over {
            
            while begin < over {
                if pivotElement < arr[over] {
                   over -= 1
                }else {
                    arr[begin] = arr[over]
                    begin += 1
                    break
                }
            }
            
            while begin < over  {
                
                if arr[begin] < pivotElement {
                    begin += 1
                }else {
                    arr[over] = arr[begin]
                    over -= 1
                    break
                }
            }
        }
        
        arr[begin] = pivotElement
        return begin
        
    }
}
```
归并排序（Merge Sort）— 分治
	 1. 把长度为n的输入序列分成两个长度为n/2的子序列；
	 2. 对这两个子序列分别采用归并排序；
	 3. 将两个排序好的子序列合并成一个最终的排序序列。 
归并排序代码 - Java
```java
public static void mergeSort(int[] array, int left, int right) {
 if (right <= left) return;
 int mid = (left + right) >> 1; // (left + right) / 2
 mergeSort(array, left, mid);
 mergeSort(array, mid + 1, right);
 merge(array, left, mid, right);
}

public static void merge(int[] arr, int left, int mid, int right) {
 	int[] temp = new int[right - left + 1]; // 中间数组
 	int i = left, j = mid + 1, k = 0;
 	while (i <= mid && j <= right) {
 		temp[k++] = arr[i] <= arr[j] ? arr[i++] : arr[j++];
 	}
 	while (i <= mid) temp[k++] = arr[i++];
 	while (j <= right) temp[k++] = arr[j++];
 	for (int p = 0; p < temp.length; p++) {
 		arr[left + p] = temp[p];
 	}
 // 也可以⽤ System.arraycopy(a, start1, b, start2, length)
 }
```


归并 和 快排 具有相似性，但步骤顺序相反

归并：先排序左右子数组，然后合并两个有序子数组
快排：先调配出左右子数组，然后对于左右子数组进行排序


堆排序（Heap Sort） — 堆插入 O(logN)，取最大/小值 O(1)
	 1. 数组元素依次建立小顶堆
      2. 依次取堆顶元素，并删除
 

```java

static void heapify(int[] array, int length, int i) {
    int left = 2 * i + 1, right = 2 * i + 2；// 下标从0开始,如果下标从1开始,则为left = 2i, right = 2i + 1
    int largest = i;
    if (left < length && array[left] > array[largest]) {
        largest = left;
    }
    if (right < length && array[right] > array[largest]) {
        largest = right;
    }
    if (largest != i) {
        int temp = array[i]; array[i] = array[largest]; array[largest] = temp;
        heapify(array, length, largest);
    }
}
public static void heapSort(int[] array) {
    if (array.length == 0) return;
    int length = array.length;
    for (int i = length / 2-1; i >= 0; i-) 
        heapify(array, length, i);   // 将数组变成堆, 
    for (int i = length - 1; i >= 0; i--) {  // 从最后一个元素向前遍历,把堆顶元素依次取出来挪到最后面
        int temp = array[0]; array[0] = array[i]; array[i] = temp;
        heapify(array, i, 0); // 从array中依次取出最小的元素出来,放在数组最前面
    }
}
```

```python
#Python

def heapify(parent_index, length, nums):
    temp = nums[parent_index]
    child_index = 2*parent_index+1
    while child_index < length:
        if child_index+1 < length and nums[child_index+1] > nums[child_index]:
            child_index = child_index+1
        if temp > nums[child_index]:
            break
        nums[parent_index] = nums[child_index]
        parent_index = child_index
        child_index = 2*parent_index + 1
    nums[parent_index] = temp


def heapsort(nums):
    for i in range((len(nums)-2)//2, -1, -1):
        heapify(i, len(nums), nums)
    for j in range(len(nums)-1, 0, -1):
        nums[j], nums[0] = nums[0], nums[j]
        heapify(0, j, nums)
```
