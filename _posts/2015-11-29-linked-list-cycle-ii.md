---
layout: post
title:  "Linked List Cycle II"
date:   2015-11-29 16:07:03
tags: 
- Leetcode
---
#####题目描述
Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

Note: Do not modify the linked list.

Follow up:
Can you solve it without using extra space?
<!--more-->

#####思路
直接上干货吧，two pointer，从head开始，慢指针每次向前走一步，快指针每次向前走两步，如果任何一个指针碰到null，则无环，如果有环，两个指针一定会相遇。
如图所示(图会补上的):

a是进入环之前的长度，(b + c)是一个完整的环的长度，b是指针相遇点与环开始点的距离。
快指针走了 a + b + (b + c)。
慢指针走了 a + b。
而快指针走的步数是慢指针的两倍, 可以得出 2*(a + b) = a + b + (b + c) => a = c

那么，当两个指针相遇后，一个指针从head开始走，另一个指针从相遇点开始走，走a步后会相遇，而且相遇点就是环的开始点，问题解决啦！

#####代码
<!--?prettify lang=java linenums=true?-->

~~~java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        
        
        while (fast != null && slow != null) {
            slow = slow.next;
            fast = fast.next;
            
            if (slow == null || fast == null) {
                return null;
            }
            
            fast = fast.next;
            
            if (slow == fast) {
                break;
            }
        }
        
        if (fast == null) {
            return null;
        } 
        
        slow = head;
        while (slow != fast) {
            slow = slow.next;
            fast = fast.next;
        }
        
        return slow;
    }
}
~~~
