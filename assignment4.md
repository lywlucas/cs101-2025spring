# Assignment #4: 位操作、栈、链表、堆和NN

Updated 1203 GMT+8 Mar 10, 2025

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

### 136.只出现一次的数字

bit manipulation, https://leetcode.cn/problems/single-number/



<mark>请用位操作来实现，并且只使用常量额外空间。</mark>

经典异或操作

代码：

```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        ans=0
        for i in nums:
            ans=ans^i
        return ans
        
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250312144703790](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250312144703790.png)



### 20140:今日化学论文

stack, http://cs101.openjudge.cn/practice/20140/



思路：

本质上就是括号匹配

代码：

```python
def uncode(a):
    num=0
    i=0
    while '0'<=a[i]<='9':
        num=num*10+int(a[i])
        i+=1
    return a[i:]*num

s=input()
l=[""]
for ss in s:
    if ss=="[":
        l.append("")
    elif ss=="]":
        ans=uncode(l[-1])
        l.pop()
        l[-1]+=ans
    else:
        l[-1]+=ss
print(l[0])
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250312150124556](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250312150124556.png)



### 160.相交链表

linked list, https://leetcode.cn/problems/intersection-of-two-linked-lists/



思路：

这题还是有点小巧思在里面的。

因为链表不能从后往前遍历，就要想办法把两个链表前后“对齐”，很自然想到首尾相接，A+B和B+A是对齐的

代码：

```python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
        A,B=headA,headB
        while A!=B:
            if A:
                A=A.next
            else:
                A=headB
            if B:
                B=B.next
            else:
                B=headA
        return A
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250312152531200](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250312152531200.png)



### 206.反转链表

linked list, https://leetcode.cn/problems/reverse-linked-list/



思路：

朴实无华的反转链表

代码：

```python
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
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250312153405013](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250312153405013.png)



### 3478.选出和最大的K个元素

heap, https://leetcode.cn/problems/choose-k-elements-with-maximum-sum/



思路：

维护一个ksmallest heap

代码：

```python
class Solution:
    def findMaxSum(self, nums1: List[int], nums2: List[int], k: int) -> List[int]:
        n=len(nums1)
        idx=sorted(range(n), key = lambda x: nums1[x])
        heap=[]
        cur=0
        ans=[0]*n
        for (i,ni) in enumerate(idx):
            pre=idx[i-1]
            if i>0 and nums1[ni]==nums1[pre]:
                ans[ni]=ans[pre]
            else:
                ans[ni]=cur
            cur+=nums2[ni]
            heapq.heappush(heap,nums2[ni])
            if len(heap)>k:
                cur-=heapq.heappop(heap)
        return ans
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![image-20250312163535087](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250312163535087.png)



### Q6.交互可视化neural network

https://developers.google.com/machine-learning/crash-course/neural-networks/interactive-exercises

**Your task:** configure a neural network that can separate the orange dots from the blue dots in the diagram, achieving a loss of less than 0.2 on both the training and test data.

**Instructions:**

In the interactive widget:

1. Modify the neural network hyperparameters by experimenting with some of the following config settings:
   - Add or remove hidden layers by clicking the **+** and **-** buttons to the left of the **HIDDEN LAYERS** heading in the network diagram.
   - Add or remove neurons from a hidden layer by clicking the **+** and **-** buttons above a hidden-layer column.
   - Change the learning rate by choosing a new value from the **Learning rate** drop-down above the diagram.
   - Change the activation function by choosing a new value from the **Activation** drop-down above the diagram.
2. Click the Play button above the diagram to train the neural network model using the specified parameters.
3. Observe the visualization of the model fitting the data as training progresses, as well as the **Test loss** and **Training loss** values in the **Output** section.
4. If the model does not achieve loss below 0.2 on the test and training data, click reset, and repeat steps 1–3 with a different set of configuration settings. Repeat this process until you achieve the preferred results.

### 体验

学习速率似乎对应着物理中的“微扰”。学习速率过高/数据批次过小，则会出现持续的波动效应，甚至随着训练量增加，损失函数不降反升；学习速率过低/数据批次过大，一方面损失下降得很慢，另一方面最终结果还很可能不是一个圆，这是因为模型专注于局部最优。

不难发现，只要学习率在0.01附近，采用ReLu激活，只需一层隐藏层即可获得较好的效果（如下图）。

同时注意到，图中第二个神经元权重几乎为0，完全无法对输出造成影响。这减小了模型本身的容量，似乎可能会带来效率不佳，甚至欠拟合的问题？

![image-20250312192508609](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250312192508609.png)

刚刚“正则化”选项并没有体现什么效果；

然而，在引入35的噪声后，如果其他数据保持不变，没有正则化的训练会出现明显的过拟合现象

![image-20250312191712501](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250312191712501.png)

在使用L2正则化模型后，此现象得到极大的缓解。询问AI得知原理是在损失函数中加入惩罚项（权重平方和），以此限制权重大小，来防止噪声的过度干扰。

![image-20250312192034891](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20250312192034891.png)



## 2. 学习总结和收获

<mark>如果发现作业题目相对简单，有否寻找额外的练习题目，如“数算2025spring每日选做”、LeetCode、Codeforces、洛谷等网站上的题目。</mark>

复习了几种基本的数据结构，作业题也都相对简单

不得不提，这个神经网络的互动练习还是非常有趣的。期待更多这样的内容。









