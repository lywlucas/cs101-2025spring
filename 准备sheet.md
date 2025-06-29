# cheat sheet

# 小寄巧：

print(f'{ans},{res:.1f}')print是可以带sep和end参数的

可以用round进行四舍六入五成双的操作

枚举：  for i,x in enumerate(list),遍历list中的（下标，值）对

递归爆栈：

```python
from sys import setrecurisonlimit
setrecursionlimit(10000)#python 默认 200
```

# OOP：


 | `__eq__(self, other)` | `==` | 判断相等 |
 | `__ne__(self, other)` | `!=` | 判断不相等 |
 | `__lt__(self, other)` | `<` | 判断是否小于 |
 | `__le__(self, other)` | `<=` | 判断是否小于等于 |
 | `__gt__(self, other)` | `>` | 判断是否大于 |
 | `__ge__(self, other)` | `>=` | 判断是否大于等于 |

# 常用库

```python
#堆
import heapq
#队列，default字典
from collections import deque,defaultdict
#递归上限
from sys import setrecursionlimit
#缓存
from functools import lru_cache
#数学***math库***：最常用的sqrt,对数log(x[,base])、三角sin()、反三角asin()也都有；还有e,pi等常数，inf表示无穷大；返回小于等于x的最大整数floor（）,大于等于ceil（）,判断两个浮点数是否接近isclose（a，b，*, rel_tol=1e-09, abs_tol=0.0）；一般的取幂pow（x，y）,阶乘factorial（x）如果不符合会ValueError,组合数comb（n，k）`math.radians()`将度数转换为弧度，或者使用`math.degrees()`将弧度转换为度数。
import math
#深拷贝
from copy import deepcopy
#二分库
import bisect
bisect.bisect_right(a,6)#返回在a列表中若要插入6的index（有重复数字会插在右边）
bisect.insort(a,6)#返回插入6后的列表a
#笛卡尔积，排列，组合
from itertools import product,permutations,combinations
#conuter 用于统计可迭代对象中元素出现的次数，并返回一个字典（key-value）key 表示元素，value 表示各元素 key 出现的次数。
from collections import Counter
```

# 算法

### 快速随机排序：

```python
def quicksort(arr, left, right):
    if left < right:
        mid = partition(arr, left, right)
        quicksort(arr, left, mid - 1)
        quicksort(arr, mid + 1, right)

def partition(arr, left, right):
    i = left
    j = right - 1
    pivot = arr[right]
    while i <= j:
        while i <= right and arr[i] < pivot:
            i += 1
        while j >= left and arr[j] >= pivot:
            j -= 1
        if i < j:
            arr[i], arr[j] = arr[j], arr[i]
    if arr[i] > pivot:
        arr[i], arr[right] = arr[right], arr[i]
    return i
```

### 分治排序

```python
def mergeSort(arr):
	if len(arr) > 1:
		mid = len(arr)//2
		L = arr[:mid]	# Dividing the array elements
		R = arr[mid:] # Into 2 halves
		mergeSort(L) # Sorting the first half
		mergeSort(R) # Sorting the second half
		i = j = k = 0
		while i < len(L) and j < len(R):
			if L[i] <= R[j]:
				arr[k] = L[i]
				i += 1
			else:
				arr[k] = R[j]
				j += 1
			k += 1
		# Checking if any element was left
		while i < len(L):
			arr[k] = L[i]
			i += 1
			k += 1
		while j < len(R):
			arr[k] = R[j]
			j += 1
			k += 1
```



## *Kadane's(最大子数组)

```python
def max_subarray_sum(arr):
    if not arr:
        return 0
    max_current=max_global=arr[0]
    for num in arr[1:]:
        max_current =max(num,max_current+num)
        if max_current>max_global:
			max_global= max_current
    return max_global
```

推广：最大子矩阵

为了找到最大的非空子矩阵，可以使用动态规划中的Kadane算法进行扩展来处理二维矩阵。
基本思路是将二维问题转化为一维问题：可以计算出从第i行到第j行的列的累计和，
这样就得到了一个一维数组。然后对这个一维数组应用Kadane算法，找到最大的子数组和。
通过遍历所有可能的行组合，我们可以找到最大的子矩阵。

## 欧拉筛：

```python
def oula(a):
    zhishu=[]
    zhishu1=[True]*(a+1)
    for i in range(2,a+1):
        if zhishu1[i]:
            zhishu.append(i)
        for h in zhishu:
            if h*i<=a:
                zhishu1[h*i]=False
    zhishu=set(zhishu)
    return zhishu
```

## sliding window：

滑动窗口：维持左右边界都不回退的一段范围，来求解很多子数组的相关问题

关键：找到 **范围** 和 **答案指标** 之间的 **单调性关系**

过程：可以用简单变量或者结构来维护信息

大流程：求子数组在每个位置 开头或结尾的情况下的答案

for 枚举右边界：
	while 枚举左边界：
	if 条件：
		ans更新



## 嵌套问题：

大概过程：

​	1.定义全局变量where
​	2.递归函数f（i）：s[i..]从i位置出发开始解析，遇到字符串终止 或 嵌套条件终止 就返回
​	3.返回值f(i)负责这一段的结果
​	4.f(i)在返回前更新的全局变量where，让上级函数通过where指导解析到了什么位置，进而继续

执行细节：

​	1.如果f(i)遇到 嵌套条件开始，就调用下级递归去处理嵌套，下级会负责嵌套部分的计算结果
​	2.f(i)下级处理完成后，f(i)可以根据下级更新的全局变量where，指导该从什么位置继续解析

e.g.括号嵌套树（重点是f函数）

```python
class treenode:
    def __init__(self,val):
        self.val=val
        self.children=[]
s=input()
where=0
def f(i):
    global where,s
    st=[]
    while i<len(s) and s[i]!=')':
        if s[i]==',':
            i+=1
        elif 'A'<=s[i]<='Z':
            st.append(treenode(s[i]))
            i+=1
        elif s[i]=='(':
            st[-1].children=f(i+1)
            i=where+1
    where=i
    return st
```

# stack	

## 单调栈：（来自柱状图最大矩形，具体题目需要变形）

求左右最近的小于自身的数：

有重复数字也是一样的操作（等于也弹出），但是最后要进行一遍右答案的修正（因为有可能记录的是相等的值）（从右往左修正）

```python
#求左右两边严格小于自身的最近的数 并且有重复值 的模板
#遍历
for i in range(n):
    while st and arr[st[-1]]>arr[i]:
        cur=st.pop()
        ans[cur][0]=st[-1] if st else -1
        ans[cur][1]=i
    st.append(i)
#清算
while st:
    cur=st.pop()
    ans[cur][0]=st[-1] if st else -1
    ans[cur][1]=-1
#修正
#n-1一定是-1，所以不需要修正
for i in range(n-2,-1,-1):
    if ans[i][1]!=-1 and arr[ans[i][1]]==arr[i]:
        ans[i][1]=ans[ans[i][1]][1]
```

重复一定要特判，子数组一题重复的就要作为ans才可以不重不漏。有些时候中间的相等值答案可能不对，只要后续的相等值进来能把答案修正对就可以了（回忆最大矩形一题，相等也弹出）

妙题：01矩阵中面积最大的长方形：枚举每一行，以每一行作为底去进行单调栈即可（不连续就变成0，还要记得复用上一行的数据）

其他用法：维持答案的一种可能性，比如求数组中的坡，维持栈中是递减的，遇到大的弹出，然后再从右往左更新答案。
比如字典序最小的规定字符的字符串，先用counter记录能不能删某个字符，再用单调栈去维护字典序最小

# tree：

#坑点——最小深度：递归 return min（孩子深度）+1  注意空节点会干扰递归，所以要先把孩子深度设为inf，如果不是空了再修改其值。

### lca问题（寻找最早公共祖先）

```python
def lowestCommonAncestor(root,p,q):
    if root is None or p is None or q is None:
        return root
    l=lowestCommonAncestor(root.left,p,q)
    r=lowestCommonAncestor(root.right,p,q)
    if l is not None and r is not None:
        return root
    if l is None and r is None:
        return None
    return l if l is not None else r
```

### 修剪搜索二叉树：

```python
def trimBST(cur,low,high):
    if cur is None:
        return None
    if cur.val<low:
        return trimBST(cur.right,low,high)
    if cur.val>high:
        return trimBST(cur.left,low,high)
    cur.left=trimBST(cur.left,low,high)
    cur.right=trimBST(cur.right,low,high)
    return cur
```



## 树形DP： 

套路：

​	1.分析父树得到答案需要子树的哪些信息
​	2.把子树的信息的全集定义成返回值
​	3.通过递归让子树返回全集信息
​	4.整合子树的全集信息得到父树的全集信息并返回

一般来说：空树的max是-inf，min是inf，这样不会干扰信息

## 前缀树trie

tips：

​	数组呢个题用num+#把它改成字符串，这样只有0-9和-和#，这样就不会爆空间
​	最大异或值也可以用前缀树来实现
​	表格中查单词：用前缀树来剪枝

```python
class Trie:
    class TrieNode:
        def __init__(self):
            self.p=0
            self.e=0
            self.next=[None]*26
            #若不仅仅是字母还有多个字符的话可以用dict优化{字母：下一个节点}

    def __init__(self):
        self.root=self.TrieNode()

    def insert(self,word):
        node=self.root
        node.p+=1
        for i in range(len(word)):
            path=ord(word[i])-ord('a')
            if node.next[path] is None:
                node.next[path]=self.TrieNode()
            node=node.next[path]
            node.p+=1
        node.e+=1
        return

    def search(self,word):
        node=self.root
        for i in range(len(word)):
            path=ord(word[i])-ord('a')
            if node.next[path] is None:
                return 0
            node=node.next[path]
        return node.e

    def startsWith(self,word):
        node=self.root
        for i in range(len(word)):
            path=ord(word[i])-ord('a')
            if node.next[path] is None:
                return 0
            node=node.next[path]
        return node.p

    def delete(self,word):
        if self.search(word)>0:
            node=self.root
            node.p-=1
            for i in range(len(word)):
                path=ord(word[i])-ord('a')
                if node.next[path].p==1:
                    node.next[path]=None
                    return
                node=node.next[path]
                node.p-=1
            node.e-=1
        return
```



## huffman：

给个例子应该就能想起来咋写的了，用heap

![image-20250602204434160](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250602204434160.png)

```python
import heapq
class node:
    def __init__(self, char,freq):
        self.char = char
        self.left=None
        self.right=None
        self.freq=freq
    #用于比较
    def __lt__(self, other):
        if self.freq==other.freq:
            return self.char<other.char
        return self.freq<other.freq
def build_huffman(d):
    q=[]
    for char,freq in d.items():
        heapq.heappush(q,node(char,freq))
    heapq.heapify(q)
    while len(q)>1:
        left=heapq.heappop(q)
        right=heapq.heappop(q)
        if left.char<right.char:
            c=left.char
        else:
            c=right.char
        nn=node(c,left.freq+right.freq)
        nn.left=left
        nn.right=right
        heapq.heappush(q,nn)
    return heapq.heappop(q)
def build_code(root):
    stack=[(root,'')]
    di={}
    dic={}
    while stack:
        x,y=stack.pop()
        if x.left:
            stack.append((x.left,y+'0'))
        if x.right:
            stack.append((x.right,y+'1'))
        if not x.left and not x.right:
            di[x.char]=y
            dic[y]=x.char
    return di,dic
n=int(input())
d={}
for i in range(n):
    char,freq=input().split()
    freq=int(freq)
    d[char]=freq
root=build_huffman(d)
d_str,d_num=build_code(root)
while True:
    try:
        s=input()
        if s[0]=='0' or s[0]=='1':
            a=''
            for i in s:
                a+=i
                if a in d_num:
                    print(d_num[a],end='')
                    a=''
        else:
            for i in s:
                print(d_str[i],end='')
        print()
    except EOFError:
        break
```

## disjointset

```python
class disjointset:
    def __init__(self,n):
        self.father=[x for x in range(n)]
        self.dict={}
    def find(self,x):
        stack=[]
        while self.father[x]!=x:
            stack.append(x)
            x=self.father[x]
        for i in stack:
            self.father[i]=x
        return x
    def issameset(self,x,y):
        return self.find(x) == self.find(y)
    def union(self,x,y):
        fx=self.find(x)
        fy=self.find(y)
        if fx!=fy:
            self.father[fy]=fx
```



# graph

## 最小生成树：

### kruskal（优先）：

greedy+disjointset

1.把所有的边按照权值sort，从权值小的开始考虑
2.如果当前边的两个节点不在一个集合：选择这个边
3.如果在一个集合：不选

## DFS：

由于DFS的特性，path可以不参与变量的传递，这样只用一个全局变量path修改就行了，找到了可能的答案就copy到ans（字符串可以用两个变量，copy的时候就不会出事）。

### *Warnsdorff：

接下来访问的点的能访问数是最少的（回忆骑士周游的degree优化）

```python
def degree(x,y,board):
    global n,di
    cnt=0
    for dx,dy in di:
        nx,ny=x+dx,y+dy
        if 0<=nx<n and 0<=ny<n and board[nx][ny]==-1:
            cnt+=1
    return cnt
def dfs(x,y,cnt):
    global di,board,n
    if cnt==n*n:
        return True
    next_move=[]
    for dx,dy in di:
        nx,ny=x+dx,y+dy
        if 0<=nx<n and 0<=ny<n and board[nx][ny]==-1:
            next_move.append((degree(nx,ny,board),nx,ny))
    next_move.sort()
    for _,nx,ny in next_move:
        board[nx][ny]=cnt
        if dfs(nx,ny,cnt+1):
            return True
        board[nx][ny]=-1
    return False
```



## BFS类

### BFS：

```python
from collections import deque
def bfs(start_x,start_y):
    q=deque([0,start_x,start_y])
    in_queue={(start_x,start_y)} #也可以是列表形式 List[bool]
    while q:
        step,x,y=q.popleft()
        if 达到终点：
        	return step
        for 坐标(i,j) in 可能的坐标：
        	if 坐标合理且不在in_queue中：
            	in_queue.add((i,j))
                q.append((step+1,i,j))
        return 错误反馈
```

tip：

​	queue中的元素：不一定只是坐标对，有时间参量的时候要考虑加上时间的三元元组
​	BFS的劣势在于求路径问题时容易MLE，回忆词梯，可以先BFS求得有无路径（同时建图），再用DFS搜图	
​	小游戏：以拐弯此处作为BFS下一层的条件
​	变换的迷宫：重复一个周期啥也没干的就可以删了（需要额外的一个列表去判断上一个周期的状态）

visited类型：

​	1.set（）
​	2.[[0]*n for _ in range(m)]

### 拓扑排序：（判断环）

e.g.邻接表形式 graph={vertex : List [ neighbors]  }

```python
from collections import deque, defaultdict
indegree=defaultdict(int)
queue=deque() #如果有并列情况下的顺序要求：用heap
result=[]
for u in graph:
    for v in graph[u]:
        indegree[v]+=1
for u in graph:
    if indegree[u]==0:
        queue.append(u)
while queue:
    u=queue.popleft()
    result.append(u)
    for v in graph[u]:
        indegree[v]-=1
        if indegree[v]==0:
            queue.append(v)
#若len(result)==len(graph)则无环，否则有环
```

### 三色标记法（判断环）：

#### 核心思路

如果在递归过程中，发现下一个节点在递归栈中（正在访问中），则找到了环。

#### 具体思路

对于每个节点 *x*，都定义三种颜色值（状态值）：

0：节点 *x* 尚未被访问到。
1：节点 *x* 正在访问中，*dfs*(*x*) 尚未结束。
2：节点 *x* 已经完全访问完毕，*dfs*(*x*) 已返回。

⚠**误区**：不能只用两种状态表示节点「没有访问过」和「访问过」。例如上图，我们先 *dfs*(0)，再 *dfs*(1)，此时 1 的邻居 0 已经访问过，但这并不能表示此时就找到了环。

```python
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        g = [[] for _ in range(numCourses)]
        for a, b in prerequisites:
            g[b].append(a)

        colors = [0] * numCourses
        # 返回 True 表示找到了环
        def dfs(x: int) -> bool:
            colors[x] = 1  # x 正在访问中
            for y in g[x]:
                if colors[y] == 1 or colors[y] == 0 and dfs(y):
                    return True  # 找到了环
            colors[x] = 2  # x 完全访问完毕
            return False  # 没有找到环

        for i, c in enumerate(colors):
            if c == 0 and dfs(i):
                return False  # 有环
        return True  # 没有环
```

<mark>无向图的loop用dfs或者并查集（考察边）来判断</mark>

### **Dijkstra**

```python
distance=[float('inf')]*n
distance[s]=0
visied=[False]*n
q=[]
heapq.heappush(q,(0,s))
while q:
    d,u=heappop(q)
    #可以加剪枝：判断是否到达终点，例如 if u==end_node: return
    if visited[u]: #看情况改成其他条件，如月考中heap内存三元数组(distance,money,node)，就判money
        continue
    visited[u]=True
    for v,w in e[u]:
		if visited[v] and distance[u]+w<distance[v]:
            diatance[v]=distance[u]+w
            #这里可以加backtracking，例如previous[x]=u
            heapq.heappush(q,(distance[u]+w,v))
```

tips:

​	最后一次月考道路一题，最短路是一方面，能不能走是另一方面，所以不能只是单纯的Dijkstra，起码不能有visited的逻辑。（可以理解为钱数就充当了能否visited的条件了）

​	剪枝，弹出来判断一下是不是终点，避免MLE

**不写visited，而是懒更新**：e.g.对于邻接表输入：

```python
import heapq
 def dijkstra(N, G, start):
    INF = float('inf')
    dist = [INF] * (N + 1)  # 存储源点到各个节点的最短距离
    dist[start] = 0  # 源点到⾃身的距离为0
    pq = [(0, start)]  # 使⽤优先队列，存储节点的最短距离
    while pq:
        d, node = heapq.heappop(pq)  # 弹出当前最短距离的节点
        if d > dist[node]:  # 如果该节点已经被更新过了，则跳过
            continue
        for neighbor, weight in G[node]:  # 遍历当前节点的所有邻居节点
            new_dist = dist[node] + weight  # 计算经当前节点到达邻居节点的距离
            if new_dist < dist[neighbor]:  # 如果新距离⼩于已知最短距离，则更新最短距离
                dist[neighbor] = new_dist
                heapq.heappush(pq, (new_dist, neighbor))  # 将邻居节点加⼊优先队列
    return dist
```



### *A星优化常数:

在Dijkstra的基础上增加了预估函数，并且让堆以**从源点出发到当前点的距离+当前点到终点的预估距离**进行排序

预估函数**要求**：当前点到终点的预估距离<=当前点到终点的真实最短距离

预估函数常选：
	曼哈顿距离、欧式距离、对角线距离

## Floyd:（求任意两点之间的最短距离）

用**邻接矩阵**储存图

适用于任何图（不能有负环）

```python
for bridge in range(n):
    for i in range(n):
        for j in range(n):
            if distance[i][bridge]!=float('inf') and distance[bridge][j]!=float('inf') and distance[i][j] > distance[i][bridge]+distance[bridge][j]:
                distance[i][j]=distance[i][bridge]+distance[bridge][j]         
```

## Bellman-Ford：（有源点的最短路）

解决可以有负权但是不能有负环（保证最短路存在）的图，单源最短路

**松弛操作**：
​边的权重Weight满足 distance[from] + Weight<distance[to]，则更新

Bellman-Ford过程：

​	1.**每一轮考察每条边**，每条边都尝试进行松弛操作，那么若干点的distance会变小
​	2.当某一轮发现不再有松弛操作出现时，停止

**重要推广：判断从某个点出发能不能到达负环**
如果从A出发存在最短路（没有负环），那么松弛的论述必然<=n-1
而如果从A点出发到达一个负环，那么松弛操作显然会无休无止地进行下去
所以，如果发现从A点出发，在**第n轮**时松弛操作依然存在，说明从A点出发能够到达一个负环

### *SPFA优化：

每一轮考察所有的边看看能否做松弛操作时不必要的
因为只有上一次被某条边松弛过的节点所连接的边，才有可能引起下一次的松弛操作
所以用队列来维护”这一轮哪些节点的distance变小了“
下一轮只需要对这些点的所有边，考察有没有松弛操作即可

只优化了常数时间复杂度O（MN）只适用于小图，没有负权边优先Dijkstra

**用途**：
1.适用于小图
2.解决有负边（无负环）的图的单源最短路径问题
3.可以判断从某个点出发是否能遇到负环，**如果想判断整张图有向图有没有负环，需要设置虚拟源点**
4.并行计算时会有很大优势，因为每一轮多点判断松弛操作是相互独立的，可以交给多线程处理

虚拟源点：到图中所有点的距离都是0



# *KMP：匹配字符串

nt[i]存储子串pattern[0 : i]的最长公共前后缀长度。当然，默认nt[0]=-1， nt[1]=0

```python
def nextarray(s):
    m=len(s)
    if m==1:
        return [-1]
    nt=[0]*m
    nt[0],nt[1]=-1,0
    i,cn=2,0
    while i<m:
        if s[i-1]==s[cn]:
            cn+=1
            nt[i]=cn
            i+=1
        elif cn>0:
            cn=nt[cn]
        else:
            nt[i]=0
            i+=1
    return nt
```

```python
def kmp(s1,s2):
    n,m=len(s1),len(s2)
    x,y=0,0
    nt=nextarray(s2)
    while x<n and y<m:
        if s1[x]==s2[y]:
            x+=1; y+=1
        elif y==0:
            x+=1
        else:
            y=nt[y]
        #如果允许匹配多个：while语句删去“y<m”
        #if y==m:
        #	ans.append(x-y)
        #   y=nt[y] （当允许重复索引字符）；y=0 (不允许重复)
    return x-y if y==m else -1
```

## *Manacher：寻找最长回文串

```python
def manacher(s):
    ss= '#' + '#'.join(s) + '#'
    n=len(ss)
    p=[0]*n
    ans=0
    c,r=0,0
    for i in range(n):
        length=min(p[2*c-i],r-i) if r>i else 1
        while i+length<n and i-length>=0 and ss[i + length]==ss[i - length]:
            length+=1
        if i+length>r:
            r=i+length
            c=i
        ans=max(ans,length)
        p[i]=length
    return ans-1
```

# *手搓heap

```python
class xiaoheap:
    def __init__(self):
        self.heaplist=[0]
        self.size=0
    def percup(self,i):
        while i//2>0:
            if self.heaplist[i]<self.heaplist[i//2]:
                self.heaplist[i],self.heaplist[i//2]=self.heaplist[i//2],self.heaplist[i]
            i//=2
    def insert(self,i):
        self.heaplist.append(i)
        self.size+=1
        self.percup(self.size)
    def percdown(self,i):
        while (i*2)<=self.size:
            mc=self.minchild(i)
            if self.heaplist[i]>self.heaplist[mc]:
                self.heaplist[i],self.heaplist[mc]=self.heaplist[mc],self.heaplist[i]
            i=mc
    def minchild(self,i):
        if (i*2+1)>self.size:
            return i*2
        else:
            return i*2 if self.heaplist[i*2]<self.heaplist[i*2+1] else i*2+1
    def delmin(self):
        m=self.heaplist[1]
        self.heaplist[1]=self.heaplist[self.size]
        self.size-=1
        self.heaplist.pop()
        self.percdown(1)
        return m
    def buildheap(self,alist):
        i=len(alist)//2
        self.size=len(alist)
        self.heaplist=0+alist[:]
        while i>0:
            self.percdown(i)
            i-=1
```
