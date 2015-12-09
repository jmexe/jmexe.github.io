---
layout: post
title:  "Single Number"
date:   2015-10-30 16:07:03
tags: 
- Leetcode
---
#####题目描述
这道题分I, II,题目相似，解题思路基本一样。
#####Single Number
Given an array of integers, every element appears twice except for one. Find that single one.
Note:
Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?
#####Single Number II
Given an array of integers, every element appears three times except for one. Find that single one.

Note:
Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?
#####Single Number III
Given an array of numbers nums, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once.

For example:

Given nums = [1, 2, 1, 3, 2, 5], return [3, 5].

Note:
The order of the result is not important. So in the above example, [5, 3] is also correct.
Your algorithm should run in linear runtime complexity. Could you implement it using only constant space complexity?

#####解题思路

#####Single Number
本题的基本思路是用位运算中的异或解决，对于两个一样的数，他们异或的结果是0，对于0与其他数，异或的结果是其他数不变，那么第一题的解法就有了：
设x的初始值为0，与数组中每个数字异或，最后的结果就是那个single number，为什么会这样呢？看一些解析：
异或预算满足交换律，所以x按顺序与数组中每个数字异或，等同于先把数组中每组相等的两个数字异或后，在把异或的结果与x一一异或，再与single number异或。其结果就是，相等两个数字异或后结果为0，一堆0与0异或后结果还是0，0再与single number异或，结果就是single number。

#####Single Number II
对于single number II来说，就要换一个思路了，因为重复出现的数字会重复3次，还是以位运算的思路来解，异或只适用于消除重复两次的数字，那重复三次怎么办？我们可以把每一个数字每一bit叠加，然后模3。这样的话，运算等同于先把数组中的数字三三聚类，对于重复的数字，每一bit值都是一样的，叠加后每一位1出现3次或者0出现三次，那么每一位的值不是0就是3，模3后都为0，然后一堆0每一位叠加后依然是0，再与single number叠加，那结果就是那个single number！

#####Single Number III
Single Number III的思路与I相似，因为重复的数字会重复两遍，我们考虑用异或，可是所有数字异或的结果为两个single number异或的结果，如何分开这两个数字？其实很简单，我们可以把数字分为两部分，第一部分包含一个single number与一堆重复两次的数字，另一部分包含另一个single number与剩余的重复两次的数字（注意，对于两个重复的数字，他们应该属于同一部分）。然后对两组数字分别进行异或运算，那么结果就是两个single number。可是问题来了，如何把数组分为两部分呢？其实不难，当我们进行完所有数字的异或结果后，得到的数字肯定不为零（为零的话说明没有single number或者single number都为0）。那么就以结果最右一位为1的bit为判断位，因为所有为1的bit说明在该bit上，两个single number一个为0，一个为1.取出这一位，然后再遍历一边数组，这一位为1的数字分为一组，进行异或，剩余的数字分为一组进行异或，结果就是两个single number

###代码

#####Single Number
~~~Java
public class Solution {
    public int[] singleNumber(int[] nums) {
        int ans = 0;
        int[] bits = new int[32];
        
        for (int i = 0; i < 32; i++) {
            for (int n : nums) {
                bits[i] += n >> i & 1;
            }
            
            bits[i] %= 3;
            ans |= bits[i] << i;
        }
        
        return ans;
    }
}
~~~

#####Single Number II
~~~Java
public class Solution {
    public int singleNumber(int[] nums) {
        if (nums == null || nums.length == 0) {
            return -1;
        }
        
        int ans = 0;
        for (int i = 0; i < 32; i++) {
            int cnt = 0;
            for (int n : nums) {
                cnt += n >> i & 1;
            }
            cnt = cnt % 3;
            ans |= cnt << i;
        }
        
        return ans;
    }
}
~~~

#####Single Number III

<!--?prettify lang=java linenums=true?-->

~~~Java
public class Solution {
    public int[] singleNumber(int[] nums) {
        int xor = 0;
        for (int n : nums) {
            xor ^= n;
        }
        
        int lastBit = xor - (xor & (xor - 1));
        
        int[] ans = new int[2];
        for (int n : nums) {
            if ((n & lastBit) == 0) {
                ans[0] ^= n;
            }
            else {
                ans[1] ^= n;
            }
        }
        
        return ans;
    }
}
~~~
