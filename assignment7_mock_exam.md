# Assignment #7: 20250402 Mock Exam

Updated 1624 GMT+8 Apr 2, 2025

2025 spring, Complied by <mark>卢殷文 物院</mark>



> **说明：**
>
> 1. **⽉考**：<mark>AC5</mark> 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。
>
> 2. **解题与记录：**
>
>    对于每一个题目，请提供其解题思路（可选），并附上使用Python或C++编写的源代码（确保已在OpenJudge， Codeforces，LeetCode等平台上获得Accepted）。请将这些信息连同显示“Accepted”的截图一起填写到下方的作业模板中。（推荐使用Typora https://typoraio.cn 进行编辑，当然你也可以选择Word。）无论题目是否已通过，请标明每个题目大致花费的时间。
>
> 3. **提交安排：**提交时，请首先上传PDF格式的文件，并将.md或.doc格式的文件作为附件上传至右侧的“作业评论”区。确保你的Canvas账户有一个清晰可见的头像，提交的文件为PDF格式，并且“作业评论”区包含上传的.md或.doc附件。
>
> 4. **延迟提交：**如果你预计无法在截止日期前提交作业，请提前告知具体原因。这有助于我们了解情况并可能为你提供适当的延期或其他帮助。 
>
> 请按照上述指导认真准备和提交作业，以保证顺利完成课程要求。



## 1. 题目

### E05344:最后的最后

http://cs101.openjudge.cn/practice/05344/



思路：

deque模拟

代码：

```python
from collections import deque
n,k=map(int,input().split())
q=deque([i for i in range(1,n+1)])
ans=[]
while len(q)>1:
    for _ in range(k-1):
        a=q.popleft()
        q.append(a)
    a=q.popleft()
    ans.append(a)
print(*ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250402174437287](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250402174437287.png)



### M02774: 木材加工

binary search, http://cs101.openjudge.cn/practice/02774/



思路：

经典二分

经实验，普通遍历会TLE（

代码：

```python
n,k=map(int,input().split())
l=[int(input()) for i in range(n)]
maxa=sum(l)//k
mina=min(l)//(k//n+1)
while maxa>mina:
    ans = (maxa + mina+1) // 2
    cnt=0
    for wood in l:
        cnt+=wood//ans
    if cnt>=k:
        mina=ans
    else:
        maxa=ans-1
print(mina)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250402174532827](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250402174532827.png)



### M07161:森林的带度数层次序列存储

tree, http://cs101.openjudge.cn/practice/07161/



思路：

就是按部就班建树+后序输出

考试的时候WA了找不到错；一考完立马反应过来，之前代码里while的判定写成了if形式

可惜可惜

代码：

```python
class TreeNode:
    def __init__(self, value, leave_num):
        self.value = value
        self.root = None
        self.leaves= []
        self.leave_num = leave_num

from collections import deque
def make_tree(s):
    l=s.split()
    ToBeDone=deque([])
    root=TreeNode(l[0],int(l[1]))
    due=root
    for i in range(2,len(l),2):
        while len(due.leaves)==due.leave_num:
            due=ToBeDone.popleft()
        v,num=l[i],int(l[i+1])
        cur=TreeNode(v,num)
        due.leaves.append(cur)
        cur.root=due
        ToBeDone.append(cur)
    return root
def root_back(root):
    if root.leave_num==0:
        return [root.value]
    l=[]
    for node in root.leaves:
        l+=root_back(node)
    l.append(root.value)
    return l
n=int(input())
ans=[]
for _ in range(n):
    s=input()
    ans+=root_back(make_tree(s))
print(*ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250402174139729](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250402174139729.png)



### M18156:寻找离目标数最近的两数之和

two pointers, http://cs101.openjudge.cn/practice/18156/



思路：

说实话这题要是不给提示，我还真想不到双指针

代码：

```python
t=int(input())
l=list(map(int,input().split()))
l.sort()
p=0
q=len(l)-1
ans = l[p] + l[q]
while p<q:
    cur=l[p]+l[q]
    if abs(cur-t)<abs(ans-t):
        ans=cur
    elif abs(cur-t)==abs(ans-t):
        if cur<t:
            ans=cur
    if cur>t:
        q-=1
    elif cur<t:
        p+=1
    else:
        break
print(ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250402174712973](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250402174712973.png)



### M18159:个位为 1 的质数个数

sieve, http://cs101.openjudge.cn/practice/18159/



思路：

欧拉筛

代码：

```python
def Eular(r):
    is_prime = [0 for i in range(r+1)]
    common=[]
    for i in range(2,r+1):
        if is_prime[i]==0:
            common.append(i)
        for j in common:
            if i*j>r:
                break
            is_prime[i*j]=1
            if i%j==0:
                break
    return common
l=Eular(10002)
p1=[]
for i in l:
    if i%10==1:
        p1.append(i)
t=int(input())
for j in range(1,t+1):
    n=int(input())
    ans=[]
    for i in p1:
        if i<n:
            ans.append(i)
        else:
            break
    print("Case%d:"%j)
    print(*ans) if ans else print("NULL")
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250402174811040](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250402174811040.png)



### M28127:北大夺冠

hash table, http://cs101.openjudge.cn/practice/28127/



思路：

defaultdict 秒了

代码：

```python
from collections import defaultdict
teams=defaultdict(list)
m=int(input())
for _ in range(m):
    name,question,isAC=input().split(",")
    num=ord(question)-ord('A')
    if name not in teams:
        teams[name]=[0]*27
    teams[name][-1]+=1
    if isAC == "yes":
        teams[name][num]=1
final=[]
for name in teams:
    tries=teams[name][-1]
    teams[name].pop()
    ACs=sum(teams[name])
    final.append([name,ACs,tries])
final.sort(key=lambda x:(-x[1],x[2],x[0]))
for i in range(min(12,len(final))):
    print(i+1,final[i][0],final[i][1],final[i][2])
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20250402174939474](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250402174939474.png)



## 2. 学习总结和收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025spring每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>

这次月考做的还算顺利吧（可能是因为最难只有Medium？）

最近在准备其他科目的期中考，算法这方面的练习要缓一缓了







