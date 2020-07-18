学习笔记
1.HashMap
(1)继承了AbstractMap<K,V>，实现了接口Map<K,V>, Cloneable和Serializable
(2)HashMap与HashTable很相似，不过HashMap是非同步的，而且允许空值
(3)HashMap的实例有两个参数会影响他的性能，initial capacity和load factor，capacity是哈希表中buckets的数量 , 当哈希表的capacity自增时，load factor用来测量哈希表允许的容量

定义了以下方法：
void clear() 清除所有映射关系
Object clone() 返回实例的浅拷贝，键和值本身不被复制
boolean containsKey(Object key) 是否包含指定键的映射关系
boolean containsValue(Object value) 是否存在一个或多个键映射到指定值
Set entrySet() 返回映射关系的集合视图
Set keySet() 返回映射包含的键的集合视图
Object get(Object key) 
boolean isEmpty()
Object put(Object key, Object value)
putAll(Map m) 
Object remove(Object key) 
int size() 
Collection values() 返回映射包含的值的collection視圖
