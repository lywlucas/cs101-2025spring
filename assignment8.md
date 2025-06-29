# Assignment #8: 树为主

Updated 1704 GMT+8 Apr 8, 2025

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

### LC108.将有序数组转换为二叉树

dfs, https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/

思路：

既然已经有序了，那我们只需简单二分即可建树

代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        def Do(left,right):
            if left>right:
                return None
            mid=(left+right)//2
            root=TreeNode(nums[mid])
            root.left=Do(left,mid-1)
            root.right=Do(mid+1,right)
            return root

        return Do(0,len(nums)-1)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250409190933769](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250409190933769.png)



### M27928:遍历树

 adjacency list, dfs, http://cs101.openjudge.cn/practice/27928/

思路：

唯一碰到的难点是，如何避免节点的重复建立与覆盖。最后想了想还是用dict

代码：

```python
d=dict()
class Node:
    def __init__(self, value):
        self.value = value
        self.parent = None
        self.children = []
        if value not in d:
            d[value] = self
    def sorted_moves(self):
        self.children.append(self)
        moves = sorted(self.children, key=lambda x: x.value)
        self.children.pop()
        return moves
class Tree:
    def __init__(self):
        self.root = None
    def insert(self, adj):
        root = d.get(adj[0],Node(adj[0]))
        for num in adj[1:]:
            now=d.get(num,Node(num))
            root.children.append(now)
            now.parent = root
        if self.root is None:
            self.root = root
        while self.root.parent:
            self.root = self.root.parent
    def recursion(self, node):
        if not node.children:
            return [node.value]
        ans=[]
        for i in node.sorted_moves():
            if i==node:
                ans+=[node.value]
            else:
                ans+=self.recursion(i)
        return ans
def main():
    n=int(input())
    t=Tree()
    for i in range(n):
        adj=list(map(int,input().split()))
        t.insert(adj)
    final=t.recursion(t.root)
    print(*final,sep='\n')

if __name__ == '__main__':
    main()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250409204034329](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250409204034329.png)



### LC129.求根节点到叶节点数字之和

dfs, https://leetcode.cn/problems/sum-root-to-leaf-numbers/

思路：

这题还是挺水的，简单dfs即可

代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sumNumbers(self, root: Optional[TreeNode]) -> int:
        def dfs(node,num=0):
            new=num*10+node.val
            ans=0
            if node.left:
                ans+=dfs(node.left,new)
            if node.right:
                ans+=dfs(node.right,new)
            if not node.left and not node.right:
                ans=new
            return ans
        return dfs(root)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250409205452435](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250409205452435.png)



### M22158:根据二叉树前中序序列建树

tree, http://cs101.openjudge.cn/practice/22158/

思路：

浙江的同学表示，这玩意在高中技术课上就做过了

代码：

```python
def tree(mid:str,pre:str):
    if mid==pre:
        return mid[::-1]
    pt=mid.find(pre[0])
    return tree(mid[:pt],pre[1:pt+1])+tree(mid[pt+1:],pre[pt+1:])+pre[0]
while True:
    try:
        a=input()
        b=input()
        print(tree(b,a))
    except EOFError:
        break
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250409205613008](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250409205613008.png)



### T24729:括号嵌套树

dfs, stack, http://cs101.openjudge.cn/practice/24729/

思路：

整体感觉比较简单，为什么会给出Tough的评级？

前序表达式就是输入给的顺序；后序表达式只需建栈，然后每次遇到" ) "或" , "时出栈即可

代码：

```python
def behind_order(s):
    t=[]
    ans=""
    for ss in s:
        if ss==')' or ss==',':
            ans+=t[-1]
            t.pop()
        elif 'A'<=ss<='Z':
            t.append(ss)
    ans+=t[-1]
    return ans
def front_order(s):
    ans=""
    for ss in s:
        if 'A'<=ss<='Z':
            ans+=ss
    return ans
def main():
    s=input()
    print(front_order(s))
    print(behind_order(s))

if __name__ == '__main__':
    main()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250409210941393](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250409210941393.png)



### LC3510.移除最小数对使数组有序II

doubly-linked list + heap, https://leetcode.cn/problems/minimum-pair-removal-to-sort-array-ii/

思路：

这题做下来确实难。比较自然的是维护一个最小堆，来模拟操作顺序；

至于区间合并和查询比较，一开始想写线段树的，后面越写越别扭(太不熟了qwq)，遂转战双向链表。

用两个数组模拟双向链表，分别维护当前点的前后指标；

比较关键的部分是取一个特征量cnt，用于表达整个数组中“相邻逆序” 发生的次数。这样做的好处是，每一次操作对于cnt的修改都是O(1)的。

其实我最后卡在了怎么从堆中删除当前操作以外的数据（这时候开始怀念SortedList了），参考了题解才发现，其实并不需要实时删除，只需修改双向链表的值，懒删除即可。

代码：

```python
class Solution:
    def minimumPairRemoval(self, nums: List[int]) -> int:
        import heapq
        n=len(nums)
        h=[(nums[x]+nums[x+1],x) for x in range(n-1)]
        heapq.heapify(h)

        cnt=0
        for i in range(n-1):
            if nums[i]>nums[i+1]:
                cnt+=1

        left=list(range(-1,n))
        right=list(range(1,n+1))

        ans=0
        while cnt:
            ans+=1
            while right[h[0][1]]>=n or h[0][0] !=nums[h[0][1]]+nums[right[h[0][1]]]:
                heapq.heappop(h)
            s,i=heapq.heappop(h)

            nxt=right[i]
            if nums[i]>nums[nxt]:
                cnt-=1
            pre=left[i]
            if pre>=0:
                if nums[pre]>nums[i]:
                    cnt-=1
                if nums[pre]>s:
                    cnt+=1
                heapq.heappush(h,(nums[pre]+s,pre))
            nnxt=right[nxt]
            if nnxt<n:
                if nums[nxt]>nums[nnxt]:
                    cnt-=1
                if s>nums[nnxt]:
                    cnt+=1
                heapq.heappush(h,(nums[nnxt]+s,i))
            nums[i]=s
            l,r=left[nxt],right[nxt]
            right[l]=r
            left[r]=l
            right[nxt]=n
        return ans
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250410095345170](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250410095345170.png)

看完题解发现线段树也是可以的……

照着题解也写了一遍，重新学习一下



## 2. 学习总结和收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025spring每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>

最后一题太折磨了/(ㄒoㄒ)/~~

不过也借此重温了懒删除堆、双向链表，以及线段树的写法；

希望期末考不要出现这样难度的题







