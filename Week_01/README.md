学习笔记

环境Java8

1.Queue源码分析：继承于Collection，有6个方法
offer()用于插入元素，如果无法插入返回false
remove()用于移除元素，返回队列的头
poll()与remove()相似，唯一的区别是当队列为空，前者返回null，后者会抛出异常
element()和peek()都可以获取队列头元素（不会删除这个元素），区别是前者抛出异常，后者返回null

2.PriorityQueue源码分析：AbstractQueue的子类，继承于Serializable
默认容量是11，有多个构造函数，可以传入Collection，PriorityQueue，SortedSet，Comparator和自定义容量cap
方法：
(1)clear()初始化storage和used，Comparator()比较器，iterator()迭代器
(2)有peek(),poll(),remove()和offer()，功能和Queue的同名方法相似
(3)addAll()传入一个Collection，返回添加所有Collection元素的结果true 或者false，如果Collection没有元素会报空指针
(4)resize()开辟内存空间，将原来的数组拷贝过去，被findSlot()调用
(5)findSlot()计算当前队列长度，找到合适的长度添加元素，被其他增删改操作所调用
(6)bubbleUp(int index)冒泡排序
