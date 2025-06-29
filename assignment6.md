# Assignment #6: 回溯、树、双向链表和哈希表

Updated 1526 GMT+8 Mar 22, 2025

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

### LC46.全排列

backtracking, https://leetcode.cn/problems/permutations/

思路：

经典

代码：

```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        def dfs(nums,cur,ans):
            if len(nums)==len(cur):
                ans.append(cur[:])
                return
            for i in nums:
                if i not in cur:
                    cur.append(i)
                    dfs(nums,cur,ans)
                    cur.pop()
        ans=[]
        cur=[]
        dfs(nums,cur,ans)
        return ans
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250324111550759](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250324111550759.png)



### LC79: 单词搜索

backtracking, https://leetcode.cn/problems/word-search/

思路：

dfs

代码：

```python
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        m,n=len(board),len(board[0])
        def dfs(i,j,k):
            if board[i][j]!=word[k]:
                return False
            if k==len(word)-1:
                return True
            board[i][j]=""
            for (dx,dy) in [(0,1),(0,-1),(-1,0),(1,0)]:
                x,y=i+dx,j+dy
                if 0<=x<m and 0<=y<n and dfs(x,y,k+1):
                    return True
            board[i][j]=word[k]
            return False
        for i in range(m):
            for j in range(n):
                if dfs(i,j,0):
                    return True
        return False
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250324120056770](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250324120056770.png)



### LC94.二叉树的中序遍历

dfs, https://leetcode.cn/problems/binary-tree-inorder-traversal/

思路：

感觉更像迭代而非dfs

代码：

```python
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        if root==None:
            return []
        l,r=root.left,root.right
        return self.inorderTraversal(root=l)+[root.val]+self.inorderTraversal(root=r)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250324195413938](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250324195413938.png)



### LC102.二叉树的层序遍历

bfs, https://leetcode.cn/problems/binary-tree-level-order-traversal/

思路：

没什么花里胡哨的，唯一需要注意一下的是浅拷贝问题

代码：

```python
class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if root==None:
            return []
        cur=[root]
        ans=[]
        while cur:
            ans.append([node.val for node in cur])
            para=[]
            for node in cur:
                l,r=node.left,node.right
                if l!=None:
                    para.append(l)
                if r!=None:
                    para.append(r)
            cur=para[:]
        return ans
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250324201134119](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250324201134119.png)



### LC131.分割回文串

dp, backtracking, https://leetcode.cn/problems/palindrome-partitioning/

思路：

估算了一下，暴力穷举也不过1e6的计算量，那甚至都不用dp

看了眼题解，确实能够节省更多计算量。

代码：

```python
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        import itertools
        n=len(s)
        if n==1:
            return [[s]]
        chances=itertools.product([True,False],repeat=n-1)
        ans=[]
        for a in chances:
            cur=s[0]
            now=[]
            for i in range(n-1):
                if a[i]:
                    cur+=s[i+1]
                else:
                    if cur==cur[::-1]:
                        now.append(cur)
                        cur=s[i+1]
                    else:
                        break
            if cur==cur[::-1]:
                now.append(cur)
                ans.append(now)
        return ans
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250325164636555](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250325164636555.png)



### LC146.LRU缓存

hash table, doubly-linked list, https://leetcode.cn/problems/lru-cache/

思路：

凭感觉写的双链表+字典，写出来结构居然跟题解几乎一样

代码：

```python
class DoubleLinkedNode:

    def __init__(self, key=0, value=0):
        self.key = key
        self.value = value
        self.pre = None
        self.nxt = None

class LRUCache:

    def __init__(self, capacity: int):
        self.dict = dict()
        self.head = DoubleLinkedNode()
        self.tail = DoubleLinkedNode()
        self.head.nxt = self.tail
        self.tail.pre = self.head
        self.capacity = capacity
        self.size = 0

    def get(self, key: int) -> int:
        if key not in self.dict:
            return -1
        node=self.dict[key]
        self.Remove(node)
        self.AddtoHead(node)
        return node.value

    def put(self, key: int, value: int) -> None:
        if key not in self.dict:
            node=DoubleLinkedNode(key=key,value=value)
            self.dict[key]=node
            self.AddtoHead(node)
            self.size+=1
            if self.size>self.capacity:
                r=self.tail.pre
                self.Remove(r)
                self.dict.pop(r.key)
                self.size-=1
        else:
            node=self.dict[key]
            node.value=value
            self.Remove(node)
            self.AddtoHead(node)
    
    def Remove(self,node):
        node.pre.nxt=node.nxt
        node.nxt.pre=node.pre    

    def AddtoHead(self,node):
        node.nxt=self.head.nxt
        node.pre=self.head
        self.head.nxt.pre=node
        self.head.nxt=node

# Your LRUCache object will be instantiated and called as such:
# obj = LRUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250326151055121](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250326151055121.png)



## 2. 学习总结和收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025spring每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>

这次作业做起来还挺快的

也是更加熟悉了Leetcode这里编程的模式









