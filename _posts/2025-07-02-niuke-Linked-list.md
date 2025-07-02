---
title: niuke Linked list复盘
author: LingMj
data: 2025-07-02
categories: [niuke]
tags: [upload]
description: 难度-Easy
---

## 第一题 BM1 反转链表

>给定一个单链表的头结点pHead(该头节点是有值的，比如在下图，它的val是1)，长度为n，反转该链表后，返回新链表的表头。 数据范围： 要求：空间复杂度 ，时间复杂度 。 如当输入链表{1,2,3}时， 经反转后，原链表变为{3,2,1}，所以对应的输出为{3,2,1}。 以上转换过程如下图所示：示例1 输入 {1,2,3} 输出 {3,2,1} 示例2 输入{} 输出 {}
>

```
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
#
# 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
#
# 
# @param head ListNode类 
# @return ListNode类
#
class Solution:
    def ReverseList(self , head: ListNode) -> ListNode:
        # write code here
        prev = None
        curr = head
        while curr:
            next_node = curr.next
            curr.next = prev
            prev = curr
            curr = next_node
            
        return prev
```

>代码解释：先定义2个变量，一个为prev上一个，一个为链表curr，链表curr存储第一个元素head，循环这个链表从左往右头端开始，当curr没有下一个为空终止循环，所以循环内部定义next_node = curr.next存储下一个节点，然后下一个节点curr.next = prev,此时从开始为空prev，进行赋值，下一次循环即可将值进行互换，prev = 等于当前节点，curr = next_node 下一个节点循环将节点往前移动
>

## 第二题 链表内指定区间反转 

![picture 0](../assets/images/534c38dcf8e4a668e424a111a739be1314a6f626613a0242400850848715670d.png)  

```
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
#
# 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
#
# 
# @param head ListNode类 
# @param m int整型 
# @param n int整型 
# @return ListNode类
#
class Solution:
    def reverseBetween(self , head: ListNode, m: int, n: int) -> ListNode:
      #定义一个新的链表
      dummpy = ListNode(0)
      dummpy.next = head
      prev = dummpy

      for _ in range(1, m):
        prev = prev.next  # 这里步骤是保证我们可以设计前缀节点是开始节点得到上一个

      curr = prev.next  # 这样开始节点就是我们要的区间反转的第一个端值

      for _ in range(n - m):
        next_node = curr.next
        curr.next = next_node.next
        next_node.next = prev.next
        prev.next = next_node
      
      return dummpy.next   
```

>这个区间反转链表的话，先定义一个新的链表用于控制反转的，前缀用循环的方式进行移动，移动到我们开始反转链表的前一个值，需要进行反转链表的次数是区间值，反转链表没有特殊变换，区别在与区间prev.next对应反转整个连curr，区间反转链表next_node.next是反转整个链表的prev
>

## 第三题 链表中的节点每k个一组翻转

![picture 1](../assets/images/c1a4d0d64112b9c2034484ad3c7130702a1fce345ef78b2f97ebda7148f9359c.png)  

```
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
#
# 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
#
# 
# @param head ListNode类 
# @param k int整型 
# @return ListNode类
#
class Solution:
    def reverseKGroup(self , head: ListNode, k: int) -> ListNode:
        temp = head

        for _ in range(k):
            if not temp:
                return head
            temp = temp.next
        
        prev = None
        curr = head

        for _ in range(k):
            next_node = curr.next
            curr.next = prev
            prev = curr
            curr = next_node
        
        head.next = self.reverseKGroup(curr, k)

        return prev
```

>这个的话和整个反转链表差不多，主要是利用递归的方式，先临时定义一个控制区间在k即可递归是下一个head是当前end的下一个
>

## 第四题 合并两个排序的链表 

![picture 2](../assets/images/c73308ba59a3f6492ff033834b72548c2aaa192851b526cc87298e645f0a7817.png)  


```
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
#
# 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
#
# 
# @param pHead1 ListNode类 
# @param pHead2 ListNode类 
# @return ListNode类
#
class Solution:
    def Merge(self , pHead1: ListNode, pHead2: ListNode) -> ListNode:
        # write code here
        if not pHead1:
            return pHead2
        
        if not pHead2:
            return pHead1

        if pHead1.val <= pHead2.val:
            pHead1.next = self.Merge(pHead1.next, pHead2)
            return pHead1
        else:
            pHead2.next = self.Merge(pHead1, pHead2.next)
            return pHead2
```

>这个好像leetcode写过,先判断是否存在空链表，如果存在直接返回另外一个链表的节点，然后是判断链表的下一个节点应该跟那个那个节点进行对比并返回
>

## 第五题 合并k个已排序的链表

>这个明显难度比之前高，我选择2个方案解决一个是最小堆(优先队列)的方法，另一个是分治合并方法
>

![picture 3](../assets/images/84b4e6b58dd1ae2ae0fb057aea08efc6b5bc6effe5c24aad214b7cec7dd5bae9.png)  

```
# 最小堆方案

import heapq


class Solution:
    def mergeKLists(self , lists: List[ListNode]) -> ListNode:
        # write code here
        dummy = ListNode(0)
        curr = dummy
        heap = []

        counter = 0
        for node in lists:
            if node:
                heapq.heappush(heap, (node.val, counter, node))
                counter += 1
        
        while heap:
            val, cnt, node = heapq.heappop(heap)
            curr.next = node
            curr = curr.next
            if node.next:
                heapq.heappush(heap, (node.next.val, counter, node.next))
                counter += 1
        
        return dummy.next
```

>最小堆方案就是定义一个新的链表，从第一个链表开始不断弹出链表的最小节点加入堆直到堆为空用计数器比较节点
>

```
# 分治合并法

# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
#
# 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
#
# 
# @param lists ListNode类一维数组 
# @return ListNode类
#

class Solution:
    def mergeKLists(self , lists: List[ListNode]) -> ListNode:
        temp_lists = [l for l in lists if l]
        if not temp_lists:
            return None

        if len(temp_lists) == 1:
            return temp_lists[0]

        lists = temp_lists

        while len(lists) > 1:
            new_lists = []
            for i in range(0, len(lists), 2):
                if i + 1 < len(lists):
                    merged = self.mergeTwo(lists[i], lists[i+1])
                    new_lists.append(merged)
                else:
                    new_lists.append(lists[i])
            
            lists = new_lists
        return lists[0] if lists else None

    def mergeTwo(self, lists1, lists2):
        if not lists1 and not lists2:
            return None
        if not lists1:
            return lists2
        if not lists2:
            return lists1
        
        dummy = curr = ListNode(0)
        while lists1 and lists2:
            if lists1.val < lists2.val:
                curr.next = lists1
                lists1 = lists1.next
            else:
                curr.next = lists2
                lists2 = lists2.next
            curr = curr.next

        curr.next = lists1 if lists1 else lists2
        return dummy.next 
```

>这个分治合并法原理不难但是写的时候对我很难，首先原理是先定义一个函数进行链表合并这个之前写过所以没有难度部分，主要是控制整个数组分成两组进行合并，剩单个直接加入链表，这里还有定义链表为空或者数组为空的时候，创建一个不断循环的新数组进行操作
>

## 第六题 判断链表中是否有环 

![picture 4](../assets/images/ff3f9771353250b5be59740771527a6edec5def9f756c87606a92c44bc881510.png)  

```
from sys import float_repr_style
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

#
# 
# @param head ListNode类 
# @return bool布尔型
#
class Solution:
    def hasCycle(self , head: ListNode) -> bool:
        if not head or not head.next:
            return False
        
        slow = head
        fast = head

        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next

            if slow == fast:
                return True
            
        return False
```

>这个考虑的是快慢指针节点是否相同相同为true其他为false，慢指针移动1格，快指针移动2格
>

## 第七题  链表中环的入口结点

![picture 5](../assets/images/734d41f8764ad7607a59da9dc747109ebb6bf74990a4b26c4f055c7fc4ebf525.png)  

```
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    def EntryNodeOfLoop(self, pHead):
        # write code here
        if not pHead or not pHead.next:
            return None
        
        fast = slow = pHead

        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next

            if slow == fast:
                break
        else:
            return None

        slow = pHead
        while slow != fast:
            slow = slow.next
            fast = fast.next
        
        return slow
```

>主要是先判断是否为有环，有环情况入口就是快慢指针相遇的位置，所以当第一次相遇时break此时我们得到的节点是为尾节点，慢指针从头开始进行当再次相遇的时候就说入口，此时指针第二次都是移动一格
>

## 第八题 链表中倒数最后k个结点

![picture 6](../assets/images/e93b851e856914058567fd3b4a6f74c86686be6b47a77162e3f1f147fede684e.png)  


```
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
#
# 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
#
# 
# @param pHead ListNode类 
# @param k int整型 
# @return ListNode类
#
class Solution:
    def FindKthToTail(self , pHead: ListNode, k: int) -> ListNode:
        # write code here
        if k <= 0 or not pHead:
            return None
        
        fast = slow = pHead

        for _ in range(k):
            if not fast:
                return None
            fast = fast.next
        
        while fast:
            fast = fast.next
            slow = slow.next

        return slow
```

>这个的话先看看k值和链表是否为空是直接none，还有快指针在移动k格的时候是否为空是也是none，最后当快指针为null时候代表指针已经移动完成，接着输出慢指针链表就是要的答案
>

## 第九题 删除链表的倒数第n个节点

![picture 7](../assets/images/f37b40ed1fc4d8a8c25920ed2cb4395113621dddb544854e51701196ca1d4cc1.png)  


```
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
#
# 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
#
# 
# @param head ListNode类 
# @param n int整型 
# @return ListNode类
#
class Solution:
    def removeNthFromEnd(self , head: ListNode, n: int) -> ListNode:
        # write code here

        dummy = ListNode(0)
        dummy.next = head

        fast = head
        slow = dummy

        for _ in range(n):
            fast = fast.next
        
        while fast:
            fast = fast.next
            slow = slow.next
        slow.next = slow.next.next

        return dummy.next
```

>这个的话就是先定义一个新链表用于输出，慢指针是新链表，然后和找倒是节点一样的方案利用快指针然后使用慢指针的下一个节点接到下下个就等于删除了那个节点
>

## 第10道 两个链表的第一个公共结点

![picture 8](../assets/images/a863d659805cc8cc79e0c80cda30a3fe030d8db54cdbfd4fe8f97888ed367ec8.png)  

```
import re
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

#
# 
# @param pHead1 ListNode类 
# @param pHead2 ListNode类 
# @return ListNode类
#
class Solution:
    def FindFirstCommonNode(self , pHead1 , pHead2 ):
        # write code here
        if not pHead1 or not pHead2:
            return None
        
        p1 = pHead1
        p2 = pHead2

        while p1 != p2:
            p1 = p1.next if p1 else pHead2
            p2 = p2.next if p2 else pHead1

        return p1
```

>这个的话就是设计这两个指针是否相同，在移动的时候判断指针是不是为空是则变成另外一个链表
>

