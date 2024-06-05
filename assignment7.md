# Assignment #7: April 月考

Updated 1557 GMT+8 Apr 3, 2024

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

### 27706: 逐词倒放

http://cs101.openjudge.cn/practice/27706/



思路：耗时1min。



代码

```python
s = list(input().split())
print(*s[::-1])
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240403172330679](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240403172330679.png)



### 27951: 机器翻译

http://cs101.openjudge.cn/practice/27951/



思路：按题意操作即可。耗时5min。



代码

```python
m,n = map(int,input().split())
lst = list(map(int,input().split()))
t = 0
queue = []
for i in lst:
    if len(queue) == m:
        if i not in queue:
            t += 1
            queue.pop(0)
            queue.append(i)
    else:
        if i not in queue:
            t += 1
            queue.append(i)
print(t)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240403172419418](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240403172419418.png)



### 27932: Less or Equal

http://cs101.openjudge.cn/practice/27932/



思路：从小到大排序，比较排在第k位和k+1位的值是否相同。耗时30min。



代码

```python
n,k = map(int,input().split())
lst = list(map(int,input().split()))
lst.sort()
if k == 0: print(-1 if lst[0] == 1 else 1)
elif k == n: print(lst[-1])
else:
    m = lst[k]
    n = lst[k-1]
    print(-1 if m == n else n)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240403172252521](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240403172252521.png)



### 27948: FBI树

http://cs101.openjudge.cn/practice/27948/



思路：直接建树就可以了。耗时15min。



代码

```python
def code(s):
    n = len(s)
    if s == '0'*n:
        return 'B'
    elif s == '1'*n:
        return 'I'
    return 'F'
class Node:
    def __init__(self,root):
        self.root = code(root)
        self.left = None
        self.right = None

def construct(s):
    tree = Node(s)
    if len(s) == 1:
        return tree
    n = len(s)//2
    tree.left = construct(s[:n])
    tree.right = construct(s[n:])
    return tree

def postorder(tree):
    if tree.left is None:
        return tree.root
    return postorder(tree.left)+postorder(tree.right)+tree.root
n = int(input())
tree = construct(input())
print(postorder(tree))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240403172512754](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240403172512754.png)



### 27925: 小组队列

http://cs101.openjudge.cn/practice/27925/



思路：存储每组的组员和每个组员所属的小组。在进行插队操作时首先查询该组员所在的小组，然后从后往前查询队列中是否有该小组成员，如果有直接插到该成员之后。耗时25min。



代码

```python
def insert_new(queue,ID,group):
    for i in range(len(queue) - 1, -1, -1):
        if queue[i] in group:
            queue.insert(i+1, ID)
            return
    return queue.append(ID)

dic = {} # 按照每组存储
dic1 = {} # 按照每个人
for i in range(int(input())):
    dic[i] = list(map(int,input().split()))
    for j in dic[i]:
        dic1[j] = i

queue = []
while True:
    s = input().split()
    if len(s) == 1:
        if s[0] == 'STOP':
            break
        else:
            print(queue.pop(0))
    else:
        ID = int(s[1])
        group = dic1[ID]
        insert_new(queue,ID,dic[group])
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240403172550078](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240403172550078.png)



### 27928: 遍历树

http://cs101.openjudge.cn/practice/27928/



思路：每棵树通过parent属性连接。用一个字典存储每一棵树。最后按照子节点的root值和根节点值的顺序递归就可以了。耗时50min。



代码

```python
class Node:
    def __init__(self,root):
        self.root = root
        self.child = []
        self.parent = None

    def get_parent(self):
        if self.parent is None:
            return self
        return self.parent.get_parent()

    def sort(self):
        self.child.sort(key=lambda x:x.root)
        for i in self.child:
            if isinstance(i,Node):
                i.sort()

    def ordered(self,dic):
        if self.child == []:
            print(self.root)
        else:
            lst = list(i.root if isinstance(i, Node) else i for i in self.child) + [self.root]
            lst.sort()
            for i in lst:
                if i == self.root:
                    print(i)
                else:
                    dic[i].ordered(dic)

n = int(input())
dic = {}
for _ in range(n):
    lst = list(map(int,input().split()))
    m = lst.pop(0)
    if m in dic:
        tree = dic[m]
    else:
        tree = Node(m)
        dic[m] = tree
    for i in lst:
        if i in dic:
            tree1 = dic[i]
            tree.child.append(tree1)
            tree1.parent = tree
            dic[i] = tree1
        else:
            tree1 = Node(i)
            tree.child.append(tree1)
            tree1.parent = tree
            dic[i] = tree1

final_tree = tree.get_parent()
final_tree.sort()
final_tree.ordered(dic)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240403172636699](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240403172636699.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

AC5，第三题没有考虑到k==0的情况最后来不及做了。其他题感觉思路上都不难，重点是如何用算法实现。



