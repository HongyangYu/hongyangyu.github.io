---
title: Fast Pointer and Slow Pointer
date: 2017-09-04
categories: 
  - Algorithm
tags: 
  - algorithm
  - java
  - programming
---

## 1. Concept
Quora：
> Slow pointer and fast pointer are simply the names given to two pointer variables. The only difference is that, slow pointer travels the linked list one node at a time where as a fast pointer travels the linked list two nodes at a time.

## 2. Application: Detect whether a singly linked list has a loop, If has, where the start of a loop is?

*Step1:* Let a fast pointer and a slow pointer both point to the head. The slow pointer travels the linked list one node at a time while the fast pointer travels the linked list two nodes at a time. If the faster pointer and the slow pointer point to a same node, then the linked list has a loop. Otherwise, fast pointer will point to null,  that is, the linked list does not have a loop. <br>

*Step2:* Let the fast pointer point to head again. In this step, fast point travels the linked list one node at a time. The start of the loop is the node where fast pointer and slow pointer meet. <br>

_Thinking_：How to use fast pointer and slow point find median in an ordered singly linked list.
	
## 3. Prove

### 1) Why fast pointer and slow pointer will meet in a linked list with loop?

Think about the following situations:

Situation 1: If the number of nodes between fast pointer and slow pointer is 1, after one step, the two pointers will meet. <br>
Situation 2: If the number of nodes between fast pointer and slow pointer is 2, after one step, the number of nodes between the two pointers will be 1, that is situation 1. <br>
Situation 3: If the number of nodes between fast pointer and slow pointer is 3, after one step, the number of nodes between the two pointers will be 2, that is situation 2. <br>
... <br>
Situation *N*: If the number of nodes between fast pointer and slow pointer is *N*, after one step, the number of nodes between the two pointers will be *N-1*, that is situation *N-1*. <br>

Then by Mathematical Induction, we can prove that fast pointer and slow pointer will meet in a linked list with loop. <br>


### 2) Why we can find the start of loop via step 2?

Assume the slow pointer move *K* times, and the two pointer meet at point *M*, then the number of nodes between point *M* and the head is *K*. <br>

In step 1, the slow pointer travels *K* nodes to point *M*, while the fast pointer travels *2K* nodes to *M*. So in step 2, the slow pointer will come back to point *M* after *K* steps, meanwhile the fast pointer will arrive at point *M*. That is, the two pointer will meet at point *M* if moving *K* steps in step 2. <br>

Because the two pointers both travels 1 node at a time in step 2, in the loop of the linked list, they will point to same node all the time. Thus, the node where the two pointers meet is the start of loop.


## 4.Practice
 * [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)
 * [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/description/)

## Reference
1. Leetcode: [https://leetcode.com](https://leetcode.com)
2. Quora: [What is a slow pointer and a fast pointer in a linked list](https://www.quora.com/What-is-a-slow-pointer-and-a-fast-pointer-in-a-linked-list)