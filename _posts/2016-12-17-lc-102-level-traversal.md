---
layout: post
title:  "LeetCode 102 Binary Tree Level Order Traversal"
categories: LeetCode
tags: LeetCode Array Traverse Tree
author: 氯甲烷
---

* content
{:toc}

LeetCode 102 Binary Tree Level Order Traversal.

![](http://www.favicon.cc/logo3d/818131.png)




# 102 Binary Tree Level Order Traversal

## 1. Frist Draft Code

```ruby
def level_order(root)
  sign = TreeNode.new 0
  q = Array.new
  q.push root
  q.push sign
  result = []
  result.push [root] unless root.nil?
  tmp_list = []
  while q.size != 0
    cur_node = q.shift
    if cur_node != sign
      unless cur_node.left.nil?
        tmp_list.push cur_node.left 
        q.push cur_node.left
      end
      unless cur_node.right.nil?
        tmp_list.push cur_node.right
        q.push cur_node.right
      end
    else
      result.push tmp_list unless tmp_list.size == 0
      tmp_list = Array.new
      return result if q.size == 0
      q.push sign
    end
  end
end
```

挺奇怪的，在RubyMine中运行结果正确...LeetCode就不行了？

## 2. First AC Code

MDZZ,原来是因为返回结果要的类型是value值，我把TreeNode传进去了，被浪费的生命啊，下面是使用Magic Node的解法，**这种解法遇到每层只有一个的节点的情况开销很大，我们看下leetcode和discuss的方法吧...有什么其他的层次遍历方法呢**
```ruby
def level_order(root)
  return [] if root.nil?
  sign = TreeNode.new 0
  q = Array.new
  q.push root
  q.push sign
  ans = []
  ans.push [root.val,]
  tmp_list = []
  while q.size != 0
    cur_node = q.shift
    if cur_node != sign
      unless cur_node.left.nil?
        tmp_list.push cur_node.left.val 
        q.push cur_node.left
      end
      unless cur_node.right.nil?
        tmp_list.push cur_node.right.val
        q.push cur_node.right
      end
    else
      ans.push tmp_list unless tmp_list.size == 0
      tmp_list = Array.new
      return ans if q.size == 0
      q.push sign
    end
  end
end
```

## 3. 其他层次遍历方法
