---
title: leetcode题库记录
date: 2022-10-28 17:15:11
tags: [leetcode, 数据结构]
categories:
  - 经验分享
---

## 前言

这篇博客主要是我刷 leetcode 的记录，每刷一道题就记录*做题时间*、_解题方法_、_对应链接_。因为刚开始刷题，没多少经验，可能一开始会使用最简单的方法(暴力解法等)，随着经验丰富后就可以再返回来优化之前做的题，通过记录可以更方便地提升自己，加油！！

### 448.找到所有数组中消失的数字

> [448. 找到所有数组中消失的数字 - 力扣（LeetCode）](https://leetcode.cn/problems/find-all-numbers-disappeared-in-an-array/)

- 2022/10/28 暴力解法

### 283.移动零

> [283. 移动零 - 力扣（LeetCode）](https://leetcode.cn/problems/move-zeroes/)

- 2022/10/28 双指针(交换法 & 覆盖法)

### 121.买卖股票的最佳时机

> [121. 买卖股票的最佳时机 - 力扣（LeetCode）](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)

- 2022/10/28 暴力解法 <font color="red">写出来跟官方的暴力解法一模一样，但是运行会**_超出时间限制_**，应该是官方换过数据集了，但题解没更新</font>
- 2022/10/28 一次遍历（看了官方的解法二思路，再自己敲的）

### 704.二分查找

> [704. 二分查找 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-search/submissions/)

- 2022/10/29 二分法 （左闭右闭，左闭右开）

### 27.移除元素

> [27. 移除元素 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-element/submissions/)

- 2022/10/29 暴力解法
- 2022/10/29 快慢指针

### 977.有序数组的平方

> [977. 有序数组的平方 - 力扣（LeetCode）](https://leetcode.cn/problems/squares-of-a-sorted-array/submissions/)

- 2022/10/29 双指针

### 209. 长度最小的子数组

> [209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)

- 2022/10/30 滑动窗口

### 59.螺旋矩阵 2⃣️

> [59. 螺旋矩阵 II](https://leetcode.cn/problems/spiral-matrix-ii/)

- 2022/10/30 模拟过程

### 203.移除链表元素

> [203. 移除链表元素](https://leetcode.cn/problems/remove-linked-list-elements/)

- 2022/10/30 虚拟头结点

### 707.设计链表

> [707. 设计链表](https://leetcode.cn/problems/design-linked-list/)

- 2022/10/31 手撕

### 19.删除链表的倒数第 N 个结点

> [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

- 2022/11/02 快慢指针

### 24.两两交换链表中的节点

> [24. 两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/)

- 2022/11/02 虚拟头结点+两个临时指针

### 206.反转链表

> [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)

- 2022/11/02 双指针
- 2022/11/02 递归(双指针的基础上做的)

### 160.链表相交

> [160.链表相交](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/)

- 2022/11/02 双链表同一起点后再比较

### 142. 环形链表 II

> [142.环形链表二](https://leetcode.cn/problems/linked-list-cycle-ii/)

- 2022/11/03 快慢指针相遇

### 242. 有效的字母异位词

> [242.有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)

- 2022/11/03 数组作为哈希表

### 349.两个数组的交集

> [349.两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/)

- 2022/11/04 使用 set 解决

### 202.快乐数

> [202.快乐数](https://leetcode.cn/problems/happy-number/)

- 2022/11/04 使用 set 解决

### 1. 两数之和

> [1.两数之和](https://leetcode.cn/problems/two-sum/)

- 2022/11/04 使用 Map 来解决

### 383.赎金信

> [383.赎金信](https://leetcode.cn/problems/ransom-note/)

- 2022/11/05 使用数组当哈希表来解决

### 454.四数相加 2⃣️

> [454.四数相加 2⃣️](https://leetcode.cn/problems/4sum-ii/)

- 2022/11/05 使用 map 当哈希表来解决

### 541.反转字符串 2⃣️

> [541.反转字符串 2⃣️](https://leetcode.cn/problems/reverse-string-ii/description/)

- 2022/11/12 使用双指针解决

### 344.反转字符串

> [344.反转字符串](https://leetcode.cn/problems/reverse-string/)

- 2022/11/12 使用双指针解决

### 剑指 offer 05.替换空格

> [剑指 offer05.替换空格](https://leetcode.cn/problems/ti-huan-kong-ge-lcof/description/)

- 2022/11/12 使用双指针解决

### 151.反转字符串中的单词

> [151.反转字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/)

- 2022/11/18 使用双指针解决

### 剑指 offer 58 - 2⃣️。.左旋转字符串

> [剑指 offer 58 - 2⃣️。.左旋转字符串](https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/description/)

- 2022/11/19 反复旋转字符串，然后使用双指针实现旋转`reverse()`方法解决

### 28.找出字符串中第一个匹配项的下标

> [28.找出字符串中第一个匹配项的下标](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)

- 2022/11/20 kmp 算法

### 459. 重复的子字符串

> [459. 重复的子字符串](https://leetcode.cn/problems/repeated-substring-pattern/description/)

- 2022/11/20 kmp 算法

### 232. 用栈实现队列

> [232. 用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/description/)

- 2022/11/20 用栈实现队列（栈就相当于用数组来实现）

### 225. 用队列实现栈

> [225. 用队列实现栈](https://leetcode.cn/problems/implement-stack-using-queues/description/)

- 2022/11/20 用队列实现栈（用数组的`push()` 和 `shift()`库函数实现）

### 20. 有效的括号

> [20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/description/)

- 2022/11/24 用栈模拟过程实现

### 1047. 删除字符串中的所有相邻重复项

> [1047.删除字符串中的所有相邻重复项](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/description/)

- 2022/11/24 用栈模拟实现过程，跟第 20 题类似的思路
