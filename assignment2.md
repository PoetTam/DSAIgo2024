# Assignment #2: 编程练习

Updated 0953 GMT+8 Feb 24, 2024

2024 spring, Complied by ==PoetTam==



**说明：**

1）The complete process to learn DSA from scratch can be broken into 4 parts:
- Learn about Time and Space complexities
- Learn the basics of individual Data Structures
- Learn the basics of Algorithms
- Practice Problems on DSA

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知3月1日导入选课名单后启用。**作业写好后，保留在自己手中，待3月1日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



**编程环境**

==（请改为同学的操作系统、编程环境等）==

操作系统：Windows 11

Python编程环境：PyCharm 2023.2.1 (Professional Edition)



## 1. 题目

### 27653: Fraction类

http://cs101.openjudge.cn/2024sp_routine/27653/



思路：定义一个类来存储分数形式的数据，并为该类定义add方法和输出方法。



##### 代码

```python
def gcd(m,n):
    if m < n:m,n = n,m
    while m % n != 0:
        m, n = n, m%n
    return n

class Fraction:
    def __init__(self,top,bottom):
        self.num = top
        self.den = bottom

    def print(self):
        print(f'{self.num}/{self.den}')

    def add(self,otherFraction):
        num = self.num * otherFraction.den + self.den * otherFraction.num
        den = self.den * otherFraction.den
        cmmn = gcd(num,den)
        return Fraction(num//cmmn,den//cmmn)

num1, den1, num2, den2 = map(int,input().split())
Fraction1 = Fraction(num1,den1)
Fraction2 = Fraction(num2,den2)

Fraction1.add(Fraction2).print()
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240301223656022](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240301223656022.png)



### 04110: 圣诞老人的礼物-Santa Clau’s Gifts

greedy/dp, http://cs101.openjudge.cn/practice/04110



思路：使用贪心算法，每次尽量平均价值最高的糖果。



##### 代码

```python
n,w=map(int,input().split())
lst = []
for _ in range(n):
    value,num=map(int,input().split())
    dic = {"value":value,"num":num,"average":value/num}
    lst.append(dic)
lst1 = sorted(lst,key=lambda x:x["average"],reverse = True)
i = 0
num = w
sums = 0
while i < n:
    if num >= lst1[i]["num"]:
        sums += lst1[i]["value"]
        num -= lst1[i]["num"]
        i += 1
    else:
        sums += lst1[i]["average"]*num
        break
print(f"{sums:.1f}")
```



代码运行截图 ==（至少包含有"Accepted"）==

![image-20240301223939279](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240301223939279.png)



### 18182: 打怪兽

implementation/sortings/data structures, http://cs101.openjudge.cn/practice/18182/



思路：每次战斗过程中都尽量选择伤害最高的几次攻击。



##### 代码

```python
for _ in range(int(input())):
    n,m,b = map(int,input().split())
    lst = []
    for _ in range(n):
        t,x = map(int,input().split())
        lst.append((t,x))
    lst.sort(key=lambda x:(x[0],-x[1]))
    dic = {}
    for x in lst:
        if x[0] not in dic:
            dic[x[0]] = [x[1]]
        else:
            dic[x[0]].append(x[1])
    for y in dic:
        b -= sum(dic[y][:m])
        if b <= 0:
            print(y)
            break

    if b > 0:
        print('alive')
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240301224125062](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240301224125062.png)



### 230B. T-primes

binary search/implementation/math/number theory, 1300, http://codeforces.com/problemset/problem/230/B



思路：T，即素数的平方。此问题的关键是判断一个数是不是素数的平方。因此先写出一个列表来存储index是不是素数。为了减小复杂度，选择素数筛法。



##### 代码

```python
n = int(input())
lst = list(map(int,input().split()))
max_num = max(lst)
length = int(max_num**0.5)+1
is_prime = [True]*length
is_prime[0] = is_prime[1] = False
for i in range(2,length):
    if is_prime[i]:
        for j in range(i*i,length,i):
            is_prime[j] = False

for i in lst:
    if int(i**0.5) == i**0.5 and is_prime[int(i**0.5)]:
        print('YES')
    else:
        print('NO')
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240301230607816](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240301230607816.png)



### 1364A. XXXXX

brute force/data structures/number theory/two pointers, 1200, https://codeforces.com/problemset/problem/1364/A



思路：对输入的n个数进行取模。如果每个数字都是x的倍数，不存在最长子序列使得子序列和不能被x整除。如果取模得到的列表和不能被x整除，说明最长子序列就是原序列；如果可以被整除，由于前面已经排除了全部都是x的倍数的情况，则说明必有两个或以上的数字不能被x整除，只需要找到第一个left和最后一个righ，然后max_length = max(right,n-left-1)就可以找到最长子序列长度。

##### 代码

```python
for _ in range(int(input())):
    n, x = map(int, input().split())
    array = list(map(int, input().split()))
    array1 = list(i%x for i in array)

    if array1 == [0 for i in range(n)]:
        max_length = -1
    elif sum(array1)%x != 0:
        max_length = n
    else:
        for i in range(n-1,-1,-1):
            if array1[i] != 0:
                right = i
                break
        for i in range(n):
            if array1[i] != 0:
                left = i
                break
        max_length = max(right,n-left-1)
    print(max_length)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240302200900649](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240302200900649.png)



### 18176: 2050年成绩计算

http://cs101.openjudge.cn/practice/18176/



思路：t-prime是素数的平方。此问题的关键是判断一个数是不是素数的平方。因此先写出一个列表来存储index是不是素数。为了减小复杂度，选择素数筛法。



##### 代码

```python
m,n = map(int,input().split())

is_prime = [True]*(10**4+1)
is_prime[0] = is_prime[1] = False
for i in range(2,10**4+1):
    if is_prime[i]:
        for j in range(i*i,10**4+1,i):
            is_prime[j] = False

for _ in range(m):
    lst1 = list(map(int,input().split()))
    s = len(lst1)
    lst = list(i for i in lst1 if i**0.5 == int(i**0.5) and is_prime[int(i**0.5)])
    if lst == []:
        print(0)
    else:
        print(f'{sum(lst)/s:.2f}')
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240301224441403](C:\Users\tlsqlink\AppData\Roaming\Typora\typora-user-images\image-20240301224441403.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

在完成xxxxx题目时，我原本想的是使用穷举法举出每一个子序列，不过这样的方法时间复杂度太大time out。于是我又尝试使用双指针，不过这样的方法会在n=2，x=8，array=[0,1,0,1,0,1,0,1]时出现错误。最后我才想到了取模的方法。有些时候，找到一个最优算法真的很难。

此外，我也刷完了到3月3日为止的2024spring每日选做，大部分题目都还比较顺畅。只有在完成合法出栈时忽略了长度不相等的情况，成功多次WA，希望以后在编程时能够多多注意到一些可能被忽略的情况orz。

关于排序算法的题目我还没有怎么看，打算下周上课前全部看完（进度有点慢，磕头）。当然现在更担心的是树，感觉树好难啊，希望到时候也能尽量顺利地学会TAT。好像碎碎念有点多bushi。anyway，数算的课程还是很有意思的，也希望能有更多的习题可以做！
