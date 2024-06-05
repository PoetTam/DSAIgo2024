# Assignment #F: All-Killed 满分

Updated 1844 GMT+8 May 20, 2024

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

### 22485: 升空的焰火，从侧面看

http://cs101.openjudge.cn/practice/22485/



思路：使用left、right、parent三个列表存储各个结点的关系，然后进行BFS，并记录层数。最后遍历反转后的BFS结果。耗时15min。



代码

```python
N = int(input())
left = [None]*N
right = [None]*N
parent = [None]*N

for i in range(N):
    l,r = map(int,input().split())
    l,r = l-1,r-1
    if l != -2:
        left[i] = l
        parent[l] = i
    if r != -2:
        right[i] = r
        parent[r] = i

root = parent.index(None)

visited = []
queue = [(root,0)]
while queue:
    node,layer = queue.pop(0)
    if left[node] is not None:
        queue.append((left[node], layer + 1))
    if right[node] is not None:
        queue.append((right[node], layer + 1))

    visited.append((node,layer))
visited.reverse()

can_look = []
current_layer = visited[0][1]+1
for (root,layer) in visited:
    if layer != current_layer:
        can_look.append(root+1)
        current_layer = layer

can_look.reverse()
print(*can_look)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240527111329672](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240527111329672.png)



### 28203:【模板】单调栈

http://cs101.openjudge.cn/practice/28203/



思路：使用一个单调递减栈记录。耗时10min。



代码

```python
N = int(input())
num_list = list(map(int,input().split()))
more_list = [0]*N

stack = []
for i in range(N):
    while stack and num_list[i] > num_list[stack[-1]]:
        s = stack.pop()
        if more_list[s] == 0:
            more_list[s] = i+1
    stack.append(i)

print(*more_list)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240527112459788](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240527112459788.png)



### 09202: 舰队、海域出击！

http://cs101.openjudge.cn/practice/09202/



思路：使用拓扑排序。耗时30min。



代码

```python
def tuopo(graph,N):
    indegree = {i:0 for i in range(1,N+1)}
    for node in graph:
        for neighbor in graph[node]:
            indegree[neighbor] += 1

    queue = [node for node in range(1,N+1) if indegree[node] == 0]
    result = []
    while queue:
        node = queue.pop(0)
        result.append(node)
        for neighbor in graph[node]:
            indegree[neighbor] -= 1
            if indegree[neighbor] == 0:
                queue.append(neighbor)

    if len(result) == N:
        return False
    else:
        return True

for _ in range(int(input())):
    N, M = map(int, input().split())
    graph = {i: [] for i in range(1, N + 1)}
    for i in range(M):
        x, y = map(int, input().split())
        graph[x].append(y)

    print('Yes' if tuopo(graph,N) else 'No')
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240527133156435](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240527133156435.png)



### 04135: 月度开销

http://cs101.openjudge.cn/practice/04135/



思路：二分查找+贪心。耗时1h。主要是不知道怎么二分。。。



代码

```python
n, m = map(int, input().split())
expenditure = list(int(input()) for _ in range(n))

def check(x):
    num, s = 1, 0
    for i in range(n):
        if s + expenditure[i] > x:
            s = expenditure[i]
            num += 1
        else:
            s += expenditure[i]
    return num > m

lo = max(expenditure)
hi = sum(expenditure) + 1
while lo < hi:
    mid = (lo + hi) // 2
    if check(mid):
        lo = mid + 1
    else:
        hi = mid
print(lo)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240528150254010](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240528150254010.png)



### 07735: 道路

http://cs101.openjudge.cn/practice/07735/



思路：dijkatra算法的应用。30min。



代码

```python
import heapq
K,N,R = int(input()),int(input()),int(input())
graph = {_:[] for _ in range(1,N+1)}
for _ in range(R):
    S,D,L,T = map(int,input().split())
    graph[S].append((D,L,T))

def dijkstra(graph):
    global K,N,R
    q,ans = [],[]
    heapq.heappush(q,(0,0,1,0))
    while q:
        distance,cost,current,step = heapq.heappop(q)
        if current == N:
            return distance
        for next,dist,need_cost in graph[current]:
            if cost+need_cost <= K and step+1 < N:
                heapq.heappush(q,(dist+distance,cost+need_cost,next,step+1))
    return -1

print(dijkstra(graph))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240528113550185](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240528113550185.png)



### 01182: 食物链

http://cs101.openjudge.cn/practice/01182/



思路：实在没有思路，所以看了群里的题解，发现不用并查集也可以。用ls存储每个动物被存储到哪个环中。



代码

```python
class Circle:
    def __init__(self,root):
        self.kinds = {root:0}
        self.root = root

    def join(self,circle,rotate):
        for idx,kind in circle.kinds.items():
            self.kinds[idx] = (kind-rotate)%3
            ls[idx] = self.root

n, k = map(int, input().split())
ls = list(range(n))
circles = [Circle(i) for i in range(n)]

count = 0
for _ in range(k):
    d, x, y = map(int, input().split())
    if x > n or y > n or (d == 2 and x == y):
        count += 1
        continue

    x -= 1
    y -= 1

    idx_x = circles[ls[x]].kinds[x]
    idx_y = circles[ls[y]].kinds[y]
    if d == 1:
        if ls[x] != ls[y]:
            circles[ls[x]].join(circles[ls[y]],(idx_y-idx_x)%3)
        else:
            if idx_x != idx_y:
                count += 1
        continue

    if ls[x] != ls[y]:
        circles[ls[x]].join(circles[ls[y]], (idx_y-idx_x-1) % 3)
    elif (idx_y-idx_x-1)%3 != 0:
        count += 1

print(count)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240528105934719](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240528105934719.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

升空的烟火、单调栈、舰队和道路都是比较常规的题目，食物链（特殊的存储技巧）和月度开销（开始甚至不知道怎么二分）比较难。

