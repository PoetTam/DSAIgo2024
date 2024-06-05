# Assignment #A: 图论：遍历，树算及栈

Updated 2018 GMT+8 Apr 21, 2024

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

### 20743: 整人的提词本

http://cs101.openjudge.cn/practice/20743/



思路：耗时20min。按照思路进行反转，最后去掉括号，输出结果。



代码

```python
str_list = list(input())
n = len(str_list)
stack = []
for i in range(n):
    if str_list[i] == '(':
        stack.append(i)
    elif str_list[i] == ')':
        j = stack.pop()
        str_list[j+1:i] = str_list[i-1:j:-1]
s2 = ''
for i in str_list:
    if i != '(' and i != ')':
        s2 += i
print(s2)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240424131925007](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240424131925007.png)



### 02255: 重建二叉树

http://cs101.openjudge.cn/practice/02255/



思路：10min。建立二叉树即可。



代码

```python
def construct_binary(preorder:str,inorder:str):
    if preorder == inorder:
        return preorder[::-1]
    node = preorder[0]
    i = inorder.find(node)
    return (construct_binary(preorder[1:i+1],inorder[:i])
            +construct_binary(preorder[i+1:],inorder[i+1:])+node)

while True:
    try:
        s1,s2=input().split()
        print(construct_binary(s1,s2))
    except EOFError:
        break
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240424132941310](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240424132941310.png)



### 01426: Find The Multiple

http://cs101.openjudge.cn/practice/01426/

要求用bfs实现



思路：主要关注余数的状态。只要保证余数变化后能够整除即可。



代码

```python
def bfs(n):
    queue = [(1%n,'1')]
    visited = [1%n]

    while queue:
        mod,num_str = queue.pop(0)
        if mod == 0:
            return num_str
        for digit in '01':
            new_nuw_str = num_str+digit
            new_mod = (mod*10+int(digit))%n
            if new_mod not in visited:
                queue.append((new_mod,new_nuw_str))
                visited.append(new_mod)

while True:
    n = int(input())
    if n == 0:
        break
    print(bfs(n))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240428012143547](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240428012143547.png)



### 04115: 鸣人和佐助

bfs, http://cs101.openjudge.cn/practice/04115/



思路：需要加入查克拉的考虑，其他和普通bfs没有区别。



代码

```python
from collections import deque
def bfs(g,x,y,t):
    visited = set()
    q = deque()
    q.append((t, x, y, 0))
    dire = [(-1, 0), (0, -1), (1, 0), (0, 1)]
    while q:
        t, x, y, ans = q.popleft()
        for dx, dy in dire:
            nx = x + dx;ny = y + dy
            if 0 <= nx < m and 0 <= ny < n:
                nt = t if g[nx][ny] != "#" else t - 1

                if nt >= 0 and (nt, nx, ny) not in visited:
                    new_ans = ans + 1
                    if g[nx][ny] == "+":
                        return new_ans
                    q.append((nt, nx, ny, new_ans))
                    visited.add((nt, nx, ny))
    return -1

m, n, t = map(int, input().split())
g = []
for i in range(m):
    g.append(list(input()))
for i in range(m):
    for j in range(n):
        if g[i][j] == "@":
            x = i;y = j

print(bfs(g,x,y,t))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240428011401010](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240428011401010.png)



### 20106: 走山路

Dijkstra, http://cs101.openjudge.cn/practice/20106/



思路：普通的dijkstra思路。



代码

```python
import heapq
import math
def dijkstra(m,n,x1,y1,x2,y2,path:list):
    moves = [(0,-1),(0,1),(-1,0),(1,0)]
    distances = [[math.inf]*n for _ in range(m)]
    pos = []
    if path[x1][y1] == '#' or path[x2][y2] == '#':
        return 'NO'
    distances[x1][y1] = 0
    heapq.heappush(pos,(0,x1,y1))
    while pos:
        cur_d,x,y = heapq.heappop(pos)
        if x == x2 and y == y2:
            return cur_d
        h = int(path[x][y])
        for nx,ny in moves:
            new_x,new_y = nx+x,ny+y
            if 0 <= new_x < m and 0 <= new_y < n and path[new_x][new_y] != '#':
                if distances[new_x][new_y] > cur_d+abs(int(path[new_x][new_y])-h):
                    distances[new_x][new_y] = cur_d+abs(int(path[new_x][new_y])-h)
                    heapq.heappush(pos,(distances[new_x][new_y],new_x,new_y))
    return 'NO'

m, n, p = map(int, input().split())
path = [input().split() for _ in range(m)]

for _ in range(p):
    a, b, c, d = map(int,input().split())
    print(dijkstra(m,n,a,b,c,d,path))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240426223838300](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240426223838300.png)



### 05442: 兔子与星空

Prim, http://cs101.openjudge.cn/practice/05442/



思路：prim算法。



代码

```python
def prim(graph):
    selected = set()
    selected.add(list(graph.keys())[0])
    unselected = set(graph.keys()) - selected
    weight_sum = 0

    while unselected:
        min_weight = float('inf')
        start_vertex = None
        end_vertex = None
        for vertex in selected:
            for neighbor, weight in graph[vertex].items():
                if neighbor in unselected:
                    if min_weight > weight:
                        min_weight = weight
                        start_vertex = vertex
                        end_vertex = neighbor
        weight_sum += min_weight
        selected.add(end_vertex)
        unselected.remove(end_vertex)
    return weight_sum

n = int(input())
graph = {chr(i):{} for i in range(ord('A'),ord('A')+n)}
for _ in range(n-1):
    vertex,k,*lst = input().split()
    for i in range(int(k)):
        graph[vertex][lst[2*i]] = int(lst[2*i+1])
        graph[lst[2*i]][vertex] = int(lst[2*i+1])

print(prim(graph))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240428015019869](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240428015019869.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

学会了bfs的变式应用。感觉bfs相比dfs变化更丰富。

此外dijlstra算法和prim算法也很有意思。

