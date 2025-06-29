# Assignment #B: 图为主

Updated 2223 GMT+8 Apr 29, 2025

2025 spring, Complied by <mark>卢殷文 物院</mark>



> **说明：**
>
> 1. **解题与记录：**
>
>    对于每一个题目，请提供其解题思路（可选），并附上使用Python或C++编写的源代码（确保已在OpenJudge， Codeforces，LeetCode等平台上获得Accepted）。请将这些信息连同显示“Accepted”的截图一起填写到下方的作业模板中。（推荐使用Typora https://typoraio.cn 进行编辑，当然你也可以选择Word。）无论题目是否已通过，请标明每个题目大致花费的时间。
>
> 2. **提交安排：**提交时，请首先上传PDF格式的文件，并将.md或.doc格式的文件作为附件上传至右侧的“作业评论”区。确保你的Canvas账户有一个清晰可见的头像，提交的文件为PDF格式，并且“作业评论”区包含上传的.md或.doc附件。
>
> 3. **延迟提交：**如果你预计无法在截止日期前提交作业，请提前告知具体原因。这有助于我们了解情况并可能为你提供适当的延期或其他帮助。 
>
> 请按照上述指导认真准备和提交作业，以保证顺利完成课程要求。



## 1. 题目

### E07218:献给阿尔吉侬的花束

bfs, http://cs101.openjudge.cn/practice/07218/

思路：

（所以这题和花束有什么关系

代码：

```python
from collections import deque
def find_SE(r,c,map):
    s,e=(-1,-1),(-1,-1)
    for i in range(r):
        for j in range(c):
            if map[i][j]=='S':
                s=(i,j)
            if map[i][j]=='E':
                e=(i,j)
    return [s,e]
def bfs(SE,r,c,map):
    s,e=SE[0],SE[1]
    x0,y0=s[0],s[1]
    q=deque([(x0,y0,0)])
    in_queue={(x0,y0)}
    while q:
        x,y,cnt=q.popleft()
        if (x,y)==e:
            return cnt
        for (dx,dy) in [(-1,0),(1,0),(0,-1),(0,1)]:
            if 0<=x+dx<r and 0<=y+dy<c and (x+dx,y+dy) not in in_queue:
                if map[x+dx][y+dy]!='#':
                    in_queue.add((x+dx,y+dy))
                    q.append((x+dx,y+dy,cnt+1))
    return "oop!"

t=int(input())
for _ in range(t):
    r,c=map(int,input().split())
    mp=[input() for i in range(r)]
    SE=find_SE(r,c,mp)
    print(bfs(SE,r,c,mp))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250503101158870](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250503101158870.png)



### M3532.针对图的路径存在性查询I

disjoint set, https://leetcode.cn/problems/path-existence-queries-in-a-graph-i/

思路：

用一个head数组表示每个数所在的组别，这是很自然的。

代码：

```python
class Solution:
    def pathExistenceQueries(self, n: int, nums: List[int], maxDiff: int, queries: List[List[int]]) -> List[bool]:
        head=nums[:]
        for i in range(1,n):
            if nums[i]-nums[i-1]<=maxDiff:
                head[i]=head[i-1]
        ans=[]
        for q in queries:
            if head[q[0]]==head[q[1]]:
                ans.append(True)
            else:
                ans.append(False)
        return ans
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250503101946288](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250503101946288.png)



### M22528:厚道的调分方法

binary search, http://cs101.openjudge.cn/practice/22528/

思路：

注意到调分函数的单调性，且其实际上是a*x的函数而非a和x各自的函数；因此可以直接解出怎样的ax可以使得调分后达到85分。

直接使用Casio容易得到结果39.95111738；事实上，另写一个程序采用牛顿法/二分法得到解也是等效的。

代码：

```python
Const=39.95111738 * 10**9 # calculated by Casio
score=list(map(float,input().split()))
score.sort()
n=len(score)
i=int(n*0.4)
x=score[i]
a=int(Const/x)
print(a if a*x>=Const else a+1)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250503104458603](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250503104458603.png)



### Msy382: 有向图判环 

dfs, https://sunnywhy.com/sfbj/10/3/382

思路：



代码：

```python
n,m=map(int,input().split())
con_to=[[] for _ in range(n)]
for _ in range(m):
    u,v=map(int,input().split())
    con_to[u].append(v)
visited=[0]*n
def dfs(start):
    flg=1
    for i in con_to[start]:
        if visited[i]==1:
            return 0
        visited[i]=1
        flg*=dfs(i)
        visited[i]=0
        if flg==0:
            break
    return flg
for i in range(n):
    visited = [0] * n
    visited[i] = 1
    if dfs(i)==0:
        print("Yes")
        break
else:
    print("No")
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250503113223342](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250503113223342.png)



### M05443:兔子与樱花

Dijkstra, http://cs101.openjudge.cn/practice/05443/

思路：

属于是dijkstra算法启蒙题了

代码：

```python
import heapq
from collections import defaultdict

p=int(input())
points=[input() for _ in range(p)]
mp=defaultdict(list)
for _ in range(int(input())):
    v1,v2,distance=input().split()
    d=int(distance)
    mp[v1].append((v2,d))
    mp[v2].append((v1,d))
def dijkstra(start,end):
    dist={point: float('inf') for point in points}
    path={point: "" for point in points}
    dist[start]=0
    path[start]=start
    q=[(0,start)]
    while q:
        dt,w=heapq.heappop(q)
        if dt>dist[w]:
            continue
        if w==end:
            break
        for v,dd in mp[w]:
            if dd+dt<dist[v]:
                dist[v]=dd+dt
                path[v]=path[w]+f"->({dd})->"+v
                heapq.heappush(q,(dd+dt,v))
    return path[end]

for _ in range(int(input())):
    s,e=input().split()
    print(dijkstra(s,e))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250505100516938](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250505100516938.png)



### T28050: 骑士周游

dfs, http://cs101.openjudge.cn/practice/28050/

思路：

单纯回溯算法的后果就是TLE；

改进：采用Warnsdorff规则，优先搜索后续选择更少的点。程序可以在1秒之内完成$16\times16$的全棋盘巡游判定，以及1秒之内完成$128\times128$的给定起始点巡游判定。

这个规则的缺点在于，只看未来一步，容易走进死胡同而浪费许多算力；在路径的末端（step接近n*n时）效率低下。

代码：

```python
import sys
sys.setrecursionlimit(10**9)
n=int(input())
mp=[[0]*n for _ in range(n)]
def knight(x,y,n):
    ans=[]
    for dx,dy in [(-1,2),(1,2),(1,-2),(-1,-2),(2,1),(2,-1),(-2,1),(-2,-1)]:
        xx=x+dx
        yy=y+dy
        if 0<=xx<n and 0<=yy<n:
            if mp[xx][yy]==0:
                ans.append((xx,yy))
    return ans

def dfs(x,y,step,n):
    flg=False
    if step==n**2:
        return True
    chance=[]
    for (i,j) in knight(x,y,n):
        chance.append((i,j,len(knight(i,j,n))))
    chance.sort(key=lambda x: x[2])
    for (i,j,k) in chance:
        mp[i][j]=1
        if dfs(i,j,step+1,n):
            flg=True
            break
        mp[i][j]=0
    return flg

sr,sc=map(int,input().split())
if dfs(sr,sc,1,n):
    print('success')
else:
    print('fail')
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250505140631172](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250505140631172.png)



## 2. 学习总结和收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025spring每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>

事实上，骑士周游问题值得更多的研究：

[Knight's tour - Wikipedia](https://en.wikipedia.org/wiki/Knight's_tour#Existence)

一个简单的结论是，对于n*n棋盘，当n>5且为偶数时，对于任意起始点，骑士巡游一定存在。







