# Assignment #4: 排序、栈、队列和树

Updated 0005 GMT+8 March 11, 2024

2024 spring, Complied by ==PoetTam==



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:

Learn about Time complexities, learn the basics of individual Data Structures, learn the basics of Algorithms, and practice Problems.

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：Window 11

Python编程环境：PyCharm 2023.2.1 (Community Edition)



## 1. 题目

### 05902: 双端队列

http://cs101.openjudge.cn/practice/05902/



思路：直接模拟就是。



代码

```python
for _ in range(int(input())):
    deque = []
    for i in range(int(input())):
        operate,s = map(int,input().split())
        if operate == 1: # 入队
            deque.append(s)
        else:
            if s == 0:
                deque.pop(0)
            else:
                deque.pop()
    print(*deque) if deque else print('NULL')
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240314205451739](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240314205451739.png)



### 02694: 波兰表达式

http://cs101.openjudge.cn/practice/02694/



思路：按照波兰表达式的规则重构算式。



代码

```python
set1 = set('+-*/') # 创建一个集合存储运算符号
list_equation = list(input().split())
n = len(list_equation)
i = n-1
list_num = []
while i >= 0:
    if i == 0:
        num1 = list_num.pop()
        num2 = list_num.pop()
        result = num1+list_equation.pop()+num2
        break
    else:
        if list_equation[i] not in set1:
            list_num.append(list_equation.pop())
        else:
            num1 = list_num.pop()
            num2 = list_num.pop()
            list_num.append('('+num1 + list_equation.pop() + num2+')')
        i -= 1
print(f'{eval(result):.6f}')
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240314204702294](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240314204702294.png)



### 24591: 中序表达式转后序表达式

http://cs101.openjudge.cn/practice/24591/



思路：遍历字符串（需要使用re库找出数字），根据后序转中序的规则重构表达式。



代码

```python
import re
pattern = r'^\d+(\.\d+)?'
for _ in range(int(input())):
    s = input()
    stack1 = [] # 存储表达式
    stack2 = [] # 存储括号
    i = 0
    prec = {'(':1,'+':2,'-':2,'*':3,'/':3}
    while i < len(s):
        match = re.match(pattern,s[i:])
        if match:
            str1 = match.group()
            stack1.append(str1)
            i += len(str1)
        else:
            if s[i] == '(':
                stack2.append(s[i])
            elif s[i] == ')':
                token = stack2.pop()
                while token != '(':
                    stack1.append(token)
                    token = stack2.pop()
            else:
                while stack2 and prec[stack2[-1]] >= prec[s[i]]:
                    stack1.append(stack2.pop())
                stack2.append(s[i])
            i += 1
    while stack2:
        stack1.append(stack2.pop())
    print(*stack1)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240314204527142](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240314204527142.png)



### 22068: 合法出栈序列

http://cs101.openjudge.cn/practice/22068/



思路：当栈中元素不为零时，和列表做对比。如果栈顶元素和列表第一个元素相同，说明栈顶元素可出栈。



代码

```python
x = input()
while True:
    try:
        s = input()
        index = 0
        stack = []
        if len(s) != len(x):
            print('NO')
            continue
        for i in x:
            stack.append(i)
            while stack != [] and stack[-1] == s[index]:
                stack.pop()
                index += 1
        print('YES' if len(stack) == 0 else 'NO')
    except:
        break
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240314184746144](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240314184746144.png)



### 06646: 二叉树的深度

http://cs101.openjudge.cn/practice/06646/



思路：定义一个函数直接输出二叉树深度。比较有难度的就是需要用一个列表把每个数字所代表的树存起来。



代码

```python
class BinaryTree:
    def __init__(self,root):
        self.root = root
        self.left_child = None
        self.right_child = None

    def depth(self):
        if not self.root:
            return 0
        elif not self.left_child and not self.right_child:
            return 1
        elif self.left_child and not self.right_child:
            return  self.left_child.depth()+1
        elif not self.left_child and self.right_child:
            return  self.right_child.depth()+1
        else:
            return max(self.left_child.depth(), self.right_child.depth())+1

n = int(input())
tree = BinaryTree(None) if n == 0 else BinaryTree(1)
lst = [0 for i in range(n)]
lst[0] = tree

for i in range(n):
    current_tree = lst[i]
    left,right = map(int,input().split())
    if left != -1:
        current_tree.left_child = BinaryTree(left)
        lst[left-1] = current_tree.left_child
    if right != -1:
        current_tree.right_child = BinaryTree(right)
        lst[right-1] = current_tree.right_child

print(tree.depth())
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240314220942627](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240314220942627.png)



### 02299: Ultra-QuickSort

http://cs101.openjudge.cn/practice/02299/



思路：看了群里大佬的思路。分治求逆序数。前面尝试过直接求，惨遭tle。



代码

```python
while True:
    n = int(input())
    if n == 0:
        break
    lst = []
    times = 0
    for i in range(n):
        lst.append(int(input()))

    def mergesort(lists):
        if len(lists) == 1:
            return lists
        n = len(lists) // 2
        left = mergesort(lists[:n])
        right = mergesort(lists[n:])
        return merge(left, right)

    def merge(left, right):
        global times
        result = []
        l, r = 0, 0
        while l < len(left) and r < len(right):
            if left[l] > right[r]:
                times += len(left) - l
                result.append(right[r])
                r += 1
            else:
                result.append(left[l])
                l += 1
        result += left[l:]
        result += right[r:]
        return result

    new_lst = mergesort(lst)
    print(times)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240314235419791](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240314235419791.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

现在的代码对算法的要求更高了，要求写出一个比较高效的代码。很多时候写出来的都是比较低效的代码然后TLE，最后还是需要改进算法。

以及，global 变量一定需要把变量写在前面，不然CF会报错（但是Pycharm不会）。
