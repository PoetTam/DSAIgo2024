# Assignment #5: "树"算：概念、表示、解析、遍历

Updated 2124 GMT+8 March 17, 2024

2024 spring, Complied by ==PoetTam==



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:

Learn about Time complexities, learn the basics of individual Data Structures, learn the basics of Algorithms, and practice Problems.

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：Windows 11

Python编程环境：PyCharm 2023.2.1 (Community Edition)



## 1. 题目

### 27638: 求二叉树的高度和叶子数目

http://cs101.openjudge.cn/practice/27638/



思路：建立二叉树，然后输出高度和叶子数目。



代码

```python
class BinaryTree:
    def __init__(self,root):
        self.root = root
        self.left_child = None
        self.right_child = None
        self.parent = None

    def get_parent(self):
        return self if not self.parent else self.parent.get_parent()

    def depth(self):
        if isinstance(self.left_child,BinaryTree):
            depth1 = self.left_child.depth()
        else:
            depth1 = 0
        if isinstance(self.right_child,BinaryTree):
            depth2 = self.right_child.depth()
        else:
            depth2 = 0
        return max(depth1,depth2)+1 if self.root is not None else max(depth1,depth2)

    def leaves(self):
        if isinstance(self.left_child,BinaryTree):
            leaves1 = self.left_child.leaves()
        else:
            leaves1 = 0
        if isinstance(self.right_child,BinaryTree):
            leaves2 = self.right_child.leaves()
        else:
            leaves2 = 0
        if leaves1 == leaves2 == 0:
            return 1
        else:
            return leaves1+leaves2

n = int(input())
lst = [BinaryTree(None) for i in range(n)]
for i in range(n):
    current_tree = lst[i]
    left,right = map(int,input().split())
    if left != -1:
        lst[left].root = left
        lst[left].parent = current_tree
        current_tree.left_child = lst[left]
    if right != -1:
        lst[right].root = right
        lst[right].parent = current_tree
        current_tree.right_child = lst[right]

tree = lst[0].get_parent()
print(tree.depth(),tree.leaves())
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240318151004668](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240318151004668.png)



### 24729: 括号嵌套树

http://cs101.openjudge.cn/practice/24729/



思路：遍历字符串建立树，然后前序遍历、后序遍历树。



代码

```python
class Tree:
    def __init__(self,root):
        self.root = root
        self.node = []
        self.parent = None

    def insert_node(self,new_node):
        self.node.append(new_node)

def preorder(tree):
    s = ''
    for i in tree.node:
        s += preorder(i) if isinstance(i,Tree) else i
    return tree.root+s

def postorder(tree):
    s = ''
    for i in tree.node:
        s += postorder(i) if isinstance(i,Tree) else i
    return s+tree.root

s = input()
tree = Tree(s[0])
stack = [tree]
current = tree
for i in s[1:]:
    if i == '(':
        current = current.node[-1] if current.node != [] else current
    elif i == ')':
        current = current.parent
    elif i == ',':
        continue
    else:
        new_tree = Tree(i)
        new_tree.parent = current
        current.insert_node(new_tree)

print(preorder(tree))
print(postorder(tree))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240318171253690](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240318171253690.png)



### 02775: 文件结构“图”

http://cs101.openjudge.cn/practice/02775/



思路：建立一个树，然后打印列表。



代码

```python
class Tree:
    def __init__(self,root):
        self.root = root
        self.node = []
        self.parent = None

    def insert_node(self,new_node):
        self.node.append(new_node)

def output(tree,t):
    for i in tree.node:
        if isinstance(i,Tree):
            print(ss*t+i.root)
            output(i,t+1)
    lst = list(i for i in tree.node if isinstance(i,str))
    lst.sort()
    for i in lst:
        print(ss*(t-1)+i)

times = 0
while True:
    s = input()
    if s == '#':
        break
    if times != 0:
        print('')
    times += 1
    print(f'DATA SET {times}:')
    tree = Tree('ROOT')
    if s[0] == 'f':
        tree.insert_node(s)
        current = tree
    else:
        new_tree = Tree(s)
        new_tree.parent = tree
        tree.insert_node(new_tree)
        current = new_tree

    while True:
        s1 = input()
        if s1 == '*':
            break
        else:
            if s1[0] == 'f':
                current.insert_node(s1)
            elif s1 == ']':
                current = current.parent
            else:
                new_tree = Tree(s1)
                new_tree.parent = current
                current.insert_node(new_tree)
                current = new_tree

    ss = '|' + ' ' * 5
    print('ROOT')
    output(tree,1)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240318175004466](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240318175004466.png)



### 25140: 根据后序表达式建立队列表达式

http://cs101.openjudge.cn/practice/25140/



思路：建立二叉树，然后层次遍历。



代码

```python
class BinaryTree:
    def __init__(self,root):
        self.root = root
        self.left_child = None
        self.right_child = None

def layerorder(tree):
    queue = [tree]
    s = ''
    while queue:
        current = queue.pop(0)
        if isinstance(current,BinaryTree):
            s += current.root
            queue.append(current.left_child)
            queue.append(current.right_child)
        else:
            s += current
    return s

for _ in range(int(input())):
    s1 = input()
    n = len(s1)
    tree = BinaryTree(s1[-1])
    current = tree
    stack = []
    for i in s1[-2::-1]:
        if i.islower():
            if current.right_child is None:
                current.right_child = i
            else:
                current.left_child = i
                while stack:
                    current = stack.pop()
                    if current.left_child is None:
                        break

        else:
            if current.right_child is None:
                new_tree = BinaryTree(i)
                current.right_child = new_tree
                stack.append(current)
                current = new_tree
            else:
                new_tree = BinaryTree(i)
                current.left_child = new_tree
                stack.append(current)
                current = new_tree
    print(layerorder(tree)[::-1])
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240319113753981](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240319113753981.png)



### 24750: 根据二叉树中后序序列建树

http://cs101.openjudge.cn/practice/24750/



思路：拆分出left_child、root和right_child，然后根据递归建立二叉树，最后前序遍历。

后序遍历最后一个元素为根节点，可以找出中序遍历中根节点的位置，从而把后序遍历中序遍历的左子节、根节点、右子节分开，在分别对左子节和右子节进行递归比较。



代码

```python
class BinaryTree:
    def __init__(self,root):
        self.root = root
        self.left_child = None
        self.right_child = None

def post_in(s1,s2):
    # 拆分出left_child、root和right_child
    length = len(s1)
    n = s1.find(s2[-1]) # 根节点索引

    tree = BinaryTree(s1[n])
    tree.left_child = post_in(s1[:n],s2[:n]) if len(s1[:n]) >= 2 else s1[:n]
    tree.right_child = post_in(s1[n+1:],s2[n:length-1]) if len(s1[n+1:]) >= 2 else s1[n+1:]

    return tree

def preorder(self):
    s = ''
    if isinstance(self.left_child,BinaryTree):
        s += preorder(self.left_child)
    else:
        s += self.left_child

    if isinstance(self.right_child,BinaryTree):
        s += preorder(self.right_child)
    else:
        s += self.right_child
    return self.root+s

tree = post_in(input(),input())
print(preorder(tree))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240319105324173](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240319105324173.png)



### 22158: 根据二叉树前中序序列建树

http://cs101.openjudge.cn/practice/22158/



思路：拆分出left_child、root和right_child，然后根据递归建立二叉树，最后后序遍历。

前序遍历第一个元素为根节点，可以找出中序遍历中根节点的位置，从而把前序遍历中序遍历的左子节、根节点、右子节分开，在分别对左子节和右子节进行递归比较。



代码

```python
class BinaryTree:
    def __init__(self,root):
        self.root = root
        self.left_child = None
        self.right_child = None

def post_in(s1,s2):
    # 拆分出left_child、root和right_child
    length = len(s1)
    n = s2.find(s1[0]) # 根节点索引

    tree = BinaryTree(s1[0])
    tree.left_child = post_in(s1[1:n+1],s2[:n]) if len(s2[:n]) >= 2 else s2[:n]
    tree.right_child = post_in(s1[n+1:],s2[n+1:]) if len(s2[n+1:]) >= 2 else s2[n+1:]

    return tree

def postorder(self):
    s = ''
    if isinstance(self.left_child,BinaryTree):
        s += postorder(self.left_child)
    else:
        s += self.left_child

    if isinstance(self.right_child,BinaryTree):
        s += postorder(self.right_child)
    else:
        s += self.right_child
    return s+self.root

while True:
    try:
        s1 = input()
        s2 = input()
        tree = post_in(s1, s2)
        print(postorder(tree))
    except EOFError:
        break
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240319105919216](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240319105919216.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

不会建树泪流满行，如果遍历需要思考。前序遍历后序遍历中序遍历的联系需理清。

树的精髓在于复用。



