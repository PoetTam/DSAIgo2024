# Assignment #6: "树"算：Huffman,BinHeap,BST,AVL,DisjointSet

Updated 2214 GMT+8 March 24, 2024

2024 spring, Complied by ==PoetTam==



**说明：**

1）这次作业内容不简单，耗时长的话直接参考题解。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：Windows 11

Python编程环境：PyCharm 2023.2.1 (Community Edition)



## 1. 题目

### 22275: 二叉搜索树的遍历

http://cs101.openjudge.cn/practice/22275/



思路：递归建立二叉搜索树，然后遍历。耗时10min。



代码

```python
n = int(input())
lst = list(map(int,input().split()))
def Binary_search(lst):
    if len(lst) <= 1:
        return lst
    else:
        m = lst[0]
        lst1 = list(i for i in lst[1:] if i <= m)
        lst2 = lst[len(lst1)+1:]
        return Binary_search(lst1)+Binary_search(lst2)+[m]

print(*Binary_search(lst))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240327152938426](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240327152938426.png)



### 05455: 二叉搜索树的层次遍历

http://cs101.openjudge.cn/practice/05455/



思路：递归建立二叉搜索树，然后遍历。耗时10min。



代码

```python
class BinarySearchTree:
    # 这是一个二叉搜索树
    def __init__(self,root):
        self.root = root
        self.left_child = None
        self.right_child = None

    def layer_order(self):
        list_tree = [self]
        list_layer = []
        while list_tree:
            current = list_tree.pop(0)
            if isinstance(current, BinarySearchTree):
                list_layer.append(current.root)
                list_tree.append(current.left_child)
                list_tree.append(current.right_child)
            else:
                if current is not None:
                    list_layer.append(current)
        return(list_layer)

lst = list(map(int,input().split()))
def Binary_search(lst):
    if len(lst) == 1:
        return BinarySearchTree(lst[0])
    else:
        m = lst[0]
        lst1 = list(i for i in lst[1:] if i < m)
        lst2 = list(i for i in lst[1:] if i > m)
        tree = BinarySearchTree(m)
        if lst1:
            tree.left_child = Binary_search(lst1)
        if lst2:
            tree.right_child = Binary_search(lst2)
        return tree

tree = Binary_search(lst)
print(*tree.layer_order())
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240327161458715](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240327161458715.png)



### 04078: 实现堆结构

http://cs101.openjudge.cn/practice/04078/

练习自己写个BinHeap。当然机考时候，如果遇到这样题目，直接import heapq。手搓栈、队列、堆、AVL等，考试前需要搓个遍。



思路：我是使用的二分法找到元素应该插入的位置，使得列表顺序保持从大到小的顺序。耗时15min。



代码

```python
def find_where(a:list,b:int):
    if a == []:
        return 0
    elif len(a) == 1:
        if a[0] >= b:
            return 1
        else:
            return 0
    else:
        n = len(a)//2
        if a[n] == b:
            return n
        elif b > a[n]:
            return find_where(a[:n],b)
        else:
            return n+find_where(a[n:],b)

lst1 = []
for _ in range(int(input())):
    lst = list(map(int,input().split()))
    if lst[0] == 1:
        n = find_where(lst1,lst[1])
        lst1.insert(n,lst[1])
    else:
        print(lst1.pop())
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240327170055793](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240327170055793.png)



### 22161: 哈夫曼编码树

http://cs101.openjudge.cn/practice/22161/



思路：首先是建立huffmanTree，然后根据huffmanTree对字母进行编码。01串解码参考了群里大佬的代码。耗时50min。



代码

```python
class Node:
    def __init__(self,freq,char,left,right):
        self.freq = freq
        self.char = char
        self.left = left
        self.right = right

def building_huffman(a:list):
    while lst:
        if len(lst) == 1:
            return lst.pop()
        else:
            lst.sort(key= lambda x:(x.freq,x.char))
            left_node = lst.pop(0)
            right_node = lst.pop(0)
            if left_node.char < right_node.char:
                idx = left_node.char+right_node.char
            else:
                idx = right_node.char+left_node.char
            new_tree = Node(left_node.freq+right_node.freq,idx,left_node,right_node)
            lst.append(new_tree)

lst = []
n = int(input())
for _ in range(n):
    char,freq = input().split()
    freq = int(freq)
    lst.append(Node(freq,char,None,None))

tree = building_huffman(lst)
dic = {}
def get_code(tree,a):
    global dic
    if tree.left is None and tree.right is None:
        dic[tree.char] = a
        dic[a] = tree.char
    else:
        if isinstance(tree.left,Node):
            get_code(tree.left,a+'0')
        if isinstance(tree.right,Node):
            get_code(tree.right,a+'1')
# 获取编码表
get_code(tree,'')
while True:
    try:
        s = input()
        if s.isdigit():
            a = b = ''
            for c in s:
                a += c
                try:
                    b += dic[a]
                    a = ''
                except KeyError:
                    continue
            print(b)
        else:
            s1 = ''
            for i in s:
                s1+=dic[i]
            print(s1)
    except EOFError:
        break
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240327193704237](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240327193704237.png)



### 晴问9.5: 平衡二叉树的建立

https://sunnywhy.com/sfbj/9/5/359



思路：平衡二叉树好难TAT。在B站上找了原理部分又看了一遍，然后自己手搓了一下，参考了题解。耗时3h。



代码

```python
class AVL:
    def __init__(self,root):
        self.root = root
        self.left = None
        self.right = None
        self.height = 1

    def _get_height(self,node):
        if node is None:
            return 0
        return max(self._get_height(node.left),self._get_height(node.right))+1

    def _get_balance(self,node):
        if node is None:
            return 0
        return self._get_height(node.left)-self._get_height(node.right)

    def _LL(self,y):
        z = y.left
        t = z.right
        z.right = y
        y.left = t

        y.height = 1 + max(self._get_height(y.left), self._get_height(y.right))
        z.height = 1 + max(self._get_height(z.left), self._get_height(z.right))
        return z

    def  _RR(self,y):
        x = y.right
        t = x.left
        x.left = y
        y.right = t

        y.height = 1 + max(self._get_height(y.left), self._get_height(y.right))
        x.height = 1 + max(self._get_height(x.left), self._get_height(x.right))
        return x

    def insert(self,tree,new_node):
        if tree.root is None:
            tree.root = new_node
            return tree
        else:
            if tree.root > new_node:
                if not isinstance(tree.left,AVL):
                    tree.left = AVL(tree.left)
                tree.left = self.insert(tree.left,new_node)
            else:
                if not isinstance(tree.right,AVL):
                    tree.right = AVL(tree.right)
                tree.right = self.insert(tree.right,new_node)

        tree.height = self._get_height(tree)
        balance = self._get_balance(tree)

        if balance > 1:
            if tree.left.root > new_node: # LL
                tree = self._LL(tree)
            else: # LR
                tree.left = self._RR(tree.left)
                tree = self._LL(tree)
        elif balance < -1:
            if tree.right.root > new_node: # RL
                tree.right = self._LL(tree.right)
                tree = self._RR(tree)
            else: # RR
                tree = self._RR(tree)

        return tree

    def preorder(self,node):
        if node is None:
            return []
        elif isinstance(node,int):
            return [node]
        else:
            return self.preorder(node.root)+self.preorder(node.left)+self.preorder(node.right)

n = int(input())
sequence = list(map(int,input().split()))

avl = AVL(None)
for i in sequence:
    avl = avl.insert(avl,i)
print(*avl.preorder(avl))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

<img src="C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240401104401222.png" alt="image-20240401104401222" style="zoom:50%;" />



### 02524: 宗教信仰

http://cs101.openjudge.cn/practice/02524/



思路：建造并查集。尝试使用集合结果，WA。耗时1h30min。



代码

```python
class Node:
    def __init__(self,size):
        self.fa = list(range(size))

    def find(self,x):
        root = x
        while root != self.fa[root]:
            root = self.fa[root]
        return self.fa[root]

    def union(self,index1,index2):
        self.fa[self.find(index1)] = self.find(index2)
t = 0
while True:
    n,m = map(int,input().split())
    if n == m == 0:
        break

    disjoint = Node(n)
    for _ in range(m):
        i,j = map(int,input().split())
        disjoint.union(i-1,j-1)

    t += 1
    set1 = set(disjoint.find(x) for x in range(n))
    print(f'Case {t}: {len(set1)}')
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240401131606350](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240401131606350.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

AVL一生之敌。学习了在定义方法时可以不对self进行修改，而是对输入的数据进行修改然后返回的方法。同时根据群里大佬的代码学习了try的新用法，之前一直只是在数据输入时使用。这周作业做得很煎熬，希望之后可以更快完成作业。
