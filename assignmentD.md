# Assignment #D: May月考

Updated 1654 GMT+8 May 8, 2024

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

### 02808: 校门外的树

http://cs101.openjudge.cn/practice/02808/



思路：使用一个数表暴力模拟。5min。



代码

```python
L,M=map(int,input().split())
lst = [1]*(L+1)
for _ in range(M):
    l,r = map(int,input().split())
    lst[l:r+1] = [0]*(r-l+1)
print(sum(lst))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240509194127348](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240509194127348.png)



### 20449: 是否被5整除

http://cs101.openjudge.cn/practice/20449/



思路：使用一个count记录当前的数字，一个字符串s1记录最终输出结果。然后遍历s，更新count和s1。耗时5min。



代码

```python
s = input()
s1 = ''
count = 0
for i in s:
    count = count*2+int(i)
    s1 += '1' if count%5 == 0 else '0'
print(s1)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240509194105631](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240509194105631.png)



### 01258: Agri-Net

http://cs101.openjudge.cn/practice/01258/



思路：Prim算法。卡在没有多到多个样例输入。耗时30min。



代码

```python
while True:
    try:
        n = int(input())
    except EOFError:
        break

    lst = [list(map(int, input().split())) for _ in range(n)]
    selected = set()
    selected.add(0)
    unselected = set(range(1, n))
    dist_sum = 0
    while unselected:
        min_dist = float('inf')
        end_vertex = None
        for i in selected:
            for j in unselected:
                if min_dist > lst[i][j]:
                    min_dist = lst[i][j]
                    end_vertex = j
        dist_sum += min_dist
        selected.add(end_vertex)
        unselected.remove(end_vertex)
    print(dist_sum)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240509194018537](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240509194018537.png)



### 27635: 判断无向图是否连通有无回路(同23163)

http://cs101.openjudge.cn/practice/27635/



思路：判断是否连通：随意选取一个节点，对该节点进行bfs。如果遍历后发现已经遍历的节点数小于总节点数，说明没有联通。

判断是否有回路：需要遍历每一个节点。对每一个节点记录prev，如果发现一个节点已经被遍历过且当前节点的prev不为该节点，说明出现了回路。



代码

```python
vertex_num,edge_num = map(int,input().split())
graph = {i:[] for i in range(vertex_num)}
for _ in range(edge_num):
    u,v = map(int,input().split())
    graph[u].append(v)
    graph[v].append(u)

vis = {0}
visitedVertex = set()
queue = [0]
while queue:
    current = queue.pop(0)
    if current not in visitedVertex:
        for new in graph[current]:
            if new not in vis:
                vis.add(new)
                queue.append(new)
        visitedVertex.add(current)
print('connected:yes' if len(vis) == vertex_num else 'connected:no')

# has a loop
flag = False
i = 0
while not flag and i < vertex_num:
    vis, visitedVertex = {i}, set()
    prev = [None] * vertex_num
    queue = [i]
    while queue:
        current = queue.pop(0)
        if current not in visitedVertex:
            while graph[current]:
                new = graph[current].pop(0)
                if new not in vis:
                    prev[new] = current
                    vis.add(new)
                    queue.append(new)
                else:
                    if prev[current] != new:
                        flag = True
                        break
            visitedVertex.add(current)
        if flag: break
    i += 1
print('loop:yes' if flag else 'loop:no')
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240509193943236](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240509193943236.png)





### 27947: 动态中位数

http://cs101.openjudge.cn/practice/27947/



思路：思路是使用left记录中位数左边的数据，right记录中位数右边的数据，median_now为当前中位数。

推入规则为：遇到小于等于median_now的数据，则将其相反数推入left，反之推入right（使用heapq.heappush()），这样保证了left中最大值的相反数、right中最小值在堆顶。

题目要求依次读入一个整数序列，每当已经读入的整数个数为奇数时，输出已读入的整数构成的序列的中位数。

则每当已经读入的整数个数为奇数时，left和right的长度有三种情况：

（1）left比right长，说明中位数在left中，则将median_now推入right，并更新median_now为-heapq.heappop(left)。

（2）left比right短，说明中位数在right中，则将median_now推入left，并更新median_now为heapq.heappop(right)。

（3）长度相等，不需要更新median_now。

然后将median_now加入median_lst中。

耗时1h。



代码

```python
import heapq
for _ in range(int(input())):
    lst = list(map(int,input().split()))
    median_now = lst[0]
    left = []
    right = []
    median_lst = [lst[0]]
    for i in range(1,len(lst)):
        if lst[i] <= median_now:
            heapq.heappush(left, -lst[i])
        else:
            heapq.heappush(right, lst[i])

        if (i+1)%2 == 1:
            if len(left) > len(right):
                heapq.heappush(right,median_now)
                median_now = -heapq.heappop(left)
            elif len(left) < len(right):
                heapq.heappush(left,-median_now)
                median_now = heapq.heappop(right)
            median_lst.append(median_now)

    print(len(median_lst))
    print(*median_lst)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240509195334121](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240509195334121.png)



### 28190: 奶牛排队

http://cs101.openjudge.cn/practice/28190/



思路：使用了原作者的题解方法。找到最大值和最小值的位置。如果最小值在最大值左边，合法，更新ans，然后计算数组左边和后边的结果；如果最小值在最大值右边，不合法，则计算[,min_ind],(min_ind,max_ind),[max_ind]的结果。本质是一种递归方法。

单调栈也尝试过，但感觉要复杂一点。



代码

```python
ans = 0
def max_cow_len(cows):
    global ans
    if len(cows) <= 1:
        return

    min_cow,min_ind = cows[0],0
    max_cow,max_ind = cows[0],0
    for (i,x) in enumerate(cows):
        if x <= min_cow:
            min_cow,min_ind = x,i
        if x > max_cow:
            max_cow,max_ind = x,i

    if max_ind < min_ind:
        max_cow_len(cows[:max_ind+1])
        max_cow_len(cows[min_ind:])
        max_cow_len(cows[max_ind+1:min_ind])
    elif max_ind == min_ind:
        max_cow_len(cows[:max_ind+1])
        max_cow_len(cows[min_ind:])
    else:
        ans = max(ans,max_ind-min_ind+1)
        max_cow_len(cows[:min_ind])
        max_cow_len(cows[max_ind+1:])

N = int(input())
cows = list(int(input()) for _ in range(N))

max_cow_len(cows)
print(ans)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240515002617767](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240515002617767.png)

## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

机考时T2数据量比较小所以当时的代码可以通过，但是作业里面数据史诗级加强后就不太好做了，所以参考了这道题原作者的题解。前面四道题比较常规，T1又自己想了想，感觉思路清晰了不难。

还是要多练单调栈。



