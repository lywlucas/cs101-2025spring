# Assignment #3: 惊蛰 Mock Exam

Updated 1641 GMT+8 Mar 5, 2025

2025 spring, Complied by <mark>卢殷文 物院</mark>



> **说明：**
>
> 1. **惊蛰⽉考**：AC4 /(ㄒoㄒ)/~~<mark>（请改为同学的通过数）</mark> 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。
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

### E04015: 邮箱验证

strings, http://cs101.openjudge.cn/practice/04015



思路：简单，按题目依次判定即可



代码：

```python
def is_allowed(s):
    l=list(s.split('@'))
    if len(l)!=2:
        return False
    a,b=l[0],l[1]
    if a=='' or b=='':
        return False
    if a[0]=='.' or b[0]=='.' or a[-1]=='.' or b[-1]=='.':
        return False
    if '.' not in b:
        return False
    return True

while True:
    try:
        s=input()
        if is_allowed(s):
            print("YES")
        else:
            print("NO")
    except EOFError:
        break
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250305192200149](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250305192200149.png)



### M02039: 反反复复

implementation, http://cs101.openjudge.cn/practice/02039/



思路：

输入输出题，没啥好说的

代码：

```python
n=int(input())
s=input()
l=[]
for i in range(len(s)//n):
    if i%2==0:
        l.append(s[i*n:i*n+n])
    else:
        l.append(s[i*n+n-1:i*n-1:-1])
for j in range(n):
    for i in range(len(l)):
        print(l[i][j],end="")
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![image-20250305192327792](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250305192327792.png)

### M02092: Grandpa is Famous

implementation, http://cs101.openjudge.cn/practice/02092/



思路：

这题一开始没看懂题目，原因是网页端的翻译没有将each appearance in a weekly ranking constitutes **a** point for the player 这句话翻译出来。事实上，将“a”改为“one”将会为理解带来更多便利。

代码本身的实现不难。

代码：

```python
from collections import defaultdict
while True:
    n,m=map(int,input().split())
    if n==0 and m==0:
        break
    d=defaultdict(int)
    for _ in range(n):
        l=list(map(int,input().split()))
        for i in l:
            if i in d:
                d[i]+=1
            else:
                d[i]=1
    sorted_d=sorted(d.items(),key=lambda x:x[1],reverse=True)
    ans=[]
    cur=sorted_d[1][1]
    for i in range(1,len(sorted_d)):
        if sorted_d[i][1]==cur:
            ans.append(sorted_d[i][0])
        else:
            break
    ans.sort()
    print(*ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250305192447965](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250305192447965.png)



### M04133: 垃圾炸弹

matrices, http://cs101.openjudge.cn/practice/04133/



思路：

老题了，暴力计算即可

代码：

```python
d=int(input())
n=int(input())
l=[[0]*1025 for _ in range(1025)]
for _ in range(n):
    x,y,i=map(int,input().split())
    for xx in range(x-d,x+d+1):
        for yy in range(y-d,y+d+1):
            if 0<=xx<1025 and 0<=yy<1025:
                l[xx][yy]+=i
m=0
cnt=0
for i in range(1025):
    for j in range(1025):
        if l[i][j]>m:
            m=l[i][j]
            cnt=1
        elif l[i][j]==m:
            cnt+=1
print(cnt,m)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![屏幕截图 2025-03-05 192739](D:\学习软件\2025春\数算 闫鸿飞\第3节\屏幕截图 2025-03-05 192739.png)



### T02488: A Knight's Journey

backtracking, http://cs101.openjudge.cn/practice/02488/



思路：dfs

考试的时候一直WA，但一直找不到问题，考完后才发现漏看了条件“按字典顺序的第一个”。这题错的可惜

代码：

```python
moves = [(-1,-2),(1,-2),(-2,-1),(2,-1),(-2,1),(2,1),(-1,2),(1,2)]
def normalize(x,y)->str:
    a=str(x+1)
    b=chr(ord('A')+y)
    return b+a
def dfs(start_x,start_y,p,q,step):
    if step==p*q:
        return True
    for (dx,dy) in moves:
        xx,yy=start_x+dx,start_y+dy
        if 0<=xx<p and 0<=yy<q and not visited[xx][yy]:
            visited[xx][yy]=True
            ans[step]=normalize(xx,yy)
            if dfs(xx,yy,p,q,step+1):
                return True
            visited[xx][yy]=False
    return False
n=int(input())
for i in range(1,n+1):
    p,q=map(int,input().split())
    print("Scenario #%d:"%i)
    ans=["" for _ in range(p*q)]
    ans[0]="A1"
    visited = [[False] * q for _ in range(p)]
    visited[0][0]=True
    if dfs(0,0,p,q,1):
        print("".join(ans))
    else:
        print("impossible")
    print()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250305200558087](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250305200558087.png)



### T06648: Sequence

heap, http://cs101.openjudge.cn/practice/06648/



（原先的）思路：greedy

1、对所有数组预处理，取出每个数组的最小值，作为基准；将元组（某数与基准值的差，该数存在的数组名）推入heap。例如：0号数组的最小值是2，那么0号数组中某个数x推入的就是（x-2，“0”）。这里x-2就是所谓“修正”

2、最终某个求和结果与最小求和结果的差，就是heap中某m<n个数的和。这m个数要求不存在同一个数组里的两个数。当前heap中只有m=1的情况。

3、每次从heap中pop出一种情形，推入答案数组；依次检索答案数组其余的情形，检测两者的求和是否合法。若合法，则将求和后的情形推入heap。

​	这里合法的判定是：修正值所在的数组之间不能重合。比如：

​		A情形：0号数组取+1修正，1号数组取+2修正，记为（3，{0，1}）

​		B情形：2号数组取+1修正，记为（1，{2}）

​		C情形：0号数组取+1修正，3号数组取+1修正，记为（2，{0，3}）

​	那么合法的求和有：AB（4，{0，1，2}），BC（3，{2，0，3}），但AC求和是不合法的。

4、重复n-1次（因为还有一种就是m=0的情形，作为最小值），答案数组得到了全部的n个最小求和可能



理论上是O（n^2），可以接受；可是这个思路获得了Wrong Answer……

（原先的）代码：

```python
import heapq
t=int(input())
for _ in range(t):
    m,n=map(int,input().split())
    min_sum=0
    ll=[]
    ans=[]
    heapq.heapify(ll)
    #预处理，将每个数组中的最小值提前取出，剩下的作为修正值入堆
    for i in range(m):
        l=list(map(int,input().split()))
        l.sort()
        min_sum+=l[0]
        for j in range(1,len(l)):
            heapq.heappush(ll,(l[j]-l[0],set([i])))
        ll=heapq.nsmallist(ll,n)
    for i in range(n-1):
        a=heapq.heappop(ll)
        ans.append(a)
        for b in ans:
            if not a[1] & b[1]:  #判定是否可以加和
                heapq.heappush(ll,(a[0]+b[0],a[1]|b[1]))
    print(str(min_sum)+" "+" ".join(str(ans[i][0]+min_sum) for i in range(len(ans))))

```

但是实在找不到哪里错了……AI也说不清楚

-----------------------------------------------------

现在经过老师的指点，我修改了代码，仍然WA

![修改后的程序](D:\学习软件\2025春\数算 闫鸿飞\第3节\修改后的程序.png)

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20250305191330408](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250305191330408.png)



## 2. 学习总结和收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025spring每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>

考的时候AC4，其实浪费了大量时间在第5题上，最后还是错了；错因在审题，这应该被避免。

第6题做起来还是挺有意思的；立刻反应到heap考点，但是目前并不清楚为什么错。也请老师帮忙看看？









