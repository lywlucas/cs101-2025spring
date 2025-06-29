# Assignment #9: Huffman, BST & Heap

Updated 1834 GMT+8 Apr 15, 2025

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

### LC222.完全二叉树的节点个数

dfs, https://leetcode.cn/problems/count-complete-tree-nodes/

思路：

O(n)的遍历代码是简单的；既然这已经符合了时间复杂度要求，就不必再去写二分查找了

代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def countNodes(self, root: Optional[TreeNode]) -> int:
        def dfs(r):
            if r is None:
                return 0
            ans=1
            ans+=dfs(r.left)+dfs(r.right)
            return ans
        return dfs(root)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250415192044220](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250415192044220.png)



### LC103.二叉树的锯齿形层序遍历

bfs, https://leetcode.cn/problems/binary-tree-zigzag-level-order-traversal/

思路：



代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def zigzagLevelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        l=[]
        h=0
        cur=[] if root is None else [root]
        while cur:
            h+=1
            now=[]
            ans=[]
            for node in cur:
                ans.append(node.val)
                if node.left is not None:
                    now.append(node.left)
                if node.right is not None:
                    now.append(node.right)
            cur=now[:]
            if h%2==0:
                ans=ans[::-1]
            l.append(ans)
        return l
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250416184015899](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250416184015899.png)



### M04080:Huffman编码树

greedy, http://cs101.openjudge.cn/practice/04080/

思路：

题目等价于：给一组数，每次操作从数组中去除两个数a,b,记录它们的和s=a+b，并将s放回数组中；

一直操作直到数组内只剩一个数，求所有记下的s之和的最小值

代码：

```python
import heapq
n=int(input())
l=list(map(int,input().split()))
heapq.heapify(l)
ans=0
while len(l)>1:
    a=heapq.heappop(l)
    b=heapq.heappop(l)
    heapq.heappush(l,a+b)
    ans+=a+b
print(ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250416185906682](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250416185906682.png)



### M05455: 二叉搜索树的层次遍历

http://cs101.openjudge.cn/practice/05455/

思路：

建二叉树，甚至都不要求是平衡二叉树。

insert过程中把value==node.value的情况特判一下就行

代码：

```python
class TreeNode:
    def __init__(self, value, left=None, right=None):
        self.value = value
        self.left = left
        self.right = right

class Tree:
    def __init__(self):
        self.root = None
    def insert(self, value):
        if not self.root:
            self.root = TreeNode(value)
        else:
            self.root = self._insert(value, self.root)
    def _insert(self, value, node):
        if not node:
            return TreeNode(value)
        elif value < node.value:
            node.left = self._insert(value, node.left)
        elif value > node.value:
            node.right = self._insert(value, node.right)
        else:
            return node
    def level_order(self):
        from collections import deque
        root=self.root
        if not root:
            return []
        result=[]
        queue=deque([root])
        while queue:
            n=len(queue)
            level=[]
            for _ in range(n):
                node=queue.popleft()
                level.append(node.value)
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            result+=level
        return result

def main():
    l=list(map(int,input().split()))
    t=Tree()
    for i in l:
        t.insert(i)
    print(" ".join(map(str,t.level_order())),end="")

if __name__=='__main__':
    main()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250416193752950](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250416193752950.png)



### M04078: 实现堆结构

手搓实现，http://cs101.openjudge.cn/practice/04078/

类似的题目是 晴问9.7: 向下调整构建大顶堆，https://sunnywhy.com/sfbj/9/7

思路：

艰难地手搓了一个，过程中还是瞄了几眼讲义

核心思路还是不难理解的

代码：

```python
class BinHeap:
    def __init__(self):
        self.heapList=[0]
        self.currentSize=0

    def percUp(self, i):
        while i//2 > 0:
            if self.heapList[i]<self.heapList[i//2]:
                temp=self.heapList[i//2]
                self.heapList[i//2]=self.heapList[i]
                self.heapList[i]=temp
            i=i//2

    def insert(self, k):
        self.heapList.append(k)
        self.currentSize+=1
        self.percUp(self.currentSize)

    def minChild(self, i):
        if i*2+1>self.currentSize:
            return i*2
        else:
            if self.heapList[i*2]<self.heapList[i*2+1]:
                return i*2
            else:
                return i*2+1

    def percDown(self, i):
        while i*2 <= self.currentSize:
            mc=self.minChild(i)
            if self.heapList[i]>self.heapList[mc]:
                temp=self.heapList[i]
                self.heapList[i]=self.heapList[mc]
                self.heapList[mc]=temp
            i=mc

    def delMin(self):
        retval=self.heapList[1]
        self.heapList[1]=self.heapList[self.currentSize]
        self.currentSize-=1
        self.heapList.pop()
        self.percDown(1)
        return retval

    def buildHeap(self,alist):
        i=len(alist)//2
        self.heapList=[0]+alist[:]
        self.currentSize=len(alist)
        while i>0:
            self.percDown(i)
            i-=1

n=int(input().strip())
bh=BinHeap()
for _ in range(n):
    inp=input().strip()
    if inp[0]=='1':
        bh.insert(int(inp.split()[1]))
    else:
        print(bh.delMin())

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250416200033371](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250416200033371.png)



### T22161: 哈夫曼编码树

greedy, http://cs101.openjudge.cn/practice/22161/

思路：

跟着搓了一个Huffman编码树

代码：

```python
import heapq
class Node:
    def __init__(self, weight, char=None):
        self.weight = weight
        self.char = char
        self.left = None
        self.right = None

    def __lt__(self, other):
        if self.weight == other.weight:
            return self.char < other.char
        return self.weight < other.weight

def build_huffman_tree(characters):
    heap=[]
    for char, weight in characters.items():
        heapq.heappush(heap, Node(weight, char))
    while len(heap) > 1:
        left = heapq.heappop(heap)
        right = heapq.heappop(heap)
        merged=Node(left.weight+right.weight, min(left.char, right.char))
        merged.left = left
        merged.right = right
        heapq.heappush(heap, merged)
    return heap[0]

def encode_huffman_tree(root):
    codes={}

    def traverse(node,code):
        if node.left is None and node.right is None:
            codes[node.char] = code
        else:
            traverse(node.left,code+'0')
            traverse(node.right,code+'1')

    traverse(root,'')
    return codes

def huffman_encoding(codes, string):
    encoded = ''
    for char in string:
        encoded += codes[char]
    return encoded

def huffman_decoding(root, encoded_string):
    decoded = ''
    node=root
    for bit in encoded_string:
        if bit == '0':
            node = node.left
        else:
            node = node.right

        if node.left is None and node.right is None:
            decoded += node.char
            node = root
    return decoded

def main():
    n=int(input())
    characters={}
    for _ in range(n):
        char,weight=input().split()
        characters[char]=int(weight)
    huffman_tree=build_huffman_tree(characters)
    codes=encode_huffman_tree(huffman_tree)
    strings=[]
    while True:
        try:
            line=input()
            strings.append(line)
        except EOFError:
            break
    results=[]
    for string in strings:
        if string[0] in ('0','1'):
            results.append(huffman_decoding(huffman_tree, string))
        else:
            results.append(huffman_encoding(codes, string))
    for result in results:
        print(result)

if __name__ == '__main__':
    main()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250416203146802](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250416203146802.png)



## 2. 学习总结和收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025spring每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>











