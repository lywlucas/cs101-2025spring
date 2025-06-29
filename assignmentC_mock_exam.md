# Assignment #C: 202505114 Mock Exam

Updated 1518 GMT+8 May 14, 2025

2025 spring, Complied by <mark>卢殷文</mark>



> **说明：**
>
> 1. **⽉考**：AC5<mark>（请改为同学的通过数）</mark> 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。
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

### E06364: 牛的选举

http://cs101.openjudge.cn/practice/06364/

思路：

sort

代码：

```python
import heapq
n,k=map(int,input().split())
h=[]
for i in range(1,n+1):
    x,y=map(int,input().split())
    heapq.heappush(h,(x,y,i))
l=heapq.nlargest(k,h)
l.sort(key=lambda x:x[1],reverse=True)
print(l[0][2])
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250514172609334](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250514172609334.png)



### M04077: 出栈序列统计

http://cs101.openjudge.cn/practice/04077/

思路：

就是排序n个push操作和n个pop操作，使得任意时候push的数量不小于pop的数量

经典dp题

代码：

```python
n=int(input())
dp=[[0]*(n+1) for _ in range(n+1)]
dp[0]=[1]*(n+1)
for popi in range(1,n+1):
    for pushj in range(popi,n+1):
        dp[popi][pushj]=dp[popi][pushj-1]+dp[popi-1][pushj]
print(dp[n][n])
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250514172703548](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250514172703548.png)



### M05343:用队列对扑克牌排序

http://cs101.openjudge.cn/practice/05343/

思路：

implementation

代码：

```python
n=int(input())
l=list(input().split())
nums=[[] for _ in range(9)]
color=[[] for _ in range(4)]
ans=[]
for s in l:
    i=int(s[1])
    nums[i-1].append(s)
for i in range(1,10):
    print("Queue%d:"%i+" ".join(nums[i-1]))
    for s in nums[i-1]:
        j=ord(s[0])-ord('A')
        color[j].append(s)
for j in range(4):
    c=chr(j+ord('A'))
    print("Queue%s:"%c+" ".join(color[j]))
    ans+=color[j]
print(*ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250514172959549](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250514172959549.png)



### M04084: 拓扑排序

http://cs101.openjudge.cn/practice/04084/

思路：

就是边排边删……

代码：

```python
v,a=map(int,input().split())
pre=[[] for _ in range(v)] #pre[i]表示节点i必须晚于哪些数
toe=[[] for _ in range(v)]
for _ in range(a):
    x,y=map(int,input().split())
    pre[y-1].append(x-1)
    toe[x-1].append(y-1)
def get_root():
    for i in range(v):
        if not pre[i]:
            return i
def delete(i):
    pre[i]=[-1]
    for j in toe[i]:
        pre[j].remove(i)

ans=[]
while len(ans)<v:
    vi=get_root()
    ans.append("v%d"%(vi+1))
    delete(vi)
print(*ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250514172842322](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250514172842322.png)



### M07735:道路

Dijkstra, http://cs101.openjudge.cn/practice/07735/

思路：

月考就错了这一题qwq

一看节点数只有100，O(n**3) 轻轻松松，直接暴力递归上了：

（考试时的）代码：

```python
INF=10**9
km=int(input())
n=int(input())
r=int(input())
g=[[(INF,INF)]*(n+1) for _ in range(n+1)]
for _ in range(r):
    s,d,l,t=map(int,input().split())
    if t<=km:
        g[s][d]=(l,t)
for k in range(1,n+1):
    for i in range(1,n+1):
        if g[i][k]!=(INF,INF):
            for j in range(1,n+1):
                if g[i][k][1]+g[k][j][1]<=km:
                    g0=min(g[i][j][0],g[i][k][0]+g[k][j][0])
                    g1=min(g[i][j][1],g[i][k][1]+g[k][j][1])
                    g[i][j]=(g0,g1)
print(g[1][n][0] if g[1][n][0]<INF else -1)
```

然后一直报WA。思考了一下，应该是没有处理“同首同尾不同路”的情况。

回来看题解，在dijkstra算法中，通过删visited筛查，补剪枝，这个问题得以解决

```python
import heapq
k = int(input())
n = int(input())
r = int(input())
graph = {i:[] for i in range(1, n+1)}
for _ in range(r):
    s, d, dl, dt = map(int, input().split())
    graph[s].append((dl,dt,d))
q = [(0,0,1)]
fee = [10000]*101
def dijkstra(g):
    while q:
        l, t, d = heapq.heappop(q)
        if d == n:
            return l
        if t>fee[d]:
            continue
        fee[d] = t
        for dl, dt, next_d in g[d]:
            if t+dt <= k:
                heapq.heappush(q,(l+dl, t+dt, next_d))
    return -1
print(dijkstra(graph))
```

代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250514172511927](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250514172511927.png)



### T24637:宝藏二叉树

dp, http://cs101.openjudge.cn/practice/24637/

思路：

这题反而不难，第一反应就是recursion

具体而言设置hunting(root)函数表示对于根节点为root的树进行宝藏搜寻，返回两个值：

挖了root时的最大收益，和不挖root时的最大收益。

挖root对应两个子树都必须不挖根；不挖root则从两个子树的hunting过程中各自取最大值。

代码：

```python
import sys
sys.setrecursionlimit(10**9)
n=int(input())
if n==0:
    print(0)
else:
    treasure = [0] + list(map(int, input().split()))
    def hunting(x):
        t = treasure[x]
        l, r = 2 * x, 2 * x + 1
        if l > n:
            return 0, t
        elif l == n:
            return treasure[l], t
        else:
            lt, lf = hunting(l)
            rt, rf = hunting(r)
            xt = max(lt, lf) + max(rt, rf)
            xf = lt + rt + treasure[x]
            return xt, xf
    print(max(hunting(1)))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250514173157922](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250514173157922.png)



## 2. 学习总结和收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025spring每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>

总地来说这次月考还是要比前几次简单一些的；话说这次题目的dp含量怎么有点高呢（

但是某个笨蛋写了dp就不去想dijkstra了（反思











