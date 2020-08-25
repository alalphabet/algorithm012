学习笔记
一.Trie树（前缀树）
1.发音为"try"，用于检索字符串数据集中的键，应用于：自动补全、拼写检查、IP路由（最长前缀匹配）、T9（九宫格）打字预测、单词游戏

2.结点结构字段：
最多 R 个指向子结点的链接，其中每个链接对应字母表数据集中的一个字母。可假定 R 为 26，小写拉丁字母的数量。
布尔字段，以指定节点是对应键的结尾还是只是键前缀。

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

3.插入键
从根开始搜索它对应于第一个键字符的链接。有两种情况：
（1）链接存在。沿着链接移动到树的下一个子层。算法继续搜索下一个键字符。
（2）链接不存在。创建一个新的节点，并将它与父节点的链接相连，该链接与当前的键字符相匹配。
重复以上步骤，直到到达键的最后一个字符，然后将当前节点标记为结束节点，算法完成。
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

4.查找键
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


5.python实现：
class Trie(object):
 
   def __init__(self): 
       self.root = {} 
       self.end_of_word = "#" 

   def insert(self, word): 
       node = self.root 
       for char in word: 
           node = node.setdefault(char, {}) 
       node[self.end_of_word] = self.end_of_word 

   def search(self, word): 
       node = self.root 
       for char in word: 
           if char not in node: 
               return False 
           node = node[char] 
       return self.end_of_word in node 

   def startsWith(self, prefix): 
       node = self.root 
       for char in prefix: 
           if char not in node: 
               return False 
           node = node[char] 
       return True


6.C++实现
class Trie {
    struct TrieNode {
        map<char, TrieNode*>child_table;
        int end;
        TrieNode(): end(0) {}
    };
        
public:
    /** Initialize your data structure here. */
    Trie() {
        root = new TrieNode();
    }
    /** Inserts a word into the trie. */
    void insert(string word) {
        TrieNode *curr = root;
        for (int i = 0; i < word.size(); i++) {
            if (curr->child_table.count(word[i]) == 0)
                curr->child_table[word[i]] = new TrieNode();
               curr = curr->child_table[word[i]];                
        }
        curr->end = 1;
    }
    /** Returns if the word is in the trie. */
    bool search(string word) {
        return find(word, 1);
    }
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string prefix) {
        return find(prefix, 0);
    }
private:
    TrieNode* root;
    bool find(string s, int exact_match) {
        TrieNode *curr = root;
        for (int i = 0; i < s.size(); i++) {
            if (curr->child_table.count(s[i]) == 0)
                return false;
            else
                curr = curr->child_table[s[i]];
        }
        if (exact_match)
            return (curr->end) ? true : false;
        else
            return true;
    }
};

7.JavaScript
class Trie {
  constructor() {
    this.root = {};
    this.endOfWord = "$";
  }

  insert(word) {
    let node = this.root;
    for (let ch of word) {
      node[ch] = node[ch] || {};
      node = node[ch];
    }
    node[this.endOfWord] = this.endOfWord;
  }

  search(word) {
    let node = this.root;
    for (let ch of word) {
      if (!node[ch]) return false;
      node = node[ch];
    }
    return node[this.endOfWord] === this.endOfWord;
  }

  startsWith(word) {
    let node = this.root;
    for (let ch of word) {
      if (!node[ch]) return false;
      node = node[ch];
    }
    return true;
  }
}


let trie = new Trie();
console.log(trie.insert("apple"));
console.log(trie.search("apple")); // 返回 true
console.log(trie.search("app")); // 返回 false
console.log(trie.startsWith("app")); // 返回 true
console.log(trie.insert("app"));
console.log(trie.search("app")); // 返回 true



二、单词搜索
给定一个二维网格 board 和一个字典中的单词列表 words，找出所有同时在二维网格和字典中出现的单词。
单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母在一个单词中不允许被重复使用。
示例:
输入: 
words = ["oath","pea","eat","rain"] and board =
[
  ['o','a','a','n'],
  ['e','t','a','e'],
  ['i','h','k','r'],
  ['i','f','l','v']
]

输出: ["eat","oath"]
说明:
你可以假设所有输入都由小写字母 a-z 组成。

提示:
你需要优化回溯算法以通过更大数据量的测试。你能否早点停止回溯？
如果当前单词不存在于所有单词的前缀中，则可以立即停止回溯。什么样的数据结构可以有效地执行这样的操作？散列表是否可行？为什么？ 前缀树如何？如果你想学习如何实现一个基本的前缀树，请先查看这个问题： 实现Trie（前缀树）。
来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/word-search-ii
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

复杂度分析

时间复杂度:O(M(4⋅3^(L−1)))，其中M 是二维网格中的单元格数，L是单词的最大长度。
分析：计算回溯算法将执行的确切步数是一个棘手的问题。我们为这个问题的最坏情况提供了该步骤的上限。该算法循环遍历二维网格中的所有单元，因此在复杂度公式中我们有 MM 作为因子。然后将其归结为每个启动单元所需的最大步骤数即 4⋅3^(L−1))
假设单词的最大长度是 L，从一个单元格开始，最初我们最多可以探索 4个方向。假设每个方向都是有效的（即最坏情况），在接下来的探索中，我们最多有 3个相邻的单元（不包括我们来的单元）要探索。因此，在回溯探索期间，我们最多遍历 4⋅3^(
L−1)个单元格。
你可能会想最坏的情况是什么样子。这里有一个例子。想象一下，二维网格中的每个单元都包含字母 a，单词词典包含一个单词 ['aaaa']。这是算法将遇到的最坏的情况之一。
注意，上述时间复杂性是在 Trie 数据结构一旦构建就不会改变的假设下估计的。如果采用优化策略逐步删除 Trie 中的节点，则可以大大提高时间复杂度，因为一旦匹配词典中的所有单词，即 Trie 变为空，回溯的成本就会降低到零。

空间复杂度：O(N)，其中 N 是字典中的字母总数。
算法消耗的主要空间是我们构建的 Trie 数据结构。在最坏的情况下，如果单词之间没有前缀重叠，则 Trie 将拥有与所有单词的字母一样多的节点。也可以选择在 Trie 中保留单词的副本。因此，我们可能需要 2N 的空间用于 Trie。


三、并查集代码模板
// Java
class UnionFind { 
    private int count = 0; 
    private int[] parent; 
    public UnionFind(int n) { 
        count = n; 
        parent = new int[n]; 
        for (int i = 0; i < n; i++) { 
            parent[i] = i;
        }
    } 
    public int find(int p) { 
        while (p != parent[p]) { 
            parent[p] = parent[parent[p]]; 
            p = parent[p]; 
        }
        return p; 
    }
    public void union(int p, int q) { 
        int rootP = find(p); 
        int rootQ = find(q); 
        if (rootP == rootQ) return; 
        parent[rootP] = rootQ; 
        count--;
    }
}

//Python 
def init(p): 
    # for i = 0 .. n: p[i] = i; 
    p = [i for i in range(n)] 
 
def union(self, p, i, j): 
    p1 = self.parent(p, i) 
    p2 = self.parent(p, j) 
    p[p1] = p2 
 
def parent(self, p, i): 
    root = i 
    while p[root] != root: 
        root = p[root] 
    while p[i] != i: # 路径压缩 ?
        x = i; i = p[i]; p[x] = root 
    return root


//C/C++
class UnionFind {
public:
    UnionFind(vector<vector<char>>& grid) {
        count = 0;
        int m = grid.size();
        int n = grid[0].size();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == '1') {
                    parent.push_back(i * n + j);
                    ++count;
                }
                else {
                    parent.push_back(-1);
                }
                rank.push_back(0);
            }
        }
    }

//递归
    int find(int i) {
        if (parent[i] != i) {
            parent[i] = find(parent[i]);
        }
        return parent[i];
    }
    void unite(int x, int y) {
        int rootx = find(x);
        int rooty = find(y);
        if (rootx != rooty) {
            if (rank[rootx] < rank[rooty]) {
                swap(rootx, rooty);
            }
            parent[rooty] = rootx;
            if (rank[rootx] == rank[rooty]) rank[rootx] += 1;
            --count;
        }
    }
    int getCount() const {
        return count;
    }
private:
    vector<int> parent;
    vector<int> rank;
    int count;
};



// JavaScript
class unionFind {
  constructor(n) {
    this.count = n;
    this.parent = new Array(n);
    for (let i = 0; i < n; i++) {
      this.parent[i] = i;
    }
  }

  find(p) {
    let root = p;
    while (parent[root] !== root) {
      root = parent[root];
    }
    // 压缩路径
    while (parent[p] !== p) {
      let x = p;
      p = this.parent[p];
      this.parent[x] = root;
    }
    return root;
  }

  union(p, q) {
    let rootP = find(p);
    let rootQ = find(q);
    if (rootP === rootQ) return;
    this.parent[rootP] = rootQ;
    this.count--;
  }
}


三. A* 代码模板
//Python
def AstarSearch(graph, start, end):
    pq = collections.priority_queue() # 优先级 —> 估价函数
    pq.append([start]) 
    visited.add(start)
    while pq: 
        node = pq.pop() # can we add more intelligence here ?
        visited.add(node)
        process(node) 
        nodes = generate_related_nodes(node) 
   unvisited = [node for node in nodes if node not in visited]
        pq.push(unvisited)

//Java
public class AStar
{
    public final static int BAR = 1; // 障碍值
    public final static int PATH = 2; // 路径
    public final static int DIRECT_VALUE = 10; // 横竖移动代价
    public final static int OBLIQUE_VALUE = 14; // 斜移动代价
    
    Queue<Node> openList = new PriorityQueue<Node>(); // 优先队列(升序)
    List<Node> closeList = new ArrayList<Node>();
    
    /**
     * 开始算法
     */
    public void start(MapInfo mapInfo)
    {
        if(mapInfo==null) return;
        // clean
        openList.clear();
        closeList.clear();
        // 开始搜索
        openList.add(mapInfo.start);
        moveNodes(mapInfo);
    }


    /**
     * 移动当前结点
     */
    private void moveNodes(MapInfo mapInfo)
    {
        while (!openList.isEmpty())
        {
            Node current = openList.poll();
            closeList.add(current);
            addNeighborNodeInOpen(mapInfo,current);
            if (isCoordInClose(mapInfo.end.coord))
            {
                drawPath(mapInfo.maps, mapInfo.end);
                break;
            }
        }
    }
    
    /**
     * 在二维数组中绘制路径
     */
    private void drawPath(int[][] maps, Node end)
    {
        if(end==null||maps==null) return;
        System.out.println("总代价：" + end.G);
        while (end != null)
        {
            Coord c = end.coord;
            maps[c.y][c.x] = PATH;
            end = end.parent;
        }
    }


    /**
     * 添加所有邻结点到open表
     */
    private void addNeighborNodeInOpen(MapInfo mapInfo,Node current)
    {
        int x = current.coord.x;
        int y = current.coord.y;
        // 左
        addNeighborNodeInOpen(mapInfo,current, x - 1, y, DIRECT_VALUE);
        // 上
        addNeighborNodeInOpen(mapInfo,current, x, y - 1, DIRECT_VALUE);
        // 右
        addNeighborNodeInOpen(mapInfo,current, x + 1, y, DIRECT_VALUE);
        // 下
        addNeighborNodeInOpen(mapInfo,current, x, y + 1, DIRECT_VALUE);
        // 左上
        addNeighborNodeInOpen(mapInfo,current, x - 1, y - 1, OBLIQUE_VALUE);
        // 右上
        addNeighborNodeInOpen(mapInfo,current, x + 1, y - 1, OBLIQUE_VALUE);
        // 右下
        addNeighborNodeInOpen(mapInfo,current, x + 1, y + 1, OBLIQUE_VALUE);
        // 左下
        addNeighborNodeInOpen(mapInfo,current, x - 1, y + 1, OBLIQUE_VALUE);
    }


    /**
     * 添加一个邻结点到open表
     */
    private void addNeighborNodeInOpen(MapInfo mapInfo,Node current, int x, int y, int value)
    {
        if (canAddNodeToOpen(mapInfo,x, y))
        {
            Node end=mapInfo.end;
            Coord coord = new Coord(x, y);
            int G = current.G + value; // 计算邻结点的G值
            Node child = findNodeInOpen(coord);
            if (child == null)
            {
                int H=calcH(end.coord,coord); // 计算H值
                if(isEndNode(end.coord,coord))
                {
                    child=end;
                    child.parent=current;
                    child.G=G;
                    child.H=H;
                }
                else
                {
                    child = new Node(coord, current, G, H);
                }
                openList.add(child);
            }
            else if (child.G > G)
            {
                child.G = G;
                child.parent = current;
                openList.add(child);
            }
        }
    }


    /**
     * 从Open列表中查找结点
     */
    private Node findNodeInOpen(Coord coord)
    {
        if (coord == null || openList.isEmpty()) return null;
        for (Node node : openList)
        {
            if (node.coord.equals(coord))
            {
                return node;
            }
        }
        return null;
    }




    /**
     * 计算H的估值：“曼哈顿”法，坐标分别取差值相加
     */
    private int calcH(Coord end,Coord coord)
    {
        return Math.abs(end.x - coord.x)
                + Math.abs(end.y - coord.y);
    }
    
    /**
     * 判断结点是否是最终结点
     */
    private boolean isEndNode(Coord end,Coord coord)
    {
        return coord != null && end.equals(coord);
    }


    /**
     * 判断结点能否放入Open列表
     */
    private boolean canAddNodeToOpen(MapInfo mapInfo,int x, int y)
    {
        // 是否在地图中
        if (x < 0 || x >= mapInfo.width || y < 0 || y >= mapInfo.hight) return false;
        // 判断是否是不可通过的结点
        if (mapInfo.maps[y][x] == BAR) return false;
        // 判断结点是否存在close表
        if (isCoordInClose(x, y)) return false;


        return true;
    }


    /**
     * 判断坐标是否在close表中
     */
    private boolean isCoordInClose(Coord coord)
    {
        return coord!=null&&isCoordInClose(coord.x, coord.y);
    }


    /**
     * 判断坐标是否在close表中
     */
    private boolean isCoordInClose(int x, int y)
    {
        if (closeList.isEmpty()) return false;
        for (Node node : closeList)
        {
            if (node.coord.x == x && node.coord.y == y)
            {
                return true;
            }
        }
        return false;
    }
}


C/C++
class Node {
public:
    int x;
    int y;
    bool operator< (const Node &A) {
        // 
    }
};

void generate_related_nodes(Node &node) {
    // 
}

void process(Node &node) {
    // 
}

void AstarSearch(vector<vector<int>>& graph, Node& start, Node& end) {
    vector<vector<bool> > visited(graph.size(), vector<bool>(graph[0].size(), false));
    priority_queue<Node> q;
    q.push(start);

    while (!q.empty()) {
        Node cur = q.top();
        q.pop();
        visited[cur.x][cur.y] = true;


        process(node);
        vector<Node> nodes = generate_related_nodes(node) 
        for (auto node : nodes) {
            if (!visited[node.x][node.y]) q.push(node);
        }
    }

    return ;
}




JavaScript
// Javascript
function aStarSearch(graph, start, end) {
  // graph 使用二维数组来存储数据
  let collection = new SortedArray([start], (p1, p2) => distance(p1) - distance(p2));

  while (collection.length) {
    let [x, y] = collection.take();
    if (x === end[0] && y === end[1]) {
      return true;
    }

    insert([x - 1, y]);
    insert([x + 1, y]);
    insert([x, y - 1]);
    insert([x, y + 1]);
  }
  return false;

  function distance([x, y]) {
    return (x - end[0]) ** 2 - (y - end[1]) ** 2;
  }

  function insert([x, y]) {
    if (graph[x][y] !== 0) return;
    if (x < 0 || y < 0 || x >= graph[0].length || y > graph.length) {
      return;
    }
    graph[x][y] = 2;
    collection.insert([x, y]);
  }
}


class SortedArray {
  constructor(data, compare) {
    this.data = data;
    this.compare = compare;
  }

  // 每次取最小值
  take() {
    let min = this.data[0];

    let minIndex = 0;
    for (let i = 1; i < this.data.length; i++) {
      if (this.compare(min, this.data[i]) > 0) {
        min = this.data[i];
        minIndex = i;
      }
    }
    this.data[minIndex] = this.data[this.data.length - 1];
    this.data.push();

    return min;
  }

  insert(value) {
    this.data.push(value);
  }

  get length() {
    return this.data.length;
  }
}
