
/****************字典树****************/
###### 字典树的数据结构

 1. 字典树,即Trie树,又称单词查找树或键树,是一种树型结构.典型应用是用于统计和排序大量的字符串(但不仅限于字符串),所以经常被搜索引擎系统用于文本词频统计
 2.  优点:最大限度地减少无谓字符串比较,查询效率比哈希表高
数据结构Trie(前缀树),用于检索字符串数据集中的键,这种数据结构有多种应用:
Trie用于检索字符串数据集中的键主要有1 自动补全单词 2 ,拼写检查

###### 字典树的基本性质
1. 结点本身不存完整单词
2. 从根节点到某一结点,路径上经过的字符连接起来,为该结点对应的字符串
3. 每个结点的所有子结点代表的字符都不相同
4. 结点存储额外信息. 下图中的数字表示单词的统计频次,然后给用户做相应的推荐(很多时候统计单词的频次,就会加一个数字,数字表示相应到这个结点所代表的单词,它的统计计数)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200721060832966.png)
![结点的内部结构](https://img-blog.csdnimg.cn/20200721062222771.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)

> Trie 树是一个有根的树，其结点具有以下字段：。
最多 R个指向子结点的链接，其中每个链接对应字母表数据集中的一个字母。
本文中假定 R 为 26，小写拉丁字母的数量。
布尔字段，以指定节点是对应键的结尾还是只是键前缀。
单词 "leet" 在 Trie 树中的表示如下图
疑问:在Trie树的基本实现第14分24秒中,老师说并查集中碰到一个长度为十几的单词最多查十多次, 比在整个排好序的字典里面查(就算排好序用二分查找)快很多, 这句话怎么理解呢,字典中查找不是O(1)么,不是用的哈希map么?
疑问解答:
还有其他的数据结构，如平衡树和哈希表，使我们能够在字符串数据集中搜索单词。为什么我们还需要 Trie 树呢？尽管哈希表可以在 O(1)O时间内寻找键值，却无法高效的完成以下操作
找到具有同一前缀的全部键值。
按词典序枚举字符串的数据集。
Trie 树优于哈希表的另一个理由是，随着哈希表大小增加，会出现大量的冲突，时间复杂度可能增加到 O(n)，其中 n 是插入的键的数量。与哈希表相比，Trie 树在存储多个具有相同前缀的键时可以使用较少的空间。此时 Trie 树只需要 O(m)的时间复杂度，其中 m 为键长。而在平衡树中查找键值需要O(mlogn) 时间复杂度。

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020072505254666.png)

###### 字典树的核心思想

 - Trie树的核心思想是空间换时间
 - 利用字符串的公共前缀来降低查询时间的开销已达到提高效率的目的
 - 只输入前缀,就会把所有的已这个为前缀的候选词,都给很方便的找出来
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200721061935924.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)
[208 实现Trie(前缀树)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/)

> 实现一个 Trie (前缀树)，包含 insert, search, 和 startsWith 这三个操作。
示例:
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // 返回 true
trie.search("app");     // 返回 false
trie.startsWith("app"); // 返回 true
trie.insert("app");   
trie.search("app");     // 返回 true
说明:
你可以假设所有的输入都是由小写字母 a-z 构成的。
保证所有输入均为非空字符串。

```java
class TrieNode {

    // R links to node children
    private TrieNode[] links;

    private final int R = 26;

    private boolean isEnd;

    public TrieNode() {
        links = new TrieNode[R];
    }

    public boolean containsKey(char ch) {
        return links[ch -'a'] != null;
    }
    public TrieNode get(char ch) {
        return links[ch -'a'];
    }
    public void put(char ch, TrieNode node) {
        links[ch -'a'] = node;
    }
    public void setEnd() {
        isEnd = true;
    }
    public boolean isEnd() {
        return isEnd;
    }
}
```
###### 向 Trie 树中插入键
我们通过搜索 Trie 树来插入一个键。我们从根开始搜索它对应于第一个键字符的链接。有两种情况：

链接存在。沿着链接移动到树的下一个子层。算法继续搜索下一个键字符。
链接不存在。创建一个新的节点，并将它与父节点的链接相连，该链接与当前的键字符相匹配。
重复以上步骤，直到到达键的最后一个字符，然后将当前节点标记为结束节点，算法完成。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200725053426725.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)

```java
class Trie {
    private TrieNode root;
    public Trie() {
        root = new TrieNode();
    }
    // Inserts a word into the trie.
    public void insert(String word) {
        TrieNode node = root;
        for (int i = 0; i < word.length(); i++) {
            char currentChar = word.charAt(i);
            if (!node.containsKey(currentChar)) {
                node.put(currentChar, new TrieNode());
            }
            node = node.get(currentChar);
        }
        node.setEnd();
    }
}
```
比如要插入ten这个单词,要做的就是把t放进来,告诉树说我有t这个字符,且t这个字符接下面一个节点,接到下面节点e时要把e初始化好连线,再接到下一个n接到把n初始化好连线,就变为ten这样的结构
上面代码所做的就是循环要插入的word,针对它每一个字符来进行考虑,进入循环,首先看当前的node是否存在字符,如果不存在就创建一个新TreeNode的给当前字符赋值,如果存在字符,就直接拿字符所对应的TreeNode进行赋值,每次node就往下走,直到结束
复杂度分析
时间复杂度：O(m)，其中 mm 为键长。在算法的每次迭代中，我们要么检查要么创建一个节点，直到到达键尾。只需要 m 次操作。
空间复杂度：O(m)。最坏的情况下，新插入的键和 Trie 树中已有的键没有公共前缀。此时需要添加 m 个结点，使用 O(m)空间。
###### 在 Trie 树中查找键
每个键在 trie 中表示为从根到内部节点或叶的路径。我们用第一个键字符从根开始，。检查当前节点中与键字符对应的链接。有两种情况：

存在链接。我们移动到该链接后面路径中的下一个节点，并继续搜索下一个键字符。
不存在链接。若已无键字符，且当前结点标记为 isEnd，则返回 true。否则有两种可能，均返回 false :
还有键字符剩余，但无法跟随 Trie 树的键路径，找不到键。
没有键字符剩余，但当前结点没有标记为 isEnd。也就是说，待查找键只是Trie树中另一个键的前缀。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200725055442670.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTI1ODE3NjA=,size_16,color_FFFFFF,t_70)

```java
class Trie {
    ...

    // search a prefix or whole key in trie and
    // returns the node where search ends
    private TrieNode searchPrefix(String word) {
        TrieNode node = root;
        for (int i = 0; i < word.length(); i++) {
           char curLetter = word.charAt(i);
           if (node.containsKey(curLetter)) {
               node = node.get(curLetter);
           } else {
               return null;
           }
        }
        return node;
    }

    // Returns if the word is in the trie.
    public boolean search(String word) {
       TrieNode node = searchPrefix(word);
       return node != null && node.isEnd();
    }
}

```

复杂度分析

时间复杂度 : O(m)。算法的每一步均搜索下一个键字符。最坏的情况下需要 m次操作。
空间复杂度 : O(1)。
###### 查找 Trie 树中的键前缀
该方法与在 Trie 树中搜索键时使用的方法非常相似。我们从根遍历 Trie 树，直到键前缀中没有字符，或者无法用当前的键字符继续 Trie 中的路径。与上面提到的“搜索键”算法唯一的区别是，到达键前缀的末尾时，总是返回 true。我们不需要考虑当前 Trie 节点是否用 “isend” 标记，因为我们搜索的是键的前缀，而不是整个键。

```java
class Trie {
    ...
    // Returns if there is any word in the trie
    // that starts with the given prefix.
    public boolean startsWith(String prefix) {
        TrieNode node = searchPrefix(prefix);
        return node != null;
    }
}
```



