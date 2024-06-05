# Assignment #9: 图论：遍历，及 树算

Updated 1739 GMT+8 Apr 14, 2024

2024 spring, Complied by ==PoetTam==



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：Windows 11

Python编程环境：PyCharm 2023.2.1 (Community Edition)



## 1. 题目

### 04081: 树的转换

http://cs101.openjudge.cn/dsapre/04081/



思路：原则是左孩子右兄弟。不过需要使用递归，对每一个子树进行转换。耗时30min。



代码

```python
class Tree:
    def __init__(self,key):
        self.root = key
        self.child = []

    def get_height(self):
        height = 0
        for i in self.child:
            height = max(height,i.get_height()+1)
        return height

class BinaryTree:
    def __init__(self,key):
        self.root = key
        self.left = None
        self.right = None

    def get_height(self):
        if self.left is None:
            height1 = 0
        else:
            height1 = self.left.get_height()+1
        if self.right is None:
            height2 = 0
        else:
            height2 = self.right.get_height()+1
        return max(height1,height2)

def build_tree(s):
    stack = []
    tree1 = Tree(0)
    current = tree1
    for i in s:
        if i == 'd':
            new_node = Tree(0)
            current.child.append(new_node)
            stack.append(current)
            current = new_node
        if i == 'u':
            current = stack.pop()
    return tree1

def Tree2BinaryTree(tree:Tree):
    new_tree = BinaryTree(None)
    new_tree.root = tree.root

    if not tree.child:
        return new_tree
    elif len(tree.child) == 1:
        new_tree.left = Tree2BinaryTree(tree.child[0])
        return new_tree
    else:
        new_tree.left = Tree2BinaryTree(tree.child[0])
        current = new_tree.left
        for i in range(1,len(tree.child)):
            current.right = Tree2BinaryTree(tree.child[i])
            current = current.right
        return new_tree

s = input()
tree1 = build_tree(s)
tree2 = Tree2BinaryTree(tree1)
print(f'{tree1.get_height()} => {tree2.get_height()}')
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240416111444128](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240416111444128.png)



### 08581: 扩展二叉树

http://cs101.openjudge.cn/dsapre/08581/



思路：根据规则把树建出来，然后遍历。耗时40min。



代码

```python
class BinaryTree:
    def __init__(self,key):
        self.root = key
        self.left = None
        self.right = None

def build_tree(s):
    stack = []
    tree = BinaryTree(s[0])
    current = tree
    for i in s[1:]:
        if i != '.':
            new_node = BinaryTree(i)
            while True:
                if current.left is None:
                    current.left = new_node
                    stack.append(current)
                    current = current.left
                    break
                elif current.right is None:
                    current.right = new_node
                    stack.append(current)
                    current = current.right
                    break
                else:
                    current = stack.pop()
        else:
            while True:
                if current.left is None:
                    current.left = i
                    break
                elif current.right is None:
                    current.right = i
                    if current: current = stack.pop()
                    break
                else:
                    current = stack.pop()
    return tree

def inorder(tree:BinaryTree):
    lst = [tree.root]
    if isinstance(tree.left,BinaryTree):
        lst1 = inorder(tree.left)
    else:
        lst1 = []
    if isinstance(tree.right,BinaryTree):
        lst2 = inorder(tree.right)
    else:
        lst2 = []
    return lst1+lst+lst2

def postorder(tree:BinaryTree):
    lst = [tree.root]
    if isinstance(tree.left, BinaryTree):
        lst1 = postorder(tree.left)
    else:
        lst1 = []
    if isinstance(tree.right, BinaryTree):
        lst2 = postorder(tree.right)
    else:
        lst2 = []
    return lst1 + lst2 + lst

s = input()
tree = build_tree(s)
print(*inorder(tree),sep='')
print(*postorder(tree),sep='')
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240416115208964](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240416115208964.png)



### 22067: 快速堆猪

http://cs101.openjudge.cn/practice/22067/



思路：使用两个列表，一个是栈，负责堆猪赶猪，一个是最小堆，负责在需要的时候输出最小值。此外需要一个集合存储被赶走的猪。**懒删除**：当需要输出最小值时，需要比较最小堆的最小值是否在集合中，如果在其中，需要同时删除集合和堆的该数据，直至最小值不在集合中。~~这个方法是看群里的，很好用的方法~~。耗时2h。



代码

```python
import heapq
stack = []
heap = []
set1 = set() # 标记已经删除的数据
while True:
    try:
        s = input()
    except EOFError:
        break

    if s == 'pop':
        if stack:
            set1.add(stack.pop()) # 标记

    elif s == 'min':
        while heap and heap[0] in set1:
            set1.remove(heapq.heappop(heap))
        if heap:
            print(heap[0])

    else:
        s1,s2 = s.split()
        s2 = int(s2)
        stack.append(s2)
        heapq.heappush(heap,s2)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240416195028531](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240416195028531.png)



### 04123: 马走日

dfs, http://cs101.openjudge.cn/practice/04123



思路：根据dfs的规则写就是了。甚至不需要优化。耗时30min。



代码

```python
def dfs(n,m,x,y):
    if x < 0 or x >=n or y < 0 or y >= m: # 超出范围
        return 0
    if chessboard[x][y] == 1: # 不能重复走
        return 0

    chessboard[x][y] = 1 # 标记走过的点
    if chessboard == [[1]*m]*n: # 全部走完
        chessboard[x][y] = 0
        return 1

    count = 0
    count += dfs(n,m,x-1,y-2)
    count += dfs(n,m,x-1,y+2)
    count += dfs(n,m,x+1,y-2)
    count += dfs(n,m,x+1,y+2)
    count += dfs(n,m,x-2,y-1)
    count += dfs(n,m,x-2,y+1)
    count += dfs(n,m,x+2,y-1)
    count += dfs(n,m,x+2,y+1)
    chessboard[x][y] = 0 # 恢复

    return count

for _ in range(int(input())):
    n,m,x,y = map(int,input().split())
    chessboard = [[0]*m for _ in range(n)] # create the chessboard
    print(dfs(n,m,x,y))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240416202452276](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240416202452276.png)



### 28046: 词梯

bfs, http://cs101.openjudge.cn/practice/28046/



思路：看了题解。bfs部分很好写，但是一直不知道bfs怎样才能停止。看了题解发现可以先全部bfs，同时记录颜色和distance，最后回溯。如果不用回溯会很麻烦。耗时2h。



代码

```python
class Vertex:
    def __init__(self, num):
        self.key = num
        self.connectedTo = []
        self.color = 'white'
        self.distance = 0
        self.previous = None

    def add_neighbor(self, nbr):
        self.connectedTo.append(nbr)

bucket = {} # 桶
graph = {}
for _ in range(int(input())):
    s = input()
    graph[s] = Vertex(s)
    for i in range(4):
        key = s[:i] + '_' + s[i + 1:]
        if key not in bucket:
            bucket[key] = [s]
        else:
            bucket[key].append(s)

for key in bucket:
    for i in bucket[key]:
        for j in bucket[key]:
            if i != j:
                graph[i].add_neighbor(graph[j])
bucket.clear()

def bfs(start:Vertex):
    start.distance = 0
    start.previous = None

    queue = []
    queue.append(start)
    while queue:
        current = queue.pop(0) # 取队首作为当前顶点
        for neighbor in current.connectedTo:   # 遍历当前顶点的邻接顶点
            if neighbor.color == "white":
                neighbor.color = "gray"
                neighbor.distance = current.distance + 1
                neighbor.previous = current
                queue.append(neighbor)
        current.color = "black" # 当前顶点已经处理完毕，设黑色

def traverse(vertex:Vertex):
    ans = []
    current = vertex
    while (current.previous):
        ans.append(current.key)
        current = current.previous
    ans.append(current.key)
    return ans

start,end = input().split()
bfs(graph[start])
ans = traverse(graph[end])
if len(ans) == 1:
    print('NO')
else:
    print(*ans[::-1])
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240417162046547](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240417162046547.png)



### 28050: 骑士周游

dfs, http://cs101.openjudge.cn/practice/28050/



思路：如果使用马走日的方法会直接tle，需要优化。搜了网上的方法，发现：可以用图存储每一个节点可以到达的其它节点。然后再使用dfs，遇到已经遍历过的节点返回False，并回到上一个节点；发现遍历完了所有的节点返回True。但是发现这样还是和马走日的思路本质上没有区别，依旧会tle。最后翻看群里，发现提到了每一次都先走可选择少的节点，这样可以大大缩短程序运行时间。耗时2h。



代码

```python
class Vertex:
    def __init__(self,id):
        self.id = id
        self.connectedTo = []
        self.color = 'white'
def pos_to_node_id(x, y, bdSize):
    return x * bdSize + y

def gen_legal_moves(row, col, board_size):
    new_moves = []
    move_offsets = [(-1, -2),(-1, 2),(-2, -1),(-2, 1),
                    (1, -2),(1, 2),(2, -1),(2, 1)]
    for r_off, c_off in move_offsets:
        if 0 <= row + r_off < board_size and 0 <= col + c_off < board_size:
            new_moves.append((row + r_off, col + c_off))
    return new_moves

def knight_graph(board_size):
    kt_graph = {}
    for row in range(board_size):
        for col in range(board_size):
            node_id = pos_to_node_id(row, col, board_size)
            kt_graph[node_id] = Vertex(node_id)
            new_positions = gen_legal_moves(row, col, board_size)
            for row2, col2 in new_positions:
                other_node_id = pos_to_node_id(row2, col2, board_size)
                kt_graph[node_id].connectedTo.append(other_node_id)
    return kt_graph

def ordered_by_avail(n:Vertex):
    res_list = []
    for v in n.connectedTo:
        if kt_graph[v].color == "white":
            c = 0
            for w in kt_graph[v].connectedTo:
                if kt_graph[w].color == "white":
                    c += 1
            res_list.append((c,kt_graph[v]))
    res_list.sort(key=lambda x: x[0])
    return [y[1] for y in res_list]

def knight_tour(n, path, u:Vertex, limit):
    u.color = "gray"
    path.append(u)
    if n < limit:
        neighbors = ordered_by_avail(u) #对所有的合法移动依次深入
        i = 0
        for nbr in neighbors:
            if nbr.color == "white" and knight_tour(n + 1, path, nbr, limit):
                return True
        else:
            path.pop()
            u.color = "white"
            return False
    else:
        return True

n = int(input())
kt_graph = knight_graph(n)
x,y = map(int,input().split())
start = kt_graph[x*n+y]
done = knight_tour(0, [], start, n*n-1)
if done:
    print("success")
else:
    print("fail")
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240417164728566](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240417164728566.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

本周题目的难度比之前高了很多。树的转换和上周的一个题目可以结合起来看，这样**二叉树转普通树、普通树转二叉树**的方法都涉及到了。快速堆猪使用的**懒删除**方法，是一种新的方式，可以和堆结合在一起用。bfs在要求寻找最短路径是可能不直接，可以使用prev属性记录前面的节点，bfs完后，再**从最终节点进行回溯**。至于dfs，学会了优先最后续选择最少的节点的**Warnsdorff算法**，精髓是提前给后续节点按照可选择数升序排序，这种方法在只要求True or False时尤其实用。
