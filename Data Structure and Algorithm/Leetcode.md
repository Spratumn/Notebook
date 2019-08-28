## 进制
### #504.七进制数
**题目描述：**  
给定一个整数，将其转化为7进制，并以字符串形式输出。  
示例 1:  
输入: 100  
输出: "202"  
注意: 输入范围是 [-1e7, 1e7] 。

**知识点：**  
N进制就是“逢N进1，借1当N。”  
10进制转换为N进制，“除N取余”取倒序

**思路和代码：**
```python3
class Solution:
    def convertToBase7(self, num: int) -> str:
        if num<-1e7 or num>1e7:
            raise AssertionError('Number out of range')
        result = ''
        flag = True if num<0 else False
        num = abs(num)
        while num>=7:
            num,m= divmod(num,7)
            result = result+str(m)
        result =result + str(num)
        if flag:
            result = result+'-'
        return result[::-1]
 ```
 
## 二分法
### #69. x的平方根
**题目描述：**  
实现int sqrt(int x)函数。  
计算并返回x的平方根，其中x 是非负整数。  
由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。  
示例 1:  
输入: 4  
输出: 2  
示例 2:    
输入: 8  
输出: 2  
说明: 8 的平方根是 2.82842...,   
由于返回类型是整数，小数部分将被舍去。

**知识点：**  
二分法：mid=(left+right)/2 

**思路和代码：**  
结果返回的是近似整数，考虑边界值的选择这里需要注意右侧边界(n/2)+1

```python3
class Solution:
    def mySqrt(self, x: int) -> int:
        if x == 0:       # 考虑特殊值
            return 0
        left = 1
        right = x
        while left < right:
            mid = (left + right + 1) >> 1
            # if mid * mid > x:          # 返回的是仅保留整数部分，必然有mid * mid <= x
            #     right = mid - 1
            # else:
            #     left = mid
            if mid * mid <= x:
                left = mid
            else:
                right = mid - 1
        return left
```
## 分治 
### #241.为运算表达式设计优先级
**题目描述：**

给定一个含有数字和运算符的字符串，为表达式添加括号，改变其运算优先级以求出不同的结果。你需要给出所有可能的组合的结果。有效的运算符号包含 +,-以及*。

示例1:   
输入: "2-1-1"  
输出: [0, 2]  
解释:  
((2-1)-1) = 0  
(2-(1-1)) = 2  

**知识点：**  
分治，就是把一个复杂的问题分成两个或更多的相同或相似的子问题，再把子问题分成更小的子问题……直到最后子问题可以简单的直接求解  
分治的特征:  
1. 问题缩小到一定规模容易解决  
2. 分解成的子问题是相同种类的子问题，即该问题具有最优子结构性质  
3. 分解而成的小问题在解决之后要可以合并  
4. 子问题是相互独立的，即子问题之间没有公共的子问题  

**思路和代码：**

```python3
class Solution:
    def diffWaysToCompute(self, input: str) -> List[int]:
        if input.isdigit():
            return [int(input)]  # 必须要转换为列表的形式
        symbol = ['+','-','*']
        result = []
        for i in range(len(input)-1,-1,-1):
            if input[i] in symbol:
                left = input[0:i]
                right = input[i+1:]
                left_result = self.diffWaysToCompute(left)
                right_result = self.diffWaysToCompute(right)
                for left_v in left_result:
                    for right_v in right_result:
                        result.append(self.comput_helper(left_v,right_v,input[i]))
        return result
    
    def comput_helper(self,x,y,sym):
        if sym == '+':
            return x+y
        elif sym == '-':
            return x-y
        else:
            return x*y
```
## 链表
### #160.相交链表
**题目描述：**  
编写一个程序，找到两个单链表相交的起始节点。

**注意**:  
- 如果两个链表没有交点，返回 null.
- 在返回结果后，两个链表仍须保持原有的结构。
- 可假定整个链表结构中没有循环。
- 程序尽量满足 O(n) 时间复杂度，且仅用 O(1) 内存

**知识点：**

**思路和代码：**  
方法一: 暴力法  
对链表A中的每一个结点 a_i，遍历整个链表 B 并检查链表 B 中是否存在结点和 a_i相同。  
复杂度分析  
时间复杂度 : O(mn)  
空间复杂度 : O(1)   
方法二: 哈希表法  
遍历链表 A 并将每个结点的地址/引用存储在哈希表中。  
然后检查链表 B 中的每一个结点 b_i是否在哈希表中。  
若在，则 b_i为相交结点。  
复杂度分析  
时间复杂度 : O(m+n)  
空间复杂度 : O(m) 或 O(n)  
```python3
def internode1(head1,head2):
    hash_dict = dict()
    inter = None
    while head1:
        hash_dict[head1] = head1.val
        head1= head1.next
    while head2:
        if head2 in hash_dict:
            inter = head2.val
            break
        else:
            head2 = head2.next
    return inter
```
方法三：双指针法  
创建两个指针pA和pB，分别初始化为链表A和B的头结点。然后让它们向后逐结点遍历。  
当pA到达链表的尾部时，将它重定位到链表B的头结点;   
类似的，当pB到达链表的尾部时，将它重定位到链表A的头结点。  
若在某一时刻pA和pB相遇，则pA/pB为相交结点。  
若无相交结点，则最终退出时pA和pB均为None

```python3
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def getIntersectionNode(self, headA, headB):
        """
        :type head1, head1: ListNode
        :rtype: ListNode
        """
        if not headA or not headB:
            return None
        a,b = headA,headB
        while a is not b:
            a = a.next if a else headB
            b = b.next if b else headA
        return a
```
## 哈希表
### #1.两数之和
**题目描述：**  
给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。  
你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。  
示例:  
给定 nums = [2, 7, 11, 15], target = 9  
因为 nums[0] + nums[1] = 2 + 7 = 9  
所以返回 [0, 1]

**知识点：**  
哈希->散列函数  
Hash算法，是将一个不定长的输入，通过散列函数变换成一个定长的输出，即散列值。  
使用Hash算法的数据结构叫做哈希表，也叫散列表，主要是为了提高查询的效率。它通过把关键码值映射到表中一个位置来访问记录，以加快查找的速度。

**思路和代码：**

```python3
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        hash_dict = dict()  # 使用字典作为查找对象，字典的in 时间复杂度位O(1),列表的in 时间复杂度位O(n)
        for index, value in enumerate(nums):
            # enumerate()将一个可遍历的数据对象(如列表、元组或字符串)组合为一个索引序列
            sub = target - value  
            if sub in hash_dict:
                return [hash_dict[sub], index]
                # 字典按值获取索引
            else:
                hash_dict[value] = index
                # 插入字典
```

## 字符串
### #3.无重复字符的最长子串
**题目描述：**  
给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。  
示例 1:  
输入: "abcabcbb"  
输出: 3   
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

**知识点：**  
滑动窗口法  
1. 列表作为窗口
2. 队列作为窗口

**思路和代码：**  
**1列表作为窗口**
```python3
def lengthOfLongestSubstring(s):
    if not s:
        return 0
    max_len_str = []  # 滑动窗口字符容器列表
    temp_max_list = []  # 最大窗口长度值记录列表
    temp_max = 0  # 最大窗口长度
    for c in s:
        if c in max_len_str:  # 窗口出现重复字符
            max_len_str.append(c)  # 将字符加入窗口
            temp_max_list.append(temp_max)  # 记录此时窗口长度
            pop_num = max_len_str.index(c) + 1  # 计算需删除的字符个数
            for i in range(pop_num):  # 删除重复字符及其前面的字符
                max_len_str.pop(0)
            temp_max = temp_max - (pop_num-1)  # 更新窗口长度
        else: # 无重复字符
            max_len_str.append(c)  # 将字符加入窗口
            temp_max += 1  # 更新窗口长度
    temp_max_list.append(temp_max)
    return max(temp_max_list)
```
**2队列作为窗口**

```python3
class Node():
    def __init__(self,value,next=None):
        self.val=value
        self.next=next

class myQueue():
    def __init__(self):
        self.head=None
        self.rear=None
        self.len=0
    def enqueue(self,v):
        node = Node(v)
        self.len+=1
        if self.rear:
            self.rear.next=node
            self.rear=node
        else:
            self.rear=node
            self.head=node
    def dequeue(self):
        if self.head:
            self.head=self.head.next
            self.len-=1
        else:
            print('queue is empty')
    def length(self):
        return self.len
    def check(self,v):
        check_node = self.head
        flag = False
        index = 0
        while check_node:
            if check_node.val != v:
                check_node=check_node.next
                index+=1
            else:
                flag=True
                break
        return (flag,index)
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        if not s:
            return 0
        max_len_str = myQueue()  # 滑动窗口字符容器列表
        temp_max_list = []  # 最大窗口长度值记录列表
        temp_max = 0  # 最大窗口长度
        for c in s:
            flag,indx=max_len_str.check(c)
            if flag:  # 窗口出现重复字符
                max_len_str.enqueue(c)  # 将字符加入窗口
                temp_max_list.append(temp_max)  # 记录此时窗口长度
                for i in range(indx+1):  # 删除重复字符及其前面的字符
                    max_len_str.dequeue()
                temp_max = temp_max - indx  # 更新窗口长度
            else: # 无重复字符
                max_len_str.enqueue(c)  # 将字符加入窗口
                temp_max += 1  # 更新窗口长度
        temp_max_list.append(temp_max)
        return max(temp_max_list)
```

### #242.有效的字母异位词
**题目描述：**  
给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。  
示例1:  
输入: s = "anagram", t = "nagaram"
输出: true
示例 2:  
输入: s = "rat", t = "car"  
输出: false  
说明:  
你可以假设字符串只包含小写字母。  
如果输入字符串包含 unicode 字符怎么办？你能否调整你的解法来应对这种情况？

**知识点：**  
1. 排序
2. 哈希

**思路和代码：**  
1. 为了检查t是否是s的重新排列，我们可以计算两个字符串中每个字母的出现次数并进行比较。因为S和T都只包含A-Z的字母，所以一个简单的26位计数器表就足够了。
2. 我们需要两个计数器数表进行比较吗？实际上不是，因为我们可以用一个计数器表计算s字母的频率，用t减少计数器表中的每个字母的计数器，然后检查计数器是否回到零。
3. 使用哈希表而不是固定大小的计数器。想象一下，分配一个大的数组来适应整个 Unicode 字符范围，这个范围可能超过 100万。哈希表是一种更通用的解决方案，可以适应任何字符范围。


```python3
def isAnagram(s,t):
    if len(s)!=len(t):
        return False
    hash_s = dict()
    for c in s:  # 把s放入哈希表中，并记录每个字符出现的次数
        if c in hash_s:
            hash_s[c]+=1
        else:
            hash_s[c]=1
    for c in t:  # 从哈希表中一次扣除出现的字符
        if c in hash_s:
            if hash_s[c]>1:
                hash_s[c]-=1
            else:
                hash_s.pop(c)
    if hash_s:  # 若执行完后哈希表不为空则返回False
         return False
    else:
         return True
```
### #409.最长回文串
**题目描述：**  
给定一个包含大写字母和小写字母的字符串，找到通过这些字母构造成的最长的回文串。  
在构造过程中，请注意区分大小写。比如"Aa"不能当做一个回文字符串。  
注意:  
假设字符串的长度不会超过 1010。  
示例 1:  
输入:"abccccdd"  
输出:7   
解释:  
我们可以构造的最长的回文串是"dccaccd", 它的长度是 7。

**知识点：**


**思路和代码：**
```python3
def longestPalindrome(s):
    hash_dict = dict()  #记录每种字符的频数
    hash_list = []  #用于遍历哈希表中记录的字符
    max_length=0  # 最大长度
    flag=0 # 是否有出现奇次的字符
    for c in s:
        if c in hash_dict:
            hash_dict[c]+=1
        else:
            hash_dict[c]=1
            hash_list.append(c)
    for c in hash_list:
        if hash_dict[c]%2==0:  # 偶数次的直接累加
            max_length+=hash_dict[c]
        else:  #奇数次的累加减一
            max_length+=(hash_dict[c]-1)
            flag = 1
    if flag: # 出现奇数次的，则加一
        return max_length+1
    else:
        return  max_length
```


## 栈和队列
### #232.用栈实现队列
**题目描述：**


**知识点：**


**思路和代码：**


使用栈实现队列的下列操作：  
- push(x) -- 将一个元素放入队列的尾部。
- pop() -- 从队列首部移除元素。
- peek() -- 返回队列首部的元素。
- empty() -- 返回队列是否为空。
示例:
```
MyQueue queue = new MyQueue();

queue.push(1);
queue.push(2);  
queue.peek();  // 返回 1
queue.pop();   // 返回 1
queue.empty(); // 返回 false
```
说明:  
你只能使用标准的栈操作  
也就是只有push to top,peek/pop from top,size, 和is empty操作是合法的。  
你所使用的语言也许不支持栈。你可以使用 list 或者 deque（双端队列）来模拟一个栈，只要是标准的栈操作即可。  
假设所有操作都是有效的 （例如，一个空的队列不会调用 pop 或者 peek 操作）。  

## 双指针
### #167.两数之和Ⅱ-输入有序数组
**题目描述：**


**知识点：**


**思路和代码：**  
给出两个非空的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。  
如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。  
您可以假设除了数字 0 之外，这两个数都不会以 0 开头。  
示例：  
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)  
输出：7 -> 0 -> 8  
原因：342 + 465 = 807  
```python3
class Solution:
    def addTwoNumbers(self, l1, l2):

        n = l1.val + l2.val
        l3 = ListNode(n % 10)
        l3.next = ListNode(n // 10)
        p1 = l1.next
        p2 = l2.next
        p3 = l3
        while True:
            if p1 and p2:
                sum = p1.val + p2.val + p3.next.val
                p3.next.val = sum % 10
                p3.next.next = ListNode(sum // 10)
                p1 = p1.next
                p2 = p2.next
                p3 = p3.next
            elif p1 and not p2:
                sum = p1.val + p3.next.val
                p3.next.val = sum % 10
                p3.next.next = ListNode(sum // 10)
                p1 = p1.next
                p3 = p3.next
            elif not p1 and p2:
                sum = p2.val + p3.next.val
                p3.next.val = sum % 10
                p3.next.next = ListNode(sum // 10)
                p2 = p2.next
                p3 = p3.next
            else:
                if p3.next.val == 0:
                    p3.next = None
                break
        return l3


l1 = ListNode(2)
l1.next = ListNode(3)
l1.next.next = ListNode(8)
l2 = ListNode(9)
l2.next = ListNode(3)
l2.next.next = ListNode(1)
s = Solution()
res = s.addTwoNumbers(l1, l2)
print(res.next.next.val, res.next.val, res.val)

```

## 排序
### #215.数组中的第k个最大元素
**题目描述：**

**知识点：**

**思路和代码：**  
在未排序的数组中找到第 k 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。  
示例 1:  
输入: [3,2,1,5,6,4] 和 k = 2  
输出: 5  
示例2:  
输入: [3,2,3,1,2,4,5,5,6] 和 k = 4  
输出: 4  
说明:  
你可以假设 k 总是有效的，且 1 ≤ k ≤ 数组的长度

### #347.前K个高频元素
**题目描述：**


**知识点：**


**思路和代码：**  
给定一个非空的整数数组，返回其中出现频率前k高的元素。  
示例 1:  
输入: nums = [1,1,1,2,2,3], k = 2  
输出: [1,2]  
示例 2:  
输入: nums = [1], k = 1  
输出: [1]  
说明：  
你可以假设给定的k总是合理的，且 1 ≤ k ≤ 数组中不相同的元素的个数。  
你的算法的时间复杂度必须优于 O(n log n) ,n是数组的大小。  

## 数组和矩阵
### #462.最少移动次数使数组元素相等Ⅱ
**题目描述：**


**知识点：**


**思路和代码：**  
给定一个非空整数数组，找到使所有数组元素相等所需的最小移动数，其中每次移动可将选定的一个元素加1或减1。 您可以假设数组的长度最多为10000。  
例如:  
输入:  
[1,2,3]  
输出:2  
说明：  
只有两个动作是必要的（记得每一步仅可使其中一个元素加1或减1）： 
[1,2,3]  =>  [2,2,3]  =>  [2,2,2]

### #169.求众数
**题目描述：**


**知识点：**


**思路和代码：**  
给定一个大小为 n 的数组，找到其中的众数。众数是指在数组中出现次数大于⌊ n/2 ⌋的元素。  
你可以假设数组是非空的，并且给定的数组总是存在众数。  
示例1:  
输入: [3,2,3]  
输出: 3  
示例2:  
输入: [2,2,1,1,1,2,2]  
输出: 2  

## 位运算
### #260.只出现一次的数Ⅲ
**题目描述：**


**知识点：**


**思路和代码：**  
给定一个整数数组nums，其中恰好有两个元素只出现一次，其余所有元素均出现两次。 找出只出现一次的那两个元素。  
示例 :  
输入: [1,2,1,3,2,5]  
输出: [3,5]  
注意：  
结果输出的顺序并不重要，对于上面的例子，[5, 3]也是正确答案。  
你的算法应该具有线性时间复杂度。你能否仅使用常数空间复杂度来实现？  

## 二叉树 
### #110.平衡二叉树
**题目描述：**  
给定一个二叉树，判断它是否是高度平衡的二叉树。  
本题中，一棵高度平衡二叉树定义为：  
一个二叉树每个节点的左右两个子树的高度差的绝对值不超过1  

**知识点：**


**思路和代码：**

```python3
def tree_depth(root):
    if not root:
        return 0
    else:
        return max(tree_depth(root.left),tree_depth(root.right)) +1

def avl_depth(root):
    if not root:
        return 0
    left_depth = avl_depth(root.left)
    right_depth = avl_depth(root.right)
    if left_depth<0:
        return left_depth
    if right_depth < 0:
        return right_depth
    if abs(left_depth - right_depth) < 2:
        return max(right_depth,left_depth)+1
    else:
        return -1

def is_balanced(root):
    return avl_depth(root)>=0
```

### #144.二叉树的前序遍历
**题目描述：**  
给定一个二叉树，返回它的 前序 遍历。

**知识点：**  
递归法与迭代法

**思路和代码：**

```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        if not root:
            return
        temp_stack = []  # 存储栈
        res_list=[]
        while root or temp_stack:
            while root:
                res_list.append(root.val)  # 入栈时访问val
                temp_stack.append(root)
                root=root.left
            root=temp_stack.pop()
            root=root.right
        return res_list
```

### #102 二叉树的层次遍历
**题目描述：**  
给定一个二叉树，返回其按层次遍历的节点值。 （即逐层地，从左到右访问所有节点）。  
例如:  
给定二叉树: [3,9,20,null,null,15,7],  
返回其层次遍历结果：[[3],[9,20],[15,7]]

**知识点：**


**思路和代码：**

```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Node():
    def __init__(self,data,next=None):
        self.data = data
        self.next=next
class myque():
    def __init__(self):
        self.length=0
        self.left=None
        self.right=None
    def is_empty(self):
        return self.left==None  # 根据左侧节点判断队列是否为空
    def enque(self,value):
        self.length+=1
        nd = Node(value)
        if not self.left:
            self.left=nd
            self.right=nd
        else:
            self.right.next=nd
            self.right=nd
    def deque(self):
        if self.left:  # 根据左侧节点判断队列是否为空
            r = self.left
            self.left=self.left.next
            self.length-=1
            return r.data
        else:
            raise ValueError('empty queue!')
class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        if not root:
            return
        res=[]
        temp_queue=myque()
        temp_queue.enque(root)
        while not temp_queue.is_empty():
            layer_res=[]
            for i in range(temp_queue.length):
                curr=temp_queue.deque()
                layer_res.append(curr.val)
                if curr.left:
                    temp_queue.enque(curr.left)
                if curr.right:
                    temp_queue.enque(curr.right)
            res.append(layer_res)
        return res
```


### #230.二叉搜索树中第k个最小的元素
**题目描述：**  
给定一个二叉搜索树，编写一个函数kthSmallest来查找其中第k个最小的元素。  
说明：  
你可以假设 k 总是有效的，1 ≤ k ≤ 二叉搜索树元素个数。  

**知识点：**


**思路和代码：**




### #208.实现一个前缀树
**题目描述：**


**知识点：**


**思路和代码：**  
实现一个 Trie (前缀树)，包含insert,search, 和startsWith这三个操作。  
示例:

```
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // 返回 true
trie.search("app");     // 返回 false
trie.startsWith("app"); // 返回 true
trie.insert("app");   
trie.search("app");     // 返回 true

```
说明:  
你可以假设所有的输入都是由小写字母a-z构成的。  
保证所有输入均为非空字符串。  

### #513.找树左下角的值
**题目描述：**


**知识点：**


**思路和代码：**  
给定一个二叉树，在树的最后一行找到最左边的值。  
注意: 您可以假设树（即给定的根节点）不为 NULL。


### #17.电话号码的字母组合
**题目描述：**


**知识点：**


**思路和代码：**  
给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。  
给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。  
示例:  
输入："23"  
输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].  
说明:  
尽管上面的答案是按字典序排列的，但是你可以任意选择答案输出的顺序。

### #279.完全平方数（BFS）
**题目描述：**


**知识点：**


**思路和代码：**  
给定正整数n，找到若干个完全平方数（比如1, 4, 9, 16, ...）使得它们的和等于 n。你需要让组成和的完全平方数的个数最少。  
示例1:  
输入: n = 12  
输出: 3   
解释: 12 = 4 + 4 + 4.  
示例 2:  
输入: n = 13  
输出: 2  
解释: 13 = 4 + 9.  

### #695.岛屿的最大面积（DFS）
**题目描述：**


**知识点：**


**思路和代码：**  
给定一个包含了一些 0 和 1的非空二维数组grid, 一个岛屿是由四个方向 (水平或垂直) 的1(代表土地) 构成的组合。你可以假设二维矩阵的四个边缘都被水包围着。  
找到给定的二维数组中最大的岛屿面积。(如果没有岛屿，则返回面积为0。)   
示例 1:  
```
[[0,0,1,0,0,0,0,1,0,0,0,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,1,1,0,1,0,0,0,0,0,0,0,0],
 [0,1,0,0,1,1,0,0,1,0,1,0,0],
 [0,1,0,0,1,1,0,0,1,1,1,0,0],
 [0,0,0,0,0,0,0,0,0,0,1,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,0,0,0,0,0,0,1,1,0,0,0,0]]
```
对于上面这个给定矩阵应返回6。注意答案不应该是11，因为岛屿只能包含水平或垂直的四个方向的‘1’。  
示例 2:  
```
[[0,0,0,0,0,0,0,0]]
```
对于上面这个给定的矩阵, 返回 0。  
注意: 给定的矩阵grid 的长度和宽度都不超过 50。

## 动态规划
### #70.爬楼梯
**题目描述：**


**知识点：**


**思路和代码：**  
假设你正在爬楼梯。需要 n阶你才能到达楼顶。  
每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？  
注意：给定 n 是一个正整数。  
示例 1：  
输入： 2  
输出： 2  
解释： 有两种方法可以爬到楼顶。  
1.  1 阶 + 1 阶  
2.  2 阶  

### #64.最小路径和
**题目描述：**


**知识点：**


**思路和代码：**  
给定一个包含非负整数的 mxn网格，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。  
说明：每次只能向下或者向右移动一步。  
示例:  
输入:  
[
 [1,3,1],
  [1,5,1],
  [4,2,1]
]  
输出: 7  
解释: 因为路径 1→3→1→1→1 的总和最小。  

### #303.区域和检索-数组不可变
**题目描述：**


**知识点：**


**思路和代码：**  
给定一个整数数组 nums，求出数组从索引i到j(i≤j) 范围内元素的总和，包含i, j两点。  
示例：  
给定  
nums = [-2, 0, 3, -5, 2, -1]，求和函数为 sumRange()  
sumRange(0, 2) -> 1  
sumRange(2, 5) -> -1  
sumRange(0, 5) -> -3  

说明:  
你可以假设数组不可变。  
会多次调用sumRange方法。  

### #343.整数拆分
**题目描述：**


**知识点：**


**思路和代码：**  
给定一个正整数n，将其拆分为至少两个正整数的和，并使这些整数的乘积最大化。 返回你可以获得的最大乘积。  
示例 1:  
输入: 2  
输出: 1  
解释: 2 = 1 + 1, 1 × 1 = 1。  
示例2:  
输入: 10  
输出: 36  
解释: 10 = 3 + 3 + 4, 3 ×3 ×4 = 36。  
说明: 你可以假设n不小于 2 且不大于 58。  

### #309.最佳股票买卖时期含冷冻期
**题目描述：**


**知识点：**


**思路和代码：**  
给定一个整数数组，其中第i个元素代表了第i天的股票价格 。  
设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:  
你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。  
卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。  
示例:  
输入: [1,2,3,0,2]  
输出: 3   
解释: 对应的交易状态为: [买入, 卖出, 冷冻期, 买入, 卖出]  


### #416.分割等和子集
**题目描述：**


**知识点：**


**思路和代码：**


给定一个只包含正整数的非空数组。是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

注意:

每个数组中的元素不会超过 100
数组的大小不会超过 200
示例 1:

输入: [1, 5, 11, 5]

输出: true

解释: 数组可以分割成 [1, 5, 5] 和 [11].



### #300.最长上升子序列
**题目描述：**


**知识点：**


**思路和代码：**


给定一个无序的整数数组，找到其中最长上升子序列的长度。

示例:

输入: [10,9,2,5,3,7,101,18]

输出: 4 

解释: 最长的上升子序列是[2,3,7,101]，它的长度是 4。

说明:

可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。

你算法的时间复杂度应该为O(n2) 。

进阶: 你能将算法的时间复杂度降低到O(n log n) 吗?


### #583.两个字符串的删除操作
**题目描述：**


**知识点：**


**思路和代码：**


给定两个单词word1和word2，找到使得word1和word2相同所需的最小步数，每步可以删除任意一个字符串中的一个字符。

示例 1:

输入: "sea", "eat"

输出: 2

解释: 第一步将"sea"变为"ea"，第二步将"eat"变为"ea"

说明:

给定单词的长度不超过500。
给定单词中的字符只含有小写字母。

## 贪心算法
<a href="#目录">首页</a> 
### #455.分发饼干
**题目描述：**


**知识点：**


**思路和代码：**


假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。对每个孩子 i ，都有一个胃口值gi ，这是能让孩子们满足胃口的饼干的最小尺寸；并且每块饼干 j ，都有一个尺寸 sj。如果 sj >= gi，我们可以将这个饼干 j 分配给孩子 i ，这个孩子会得到满足。你的目标是尽可能满足越多数量的孩子，并输出这个最大数值。

注意：

你可以假设胃口值为正。
一个小朋友最多只能拥有一块饼干。

示例1:

输入: [1,2,3], [1,1]

输出: 1

解释: 

你有三个孩子和两块小饼干，3个孩子的胃口值分别是：1,2,3。
虽然你有两块小饼干，由于他们的尺寸都是1，你只能让胃口值是1的孩子满足。
所以你应该输出1。


## 荷兰国旗问题
### #75.颜色分类
**题目描述：**


**知识点：**


**思路和代码：**


给定一个包含红色、白色和蓝色，一共n 个元素的数组，原地对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

此题中，我们使用整数 0、1 和 2 分别表示红色、白色和蓝色。

注意:
不能使用代码库中的排序函数来解决这道题。

示例:

输入: [2,0,2,1,1,0]
输出: [0,0,1,1,2,2]
进阶：

一个直观的解决方案是使用计数排序的两趟扫描算法。
首先，迭代计算出0、1 和 2 元素的个数，然后按照0、1、2的排序，重写当前数组。
你能想出一个仅使用常数空间的一趟扫描算法吗？
 
