学习笔记
一、布隆过滤器(Bloom Filter)
1.比较常见的例子: 如何判断一个元素是否存在一个集合中？
字处理软件中，需要检查一个英语单词是否拼写正确
在网络爬虫里，一个网址是否被访问过
yahoo, gmail等邮箱垃圾邮件过滤功能
比特币网络
分布式系统(Map-reduce) —— Hadoop
Redis缓存

2.介绍
一个很长的二进制向量 （位数组）
一系列随机函数 (哈希)
空间效率和查询效率高
有一定的误判率（哈希表是精确匹配）

3.布隆过滤器原理
核心实现是一个超大的位数组和几个哈希函数。假设位数组的长度为m，哈希函数的个数为k
假设集合里面有3个元素{x,y,z}，哈希函数的个数为3。
首先将位数组进行初始化，将每个位都设置位0。对于集合里面的每一个元素，将元素依次通过3个哈希函数进行映射，每次映射都会产生一个哈希值，这个值对应位数组上面的一个点，对应的位置标记为1。查询W元素是否存在集合中的时候，同样的方法将W通过哈希映射到位数组上的3个点。如果3个点的其中有一个点不为1，则可以判断该元素一定不存在集合中。反之，如果3个点都为1，则该元素可能存在集合中。

4.Python实现
from bitarray import bitarray 
import mmh3 
class BloomFilter: 
    def __init__(self, size, hash_num): 
        self.size = size 
        self.hash_num = hash_num 
        self.bit_array = bitarray(size) 
        self.bit_array.setall(0) 
    def add(self, s): 
        for seed in range(self.hash_num): 
            result = mmh3.hash(s, seed) % self.size 
            self.bit_array[result] = 1 
    def lookup(self, s): 
        for seed in range(self.hash_num): 
            result = mmh3.hash(s, seed) % self.size
            if self.bit_array[result] == 0: 
                return "Nope" 
            return "Probably" 
    bf = BloomFilter(500000, 7) 
    bf.add("dantezhao") 
    print (bf.lookup("dantezhao")) 
    print (bf.lookup("yyj")) 
    
5.Java实现
https://github.com/lovasoa/bloomfilter/blob/master/src/main/java/BloomFilter.java


二、LRU cache ( Least Recently Used )
1.两个要素：大小、替换策略
2.实现：哈希表+双向链表
3.查询、修改、更新：O(1)


三、排序
高级排序 O(N*LogN)
归并和快排具有相似性，但步骤顺序相反
归并：先排序左右子数组，然后合并两个有序子数组
快排：先调配处左右子数组，然后对左右子数组进行排列
