# Assignment #A: Graph starts

Updated 1830 GMT+8 Apr 22, 2025

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

### M19943:图的拉普拉斯矩阵

OOP, implementation, http://cs101.openjudge.cn/practice/19943/

要求创建Graph, Vertex两个类，建图实现。

思路：

说实话，直接维护一个Laplace矩阵，每次加边对于它的修改都是O(1)的。那样代码会更加简洁。

不过这样OOP也有其清晰易读、易于其他地方调用的好处

代码：

```python
class Vertex:
    def __init__(self, name):
        self.name = name
        self.degree = 0
        self.neighbors = []
    def add_neighbor(self, vertex):
        self.neighbors.append(vertex)
        self.degree += 1
    def adj(self,n):
        l=[0]*n
        for neighbor in self.neighbors:
            l[neighbor.name]-=1
        l[self.name]=self.degree
        return l

class Graph:
    def __init__(self):
        self.vertices = {}
    def add_vertex(self, vertex):
        self.vertices[vertex.name] = vertex
    def find_vertex(self, name):
        return self.vertices[name]
    def add_edge(self, vertex1_name, vertex2_name):
        vertex1, vertex2 = self.find_vertex(vertex1_name), self.find_vertex(vertex2_name)
        vertex1.add_neighbor(vertex2)
        vertex2.add_neighbor(vertex1)
    def get_Laplace(self,n):
        ans=[]
        for vertex in self.vertices.values():
            ans.append(vertex.adj(n))
        return ans

def main():
    g=Graph()
    n,m=map(int,input().split())
    for i in range(n):
        g.add_vertex(Vertex(i))
    for j in range(m):
        a,b=map(int,input().split())
        g.add_edge(a,b)
    Laplace=g.get_Laplace(n)
    for i in range(n):
        print(*Laplace[i])

if __name__=='__main__':
    main()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250425143652145](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250425143652145.png)



### LC78.子集

backtracking, https://leetcode.cn/problems/subsets/

思路：

出题者的本意应该是用dfs枚举所有的可能；奈何 [true，false] 的笛卡尔积已经完成了这一点

代码：

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        import itertools
        n=len(nums)
        chances=itertools.product([True,False],repeat=n)
        results=[]
        for chance in chances:
            subset=[]
            for i in range(n):
                if chance[i]:
                    subset.append(nums[i])
            results.append(subset)
        return results
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250425144811186](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250425144811186.png)



### LC17.电话号码的字母组合

hash table, backtracking, https://leetcode.cn/problems/letter-combinations-of-a-phone-number/

思路：

笛卡尔积还是太好用了

代码：

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if len(digits)==0:
            return []
        d={"2": "abc","3": "def","4": "ghi", "5": "jkl","6": "mno","7": "pqrs","8": "tuv","9": "wxyz"}
        tries=(d[i] for i in digits)
        l=itertools.product(*tries)
        return ["".join(chance) for chance in l]
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250425150912221](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250425150912221.png)



### M04089:电话号码

trie, http://cs101.openjudge.cn/practice/04089/

思路：

这里特意把 insert 和 search 分开写，也是保留了Trie本身的完整性。

代码：

```python
class TrieNode:
    def __init__(self):
        self.children = {}

class Trie:
    def __init__(self):
        self.root = TrieNode()
    def insert(self, word):
        node = self.root
        for letter in word:
            if letter not in node.children:
                node.children[letter] = TrieNode()
            node = node.children[letter]
    def search(self, word):
        node = self.root
        for letter in word:
            if letter not in node.children:
                return False
            node = node.children[letter]
        return True

def main():
    t=int(input())
    for _ in range(t):
        n=int(input())
        nums=[]
        for _ in range(n):
            nums.append(str(input()))
        nums.sort(reverse=True)
        trie=Trie()
        for num in nums:
            if trie.search(num):
                print("NO")
                break
            trie.insert(num)
        else:
            print("YES")

if __name__=='__main__':
    main()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250425201910781](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250425201910781.png)



### T28046:词梯

bfs, http://cs101.openjudge.cn/practice/28046/

思路：

正常建图，美美TLE

第一版代码：

```python
from collections import deque
class Vertex:
    def __init__(self, name):
        self.name = name
        self.neighbors = []
    def add_neighbor(self, neighbor):
        self.neighbors.append(neighbor)
    def is_connected(self, other):
        s1,s2=self.name,other.name
        if len(s1)!=len(s2):
            return False
        cnt=0
        for i in range(len(s1)):
            if s1[i]==s2[i]:
                cnt+=1
        if cnt==len(s1)-1:
            return True
        return False

class Graph:
    def __init__(self):
        self.vertices = {}
    def add_vertex(self, name):
        self.vertices[name] = Vertex(name)
        return self.vertices[name]
    def add_edge(self, vertex1, vertex2):
        vertex1.add_neighbor(vertex2)
        vertex2.add_neighbor(vertex1)
    def insert_word(self,name):
        now=self.add_vertex(name)
        for v in self.vertices.values():
            if v.is_connected(now):
                self.add_edge(now,v)
    def bfs(self,start,end):
            queue=deque([[start]])
            in_queue={start}
            while queue:
                path=queue.popleft()
                cur=path[-1]
                if cur.name==end.name:
                    return path
                for neighbor in cur.neighbors:
                    if neighbor not in in_queue:
                        queue.append(path+[neighbor])
                        in_queue.add(neighbor)
            return None
    def word_ladder(self,name1,name2):
        start=self.vertices[name1]
        end=self.vertices[name2]
        ans=self.bfs(start,end)
        if ans is None:
            return "NO"
        else:
            l=[v.name for v in ans]
            return " ".join(l)

def main():
    n=int(input())
    g=Graph()
    for _ in range(n):
        word=input()
        g.insert_word(word)
    start,end=input().split()
    print(g.word_ladder(start,end))

if __name__=='__main__':
    main()
```



学习了一番桶的用法，于是采用defaultdict，用 “*ool”的键来储存所有本形式的词fool,tool等;

另外，将回溯过程挂在每个节点的previous值上，而非deque里，有利于节省内存

第二版代码：

```python
from collections import defaultdict,deque

buckets=defaultdict(list)
for _ in range(int(input())):
    word=input()
    for i in range(len(word)):
        buckets[word[:i]+'*'+word[i+1:]].append(word)
x,y=input().split()
pre={x:x}
q=deque([x])
flg=False
while q:
    word=q.popleft()
    if word==y:
        flg=True
        break
    for i in range(len(word)):
        for w in buckets[word[:i]+'*'+word[i+1:]]:
            if w not in pre:
                pre[w]=word
                q.append(w)
if flg:
    ans=[y]
    while y!=x:
        y=pre[y]
        ans.append(y)
    ans.reverse()
    print(*ans)
else:
    print("NO")
```

代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250426171248567](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250426171248567.png)



### T51.N皇后

backtracking, https://leetcode.cn/problems/n-queens/

思路：

暴力枚举+判定的方法是O(n!*n^2)的，这在n<=9的情形下刚好得以通过

代码：

```python
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        from itertools import permutations
        ans=[]
        def is_permitted(x,n):
            for i in range(n-1):
                for j in range(i+1,n):
                    if abs(x[i]-x[j])==j-i:
                        return False
            return True
        def normalize(x,n):
            a=[]
            for i in range(n):
                s=['.']*n
                s[x[i]]='Q'
                a.append(''.join(s))
            return a
        l=permutations(range(n),n)
        for chance in l:
            if is_permitted(chance,n):
                ans.append(normalize(chance,n))
        return ans
        
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250426173148164](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250426173148164.png)

当然dfs可以直接把时间复杂度降到O(n!*n)；

```
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        chess=[['.']*n for _ in range(n)]
        ans=[]
        def dfs(row,chess):
            if row==n:
                ans.append([''.join(r) for r in chess])
                return
            for col in range(n):
                if is_permitted(row,col,chess):
                    chess[row][col]='Q'
                    dfs(row+1,chess)
                    chess[row][col]='.'
        def is_permitted(row,col,chess):
            for i in range(row):
                if chess[i][col]=='Q':
                    return False
            for j in range(col):
                if chess[row][j]=='Q':
                    return False
            i, j = row - 1, col - 1
            while i >= 0 and j >= 0:
                if chess[i][j] == 'Q':
                    return False
                i -= 1
                j -= 1
            i, j = row - 1, col + 1
            while i >= 0 and j < n:
                if chess[i][j] == 'Q':
                    return False
                i -= 1
                j += 1
            return True
        dfs(0,chess)
        return ans
```

![image-20250426174420029](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250426174420029.png)

看了眼正解的O(n!)，就是采用数组储存已经到达过的行列与对角线，这样判定的时间复杂度降为O(1)

话说当年八皇后是不是就该这么写的

## 2. 学习总结和收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025spring每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>



本周作业题还是比较难的，主要学习了有关图的概念，以及重温了回溯算法。







