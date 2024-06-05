# Assignment #3: March月考

Updated 1537 GMT+8 March 6, 2024

2024 spring, Complied by ==PoetTam==



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:
- Learn about Time and Space complexities
- Learn the basics of individual Data Structures
- Learn the basics of Algorithms
- Practice Problems on DSA

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：Windows 11

Python编程环境：PyCharm 2023.2.1 (Community Edition)



## 1. 题目

**02945: 拦截导弹**

http://cs101.openjudge.cn/practice/02945/



思路：先找出所有导弹的最高高度，暂时设为max1。然后将max1和列表中的每个值进行比较。如果max1大于等于该值，可以选择拦截（max1更新为该值），也可以不拦截（不更新）；如果小于，说明不能拦截该导弹（max1不更新）。



##### 代码

```python
k = int(input())
pun = list(map(int,input().split()))
def dis(pun,max1):
    if max1 >= pun[0]: # 可以选择拦截
        return 1 if len(pun) == 1 else max(dis(pun[1:],max1),dis(pun[1:],pun[0])+1)
    else: # 无法拦截该导弹
        return 0 if len(pun) == 1 else dis(pun[1:], max1)
max1 = max(pun)
print(dis(pun,max1))
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240306221113274](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240306221113274.png)



**04147:汉诺塔问题(Tower of Hanoi)**

http://cs101.openjudge.cn/practice/04147



思路：依旧使用递归。假设共有abc三根柱子，要将所有圆盘从a柱全部转移到c柱，那么应该将除最大圆盘外的所有圆盘全部挪到b柱。因此如果我们忽略最大的圆盘，问题便变成了将所有的圆盘移到b柱。

##### 代码

```python
m,a,b,c = input().split()
m = int(m)
def hanoi(m,a,b,c):
    if m == 1:
        print(f'{m}:{a}->{c}')
    else:
        hanoi(m-1,a,c,b)
        print(f'{m}:{a}->{c}')
        hanoi(m-1,b,a,c)
hanoi(m,a,b,c)
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240306221547550](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240306221547550.png)



**03253: 约瑟夫问题No.2**

http://cs101.openjudge.cn/practice/03253



思路：没什么复杂的，直接使用队列就可以了。只多了一步将每个被淘汰的人存在一个列表中。



##### 代码

```python
while True:
    s = input()
    if s == '0 0 0':
        break
    n,p,m = map(int,s.split())
    quene = list(range(p,n+1))+list(range(1,p))
    quene1 = []
    while len(quene) >= 2:
        for i in range(m):
            if i == m-1:
                quene1.append(quene.pop(0))
                break
            else:
                quene.append(quene.pop(0))
    quene1 += quene
    print(*quene1,sep=',')
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240306221738081](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240306221738081.png)



**21554:排队做实验 (greedy)v0.2**

http://cs101.openjudge.cn/practice/21554



思路：要是平均排队时间最短，其实就是要是实验时间短的人先做实验，长的人后做实验。



##### 代码

```python
n = int(input())
lst = list(map(int,input().split()))
dic = []
for i in range(n):
    dic.append((lst[i],i+1))
dic.sort(key=lambda x:(x[0],x[1]))
order = []
sum_time = 0
person = 1
for i in dic:
    if person < n:
        sum_time += i[0]*(n-person)
        person += 1
    order.append(i[1])
print(*order)
print(f'{sum_time/n:.2f}')
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240306221908233](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240306221908233.png)



**19963:买学区房**

http://cs101.openjudge.cn/practice/19963



思路：将价格存为一个列表，性价比存为一个列表。然后找出每个列表的中位数进行比较。两个条件都符合的时候，num加一即可。此外，可以直接调用python基础库statistics。



##### 代码

```python
import statistics
n = int(input())
distance = list(map(lambda x:sum(eval(x)),input().split()))
prize = list(map(int,input().split()))
average = list(distance[i]/prize[i] for i in range(n))
prize_median = statistics.median(prize)
average_median = statistics.median(average)
num = 0
for i in range(n):
    if average[i] > average_median and prize[i] < prize_median:
        num += 1
print(num)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240306222141177](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240306222141177.png)



**27300: 模型整理**

http://cs101.openjudge.cn/practice/27300



思路：创建一个字典，将不同模型的不同参数量存起来，然后按要求输出即可。不过本人忘了setdefault，就使用了一个笨方法orz



##### 代码

```python
def number_trans(s):
    if s[-1] == 'B':
        return float(s[:-1])*1000
    else:
        return float(s[:-1])

dic = {}
for _ in range(int(input())):
    model,number=input().split('-')
    if model not in dic:
        dic[model] = [number]
    else:
        dic[model].append(number)
for k in sorted(dic.keys()):
    print(k,end=': ')
    num = sorted(dic[k],key=lambda x:number_trans(x))
    print(*num,sep=', ')
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240306222410044](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240306222410044.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

由于之前的计概班好像没怎么要求dp，所以不怎么会写dp，只能用递归的笨方法解决问题。这次月考成功push我去学习dp了，此外dfs，bfs啥的更是没听说过。因此感觉任务量一下子就大起来了。

在完成买学区房的时候，自己根据定义手搓了一个median，惨遭WA，最后才想起statistics库，也算是让我明白了能调库的时候不要硬撑（苦笑）。



