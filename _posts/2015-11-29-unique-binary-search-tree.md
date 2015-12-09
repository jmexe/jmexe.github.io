---
layout: post
title:  "Unique Binary Search Tree II"
date:   2015-11-29 16:07:03
tags: 
- Leetcode
---

#####题目描述
Given n, generate all structurally unique BST's (binary search trees) that store values 1...n.

For example,
Given n = 3, your program should return all 5 unique BST's shown below.

> 1         3     3      2      1
>   \       /     /      / \      \
>   3     2     1      1   3      2
>  /     /       \                 \
> 2     1         2                 3

<!--more-->
#####解题思路
这个题可以递归的解，假设我们有一个数组，存放 1~n这个N个数字，找到所有的Binary Search Tree就是找到分别以1~n为root节点的所有的BST，假设我们要找以m为节点的BST，首先root是m，左子树的合集是数字1~m-1 所能构成所有BST，右字数的合集是数字 m + 1 ~ n所能构成的所有BST的集合。这样就把这个问题化为了了两个子问题啦，可以递归的调用了。注意边界条件就好了。

#####代码
~~~Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public List<TreeNode> generateTrees(int n) {
        int[] nums = new int[n];
        for (int i = 0; i < n; i++) {
            nums[i] = i + 1;
        }
        
        return helper(nums, 0, n - 1);
        
    }
    
    public List<TreeNode> helper(int[] nums, int start, int end) {
        List<TreeNode> ans = new ArrayList<TreeNode>();
        if (end < start) {
            ans.add(null);
            return ans;
        }
        
        for (int i = start; i <= end; i++) {
            List<TreeNode> lSet = helper(nums, start, i - 1);
            List<TreeNode> rSet = helper(nums, i + 1, end);
            
            for (TreeNode lTree : lSet) {
                for (TreeNode rTree : rSet) {
                    TreeNode root = new TreeNode(nums[i]);
                    root.left = lTree;
                    root.right = rTree;
                    ans.add(root);
                }
            }
            
        }
        
        return ans;
    }
    
}
~~~