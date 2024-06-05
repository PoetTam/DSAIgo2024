# Assignment #B: 图论和树算

Updated 1709 GMT+8 Apr 28, 2024

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

### 28170: 算鹰

dfs, http://cs101.openjudge.cn/practice/28170/



思路：每遇到一个己方落点，就把它周围相连的己方棋子和该棋子清除。20min。



代码

```python
def YING(x,y):
    moves = [(0,1),(0,-1),(1,0),(-1,0)]
    global chessboard
    for dx,dy in moves:
        nx,ny = x+dx,y+dy
        if 0 <= nx < 10 and 0 <= ny < 10:
            if chessboard[nx][ny] == '.':
                chessboard[nx][ny] = '_'
                YING(nx,ny)

chessboard = [list(input()) for _ in range(10)]
Ying = 0
for x in range(10):
    for y in range(10):
        if chessboard[x][y] == '.':
            YING(x, y)
            chessboard[x][y] = '_'
            Ying += 1
print(Ying)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240430142221431](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240430142221431.png)



### 02754: 八皇后

dfs, http://cs101.openjudge.cn/practice/02754/



思路：由题意可知，所有可能方法只能是每个数字只出现一次的情况。（对于某个满足要求的8皇后的摆放方法，定义一个皇后串a与之对应，即a=b1b2...b8，其中bi为相应摆法中第i行皇后所处的列数。本质上行列对称。）直接输出全排列，然后按照斜线上不能有其他皇后筛选，最后将结果按照数字大小对列表进行排序。

貌似这种方法比较讨巧（）。40min。



代码

```python
from itertools import permutations
def eightOK(lst:list):
    address = set((i+1,lst[i]) for i in range(8))
    for (x,y) in address:
        for d in range(1,9-x):
            if ((x+d,y+d) in address or (x+d,y-d) in address
            or (x-d,y+d) in address or (x-d,y-d) in address):
                return False
    return True
ways = []
for lst in permutations(list(range(1,9))):
    if eightOK(lst):
        string = ''.join(str(i) for i in lst)
        ways.append(int(string))
ways.sort()

for _ in range(int(input())):
    print(ways[int(input())-1])
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240430150803233](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240430150803233.png)



### 03151: Pots

bfs, http://cs101.openjudge.cn/practice/03151/



思路：每次都考虑六种操作。30min。



代码

```python
def find_water_operations(A, B, C):
    queue = [(0, 0, [])]
    visited = {(0, 0)}

    while queue:
        potA, potB, operations = queue.pop(0)
        if potA == C or potB == C: return True,operations

        if potA < A: # fill A
            new_potA, new_potB = A, potB
            if (new_potA, new_potB) not in visited:
                visited.add((new_potA, new_potB))
                queue.append((new_potA, new_potB, operations + ['FILL(1)']))
        if potB < B: # fill B
            new_potA, new_potB = potA, B
            if (new_potA, new_potB) not in visited:
                visited.add((new_potA, new_potB))
                queue.append((new_potA, new_potB, operations + ['FILL(2)']))

        if potA > 0: # drop A
            new_potA, new_potB = 0, potB
            if (new_potA, new_potB) not in visited:
                visited.add((new_potA, new_potB))
                queue.append((new_potA, new_potB, operations + ['DROP(1)']))
        if potB > 0: # drop B
            new_potA, new_potB = potA, 0
            if (new_potA, new_potB) not in visited:
                visited.add((new_potA, new_potB))
                queue.append((new_potA, new_potB, operations + ['DROP(2)']))

        if potA > 0 and potB < B: # from A to B
            change = min(B-potB,potA)
            new_potA, new_potB = potA-change,potB+change
            if (new_potA, new_potB) not in visited:
                visited.add((new_potA, new_potB))
                queue.append((new_potA, new_potB, operations+['POUR(1,2)']))
        if potB > 0 and potA < A: # from B to A
            change = min(potB,A-potA)
            new_potA, new_potB = potA+change, potB-change
            if (new_potA, new_potB) not in visited:
                visited.add((new_potA, new_potB))
                queue.append((new_potA, new_potB, operations+['POUR(2,1)']))
    return False,[]

A, B, C = map(int, input().split())
flag,result = find_water_operations(A, B, C)
if flag:
    print(len(result))
    print(*result,sep='\n')
else:
    print('impossible')
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240430214924350](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240430214924350.png)



### 05907: 二叉树的操作

http://cs101.openjudge.cn/practice/05907/



思路：没有特别的操作。20min。



代码

```python
class BinaryTree:
    def __init__(self,root):
        self.root = root
        self.left = None
        self.right = None
        self.father = None

for _ in range(int(input())):
    n,m = map(int,input().split())
    tree_list = list(BinaryTree(i) for i in range(n))

    for __ in range(n):
        root,left,right = map(int,input().split())
        if left != -1:
            tree_list[root].left = tree_list[left]
            tree_list[left].father = tree_list[root]
        if right != -1:
            tree_list[root].right = tree_list[right]
            tree_list[right].father = tree_list[root]

    for __ in range(m):
        type,*tu = map(int,input().split())

        if type == 1: # swap
            x,y = tu
            tree1,tree2 = tree_list[x],tree_list[y]
            father1 = tree1.father
            father2 = tree2.father
            if father2 is father1:
                father2.left,father2.right = father2.right,father2.left

            else:
                if father1.left == tree1:
                    father1.left = tree2
                else: father1.right = tree2

                if father2.left == tree2:
                    father2.left = tree1
                else: father2.right = tree1
                tree1.father,tree2.father = father2,father1

        elif type == 2:
            node = tree_list[tu[0]]
            while node.left:
                node = node.left
            print(node.root)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240430160019017](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240430160019017.png)



### 18250: 冰阔落 I

Disjoint set, http://cs101.openjudge.cn/practice/18250/



思路：使用了两个字典，一个储存每个可乐对应的杯子，一个对应杯子对应的可乐，然后按照要求修改字典。耗时60min。



代码

```python
while True:
    try:
        n,m = map(int,input().split())
    except EOFError:
        break
    cola = {i:i for i in range(1,n+1)} # 记录每个可乐的杯子
    colas = {i:[i] for i in range(1,n+1)} # 记录每个杯子装了哪些可乐
    for _ in range(m):
        x,y = map(int,input().split())
        cup1 = cola[x]
        cup2 = cola[y]
        if cup1 == cup2:
            print('Yes')
        else:
            print('No')
            for i in colas[cup2]:
                cola[i] = cup1
            colas[cup1] += colas[cup2]
            del colas[cup2]
    cups = list(colas.keys())
    print(len(cups))
    print(*sorted(cups))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240430202850305](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240430202850305.png)



### 05443: 兔子与樱花

http://cs101.openjudge.cn/practice/05443/



思路：多添加一个路径进入pos即可。



代码

```python
import heapq
import math
def dijkstra(graph,start,end,P):
    if start == end: return []
    dist = {i:(math.inf,[]) for i in graph}
    dist[start] = (0,[start])
    pos = []
    heapq.heappush(pos,(0,start,[]))
    while pos:
        dist1,current,path = heapq.heappop(pos)
        for (next,dist2) in graph[current].items():
            if dist2+dist1 < dist[next][0]:
                dist[next] = (dist2+dist1,path+[next])
                heapq.heappush(pos,(dist1+dist2,next,path+[next]))
    return dist[end][1]

P = int(input())
graph = {input():{} for _ in range(P)}
for _ in range(int(input())):
    place1,place2,dist = input().split()
    graph[place1][place2] = graph[place2][place1] = int(dist)

for _ in range(int(input())):
    start,end = input().split()
    path = dijkstra(graph,start,end,P)
    s = start
    current = start
    for i in path:
        s += f'->({graph[current][i]})->{i}'
        current = i
    print(s)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240430210139986](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240430210139986.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

感觉八皇后没有群友们说的离谱，没有使用dfs，使用全排列找出了所有可能的方法。打算放假结束了试试dfs。

这周的作业好像都是之前作业的变式，感觉没有上周的难。





