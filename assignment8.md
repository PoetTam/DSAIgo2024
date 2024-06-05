# Assignment #8: 图论：概念、遍历，及 树算

Updated 1919 GMT+8 Apr 8, 2024

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

### 19943: 图的拉普拉斯矩阵

matrices, http://cs101.openjudge.cn/practice/19943/

请定义Vertex类，Graph类，然后实现



思路：最先尝试了邻接矩阵的方法，然后可以根据邻接矩阵得到D和A，两矩阵相减得到L。

然后尝试了定义类的方法。根据定义，对角线上的元素为每个节点连接的边数，其余位置，两节点相连值为-1。

耗时10min。



代码

```python
# method1 两个列表实现
n,m = map(int,input().split())
A = [[0 for i in range(n)] for j in range(n)]
for _ in range(m):
    i,j = map(int,input().split())
    A[i][j] = A[j][i] = 1
D = [[0 for i in range(n)] for j in range(n)]
for i in range(n):
    D[i][i] = sum(A[i])
L = [[D[i][j]-A[i][j] for i in range(n)] for j in range(n)]
for i in L:
    print(*i)

# mothod2 定义类
class Vertex:
    def __init__(self,id):
        self.id = id
        self.connect = []

class Graph:
    def __init__(self):
        self.vertex = {}

n,m = map(int,input().split())
graph = Graph()
for _ in range(n):
    graph.vertex[_] = Vertex(_)
for _ in range(m):
    i,j = map(int,input().split())
    graph.vertex[i].connect.append(j);graph.vertex[j].connect.append(i)
L = [[0 for i in range(n)] for j in range(n)]
for i in graph.vertex:
    L[i][i] = len(graph.vertex[i].connect)
    for j in graph.vertex[i].connect:
        L[i][j] = -1
for i in L:
    print(*i)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240412143822121](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240412143822121.png)

![image-20240414221250184](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240414221250184.png)



### 18160: 最大连通域面积

matrix/dfs similar, http://cs101.openjudge.cn/practice/18160



思路：使用dfs，向八个方向依次拓展。

耗时20min。



代码

```python
def dfs(graph,i,j):
    if i < 0 or i >= len(graph) or j < 0 or j >= len(graph[0]): return 0
    elif graph[i][j] == '.': return 0
    count = 1
    graph[i][j] = '.'
    count += dfs(graph, i-1, j-1)
    count += dfs(graph, i-1, j)
    count += dfs(graph, i-1, j+1)
    count += dfs(graph, i, j-1)
    count += dfs(graph, i, j+1)
    count += dfs(graph, i+1, j-1)
    count += dfs(graph, i+1, j)
    count += dfs(graph, i+1, j+1)
    return count

for _ in range(int(input())):
    n,m = map(int,input().split())
    graph = [[0] for i in range(n)]
    for i in range(n):
        graph[i] = list(input())

    maxarea = 0
    for i in range(n):
        for j in range(m):
            maxarea = max(maxarea,dfs(graph,i,j))
    print(maxarea)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240412152955012](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240412152955012.png)



### sy383: 最大权值连通块

https://sunnywhy.com/sfbj/10/3/383



思路：同最大联通域，不过每次相加的数为weight值。耗时30min。



代码

```python
class Vertex:
    def __init__(self,key):
        self.id = key
        self.connected = []

    def get_connection(self,vertexes):
        lst = []
        while self.connected:
            k = self.connected.pop()
            lst += vertexes[k].get_connection(vertexes)
        return set(lst+[self.id])

n,m = map(int,input().split())
lst = list(map(int,input().split()))
vertexes = [Vertex(i) for i in range(n)]
for _ in range(m):
    i,j = map(int,input().split())
    vertexes[i].connected.append(j)
    vertexes[j].connected.append(i)
maxweight = 0
for i in range(n):
    set1 = vertexes[i].get_connection(vertexes)
    weight = 0
    for j in set1:
        weight += lst[j]
    maxweight = max(maxweight,weight)
print(maxweight)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

<img src="C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240412163451449.png" alt="image-20240412163451449" style="zoom:50%;" />



### 03441: 4 Values whose Sum is 0

data structure/binary search, http://cs101.openjudge.cn/practice/03441



思路：两两划分降低时间复杂度。耗时80min。



代码

```python
l_a,l_b,l_c,l_d = [],[],[],[]
for _ in range(int(input())):
    a,b,c,d = map(int,input().split())
    l_a.append(a);l_b.append(b);l_c.append(c);l_d.append(d)
count = 0
ab = {}
for a in l_a:
    for b in l_b:
        s=a+b
        if s in ab:
            ab[s] += 1
        else:
            ab[s] = 1
for c in l_c:
    for d in l_d:
        t = c+d
        if -t in ab:
            count += ab[-t]
print(count)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240414170957478](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240414170957478.png)



### 04089: 电话号码

trie, http://cs101.openjudge.cn/practice/04089/

Trie 数据结构可能需要自学下。



思路：定义Trie类。首先查看电话号码能不能拨通，能加入Trie；不能，flag变为False。耗时40min。



代码

```python
def find(trie,s):
    current = trie
    for j in range(len(s)):
        if current:
            if s[j] not in current:
                return True
            current = current[s[j]]
        else:
            if current is trie:
                return True
            return False
    return False

for _ in range(int(input())):
    trie = {}
    flag = True
    for i in range(int(input())):
        s = input()
        if not find(trie,s):
            flag = False
        else:
            current = trie
            for j in range(len(s)):
                if s[j] not in current: current[s[j]] = {}
                current = current[s[j]]
    print('YES' if flag else 'NO')
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240414142942371](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240414142942371.png)



### 04082: 树的镜面映射

http://cs101.openjudge.cn/practice/04082/



思路：先建立二叉树，然后把二叉树还原为普通树，最后按要求遍历。耗时5h。



代码

```python
class Tree:
    def __init__(self,key):
        self.key = key
        self.children = []

def layer_overturn(tree:Tree):
    lst = []
    queue = [tree]
    while queue:
        current = queue.pop(0)
        if isinstance(current,Tree):
            lst.append(current.key)
            for i in current.children[::-1]:
                queue.append(i)
        else:
            lst.append(current)
    print(*lst)
class BinaryTree:
    def __init__(self,key):
        self.root = key
        self.left = None
        self.right = None

def build_binary(tempList):
    binary = BinaryTree(None)
    stack = []
    current = binary
    for i in tempList:
        s, s1 = i[0], i[1]
        if s1 == '0':
            if current.root is None:
                current.root = s
            else:
                while True:
                    if current.left is None:
                        current.left = BinaryTree(s)
                        stack.append(current)
                        current = current.left
                        break
                    elif current.right is None:
                        current.right = BinaryTree(s)
                        stack.append(current)
                        current = current.right
                        break
                    else:
                        current = stack.pop()

        else:
            while True:
                if current.left is None:
                    current.left = s
                    break
                elif current.right is None:
                    current.right = s
                    if stack: current = stack.pop()
                    break
                else:
                    current = stack.pop()
    return binary

def binary2tree(binary:BinaryTree):
    tree = Tree(None)
    tree.key = binary.root
    current = binary.left

    while isinstance(current,BinaryTree):
        a = current.right
        current.right = None
        tree.children.append(binary2tree(current))
        current = a

    if current != '$' and current is not None:
        tree.children.append(current)
    return tree

n = int(input())
if n == 1:
    binary = BinaryTree(input()[0])
else:
    tempList = list(input().split())
    binary = build_binary(tempList)
tree = binary2tree(binary)
layer_overturn(tree)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240414220042111](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240414220042111.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

又复习了一次dfs，但是还是不怎么会，感觉需要再复习一下dfs的相关内容。镜面映射很难，考虑掉了一种情况，但是根据群里给出的样例修改了一下成功ac，令我想笑的是在s[-1]=='1'考虑到了，但是s[-1]=='0'反而没有考虑到。感觉有些时候还是很需要样例进行debug的。

这次作业很难，花了8h完成。其中镜面映射花了整整5h，感觉要被这道题逼疯了：）。



