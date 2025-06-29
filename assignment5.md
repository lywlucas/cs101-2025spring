# Assignment #5: 链表、栈、队列和归并排序

Updated 1348 GMT+8 Mar 17, 2025

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

### LC21.合并两个有序链表

linked list, https://leetcode.cn/problems/merge-two-sorted-lists/

思路：

非常丑陋的AC，因为讨论None节点占据了大部分的篇幅。

相比之下，题解就显得高明多了

代码：

```python
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        A,B=list1,list2
        if A==None:
            return B
        if B==None:
            return A 
        head=A if A.val<=B.val else B
        
        while A and B:
            if A.val<=B.val:
                cur=A
                A=A.next
            else:
                cur=B
                B=B.next
            if A==None:
                cur.next=B
            elif B==None:
                cur.next=A
            elif A.val<=B.val:
                cur.next=A
            else:
                cur.next=B
        return head
        
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250319171716331](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250319171716331.png)



### LC234.回文链表

linked list, https://leetcode.cn/problems/palindrome-linked-list/

<mark>请用快慢指针实现。</mark>

很容易想到快慢指针找中间元；再把上周反转链表的函数复制过来即可

代码：

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        pre=None
        cur=head
        while cur:
            b=cur.next
            cur.next=pre
            pre=cur
            cur=b
        return pre
    def findHalf(self,head):
        fast,slow=head,head
        while fast.next and fast.next.next:
            fast=fast.next.next
            slow=slow.next
        return slow
    def isPalindrome(self, head: Optional[ListNode]) -> bool:
        half=self.findHalf(head)
        end=self.reverseList(half.next)
        A,B=head,end
        flg=True
        while flg and B:
            if A.val!=B.val:
                flg=False
            A=A.next
            B=B.next
        return flg
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250319185152406](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250319185152406.png)



### LC1472.设计浏览器历史记录

doubly-lined list, https://leetcode.cn/problems/design-browser-history/

<mark>请用双链表实现。</mark>

感觉双链表也没比内建list类型快

代码：

```python
class Node:
    def __init__(self,item,nxt=None,prev=None):
        self.item=item
        self.next=nxt
        self.prev=prev

class BrowserHistory:

    def __init__(self, homepage: str):
        node=Node(item=homepage)
        self._cur=node

    def visit(self, url: str) -> None:
        node=Node(item=url,prev=self._cur)
        self._cur.next=node
        self._cur=node

    def forward(self, steps: int) -> str:
        i=0
        node=self._cur
        while node.next and i<steps:
            node=node.next
            i+=1
        self._cur=node
        return self._cur.item

    def back(self, steps: int) -> str:
        i=0
        while self._cur.prev and i<steps:
            self._cur=self._cur.prev
            i+=1
        return self._cur.item


# Your BrowserHistory object will be instantiated and called as such:
# obj = BrowserHistory(homepage)
# obj.visit(url)
# param_2 = obj.back(steps)
# param_3 = obj.forward(steps)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250319193059410](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250319193059410.png)



### 24591: 中序表达式转后序表达式

stack, http://cs101.openjudge.cn/practice/24591/

思路：

经典

代码：

```python
def nomalize(s):
    ans_list=[]
    now=""
    for ss in s:
        if ss==" ":
            continue
        if ss in ["(",")","+","-","*","/"]:
            if now:
                ans_list.append(now)
                now=""
            ans_list.append(ss)
        else:
            now+=ss
    if now:
        ans_list.append(now)
    return ans_list
n=int(input())
for _ in range(n):
    s=input()
    l=nomalize(s)
    my_stack=[]
    ans=""
    for i in l:
        if i in ["+","-"]:
            while my_stack and my_stack[-1] in ["+","-","*","/"]:
                a=my_stack.pop()
                ans+=" "+a
            my_stack.append(i)
        elif i in ["*","/"]:
            while my_stack and my_stack[-1] in ["*","/"]:
                a=my_stack.pop()
                ans+=" "+a
            my_stack.append(i)
        elif i==')':
            while my_stack[-1]!="(":
                a = my_stack.pop()
                ans += " " + a
            my_stack.pop()
        elif i=='(':
            my_stack.append(i)
        else:
            ans+=" "+i
    while my_stack:
        a=my_stack.pop()
        ans+=" "+a
    print(ans[1:])
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250319193329788](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250319193329788.png)



### 03253: 约瑟夫问题No.2

queue, http://cs101.openjudge.cn/practice/03253/

<mark>请用队列实现。</mark>



代码：

```python
from collections import deque
while True:
    n,p,m=map(int,input().split())
    if n==p==m==0:
        break
    q=deque([i%n for i in range(p,n+p)])
    ans=[]
    while q:
        for i in range(m):
            p=q.popleft()
            if i!=m-1:
                q.append(p)
            elif p==0:
                ans.append(n)
            else:
                ans.append(p)
    print(*ans,sep=",")
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>



![image-20250319194729991](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250319194729991.png)

### 20018: 蚂蚁王国的越野跑

merge sort, http://cs101.openjudge.cn/practice/20018/

思路：

一直在想和归并排序有什么关系，最后一看群里原来就是个二分查找

代码：

```python
from bisect import bisect_left
n=int(input())
l=[]
ans=0
for i in range(n):
    p=int(input())
    index=bisect_left(l,p)
    l.insert(index,p)
    ans+=index
print(ans)

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250319204747196](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250319204747196.png)



## 2. 学习总结和收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025spring每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>

LC上面几道链表的题让我对class构建、oop编程有了更加深入的理解









