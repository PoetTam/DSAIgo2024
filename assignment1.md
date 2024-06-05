# Assignment #1: 拉齐大家Python水平

Updated 0940 GMT+8 Feb 19, 2024

2024 spring, Complied by ==PoetTam==



**说明：**

1）数算课程的先修课是计概，由于计概学习中可能使用了不同的编程语言，而数算课程要求Python语言，因此第一周作业练习Python编程。如果有同学坚持使用C/C++，也可以，但是建议也要会Python语言。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知3月1日导入选课名单后启用。**作业写好后，保留在自己手中，待3月1日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：Windows 11

Python编程环境：PyCharm 2023.2.1 (Community Edition)



## 1. 题目

### 20742: 泰波拿契數

http://cs101.openjudge.cn/practice/20742/



思路：

创建一个列表，用列表存储泰波拿契数，最后输出结果。

因为题目简单，花费时间不到五分钟。

##### 代码

```python
def tabonacci(n):
    tabo_list = [0,1,1]
    if n <= 2:
        return tabo_list[n]
    else:
        for i in range(n-2):
            tabo_list.append(sum(tabo_list[-3:]))
        return tabo_list[n]

n = int(input())
print(tabonacci(n))
```



代码运行截图 ==（至少包含有"Accepted"）==

![img_12.png](img_12.png)



### 58A. Chat room

greedy/strings, 1000, http://codeforces.com/problemset/problem/58/A



思路：

用列表存储‘olleh’，遍历输入的字符串，如果i与列表最后一个元素相同时，删除列表中最后一个元素。最后根据列表是否为空输出结果。

最终花费时间不超过5min。

##### 代码

```python
def hello(s):
    lst = list('olleh')
    for i in s:
        if lst != [] and i == lst[-1]:
            lst.pop()
    return 'YES' if lst == [] else 'NO'

print(hello(input()))
```



代码运行截图 ==（至少包含有"Accepted"）==

![img_10.png](img_10.png)



### 118A. String Task

implementation/strings, 1000, http://codeforces.com/problemset/problem/118/A



思路：

首先将字符串转换为全小写，然后遍历字符串，将辅音字母按格式拼接到字符串中，最后输出字符串。

最终花费时间不超过5min。

##### 代码

```python
def only_consonant(s):
    s1 = s.lower()
    vowel_list = ['a','o','e','i','u','y']

    consonant_list = ''
    for i in s1:
        if i not in vowel_list:
            consonant_list += '.'+i

    return consonant_list

print(only_consonant(input()))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![img_11.png](img_11.png)



### 22359: Goldbach Conjecture

http://cs101.openjudge.cn/practice/22359/



思路：

已知素数中只有2为偶数。因此我们可以根据和的奇偶性来简化思路。

当和为奇数时，其中一个必为2；当和为偶数时，则需要我们使用循环来判断结果。

最终花费时间不超过5min。

##### 代码

```python
def is_prime(n):
    for i in range(2,int(n**0.5)+1):
        if n % i == 0:
            return False
    return True

def two_prime(n):
    if n % 2 == 1:
        return '2 '+str(n-2)
    else:
        for i in range(3,n//2):
            if is_prime(i) and is_prime(n-i):
                return str(i)+' '+str(n-i)

print(two_prime(int(input())))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![img_9.png](img_9.png)



### 23563: 多项式时间复杂度

http://cs101.openjudge.cn/practice/23563/



思路：

首先对字符串进行split操作，将每一项分割出来。然后使用变量max_complexity存储最大复杂度。

遍历每一项，当常数项不为0且复杂度大于等于max_complexity时，将值赋给max_complexity。最后按照格式输出。

花费时间15~20min。

##### 代码

```python
def complexity(s):
    alist = list(s.split('+'))
    max_comlexity = 0
    for i in alist:
        s1,s2 = i.split('n^')
        if int(s2) >= max_comlexity and s1 != '0':
            max_comlexity = int(s2)
    return 'n^'+str(max_comlexity)

print(complexity(input()))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![img_8.png](img_8.png)



### 24684: 直播计票

http://cs101.openjudge.cn/practice/24684/



思路：

使用字典存储每个选项出现的次数，并得到最大出现次数。将出现次数等于最大出现次数的选项存储在列表中并进行重排序，最后输出结果。

花费时间10~15min。

##### 代码

```python
def max_vote(s):
    lst = list(s.split())
    dic = {}
    for i in lst:
        dic[i] = 1 if i not in dic else dic[i]+1
    max_value = max(dic.values())
    lst2 = list(int(i) for i in dic if dic[i] == max_value)
    lst2.sort()
    return ' '.join(str(x) for x in lst2)

print(max_vote(input()))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![img_7.png](img_7.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“数算pre每日选做”、CF、LeetCode、洛谷等网站题目。==

在完成多项式时间复杂度时，出现了两个错误。一是我忽略了常数项为1时，可以省去；而是直接使用字符串比较max_complexity和当前复杂度也会出现错误（例如： ‘10’和‘3’的比较）。最后在改掉这些bug后我才完成了我的程序。

