---
layout: post
title:  "LeetCode 111 Minimum Depth of Binary Tree"
categories: LeetCode
tags: LeetCode Array Traverse Tree
author: 氯甲烷
---

* content
{:toc}

Given a binary tree, find its minimum depth.

![](http://www.favicon.cc/logo3d/818131.png)




# 111 Minimum Depth of Binary Tree

```
Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.
```

My first draft:
```ruby
# Definition for a binary tree node.
# class TreeNode
#     attr_accessor :val, :left, :right
#     def initialize(val)
#         @val = val
#         @left, @right = nil, nil
#     end
# end

# @param {TreeNode} root
# @return {Integer}
def min_depth(root)
  if root.nil? then
    return 0
  end
  if root.left.nil? && root.right.nil? then
    return 0
  end
  left_depth = nil
  right_depth = nil
  unless root.left.nil? then
    left_depth = min_depth(root.left)
  end
  unless root.right.nil? then
    right_depth = min_depth(root.right)
  end
  if left_depth == nil then
    return right_depth + 1
  elsif right_depth == nil then
    return left_depth + 1
  else
    return left_depth > right_depth ? (left_depth + 1) : (right_depth + 1)
  end
end
```

## Ruby how to find the min of two value

Just use Array: `[a,b].min`, that's increditable

cited from [forum](http://archive.railsforum.com/viewtopic.php?id=37532)

## Second draft 

cited from [blog](http://blog.csdn.net/linhuanmars/article/details/19660209)

```ruby
# Definition for a binary tree node.
# class TreeNode
#     attr_accessor :val, :left, :right
#     def initialize(val)
#         @val = val
#         @left, @right = nil, nil
#     end
# end

# @param {TreeNode} root
# @return {Integer}
def min_depth(root)
  if root.nil? then
    return 0
  end
  if root.left.nil? then
    return 1 + min_depth(root.right)
  end
  if root.right.nil? then
    return 1 + min_depth(root.left)
  end

  return [min_depth(root.left),min_depth(root.right)].min + 1
end
```

## Other methods: non-recurive
It was too slow, so there must be other alters for this problem or BFS

```ruby
# Definition for a binary tree node.
# class TreeNode
#     attr_accessor :val, :left, :right
#     def initialize(val)
#         @val = val
#         @left, @right = nil, nil
#     end
# end

# @param {TreeNode} root
# @return {Integer}
def min_depth(root)
  dpt = 0
  q = Array.new
  q.push root
  while q.size!=0 do
    node = q.shift
    q.push node.left unless node.left.nil?
    q.push node.right unless node.right.nil?
    
end
```

That's terriable, it is just a method to level order traverse. But I cannot calculate the level num.


## Methods from discuss

### Method 1:level-order traversal
Ooooooooooooooh! Level-order traversal, I am foolish. Just add a tag in the quene, we can discern the sign of entering next level.

#### Way 1: use a magic node as a sign

The answer cited from [this post](https://discuss.leetcode.com/topic/1948/my-solution-used-level-order-traversal).

I translate the code into Ruby code, as following:
```ruby
def min_depth(root)
  return 0 if root.nil?
  dpt = 1
  sign_node = TreeNode.new -999
  q = Array.new
  q.push root
  q.push sign_node # used for determine if we enter next level
  while q.size != 0 do
    node = q.shift
    if node != sign_node
      q.push node.left unless node.left.nil?
      q.push node.right unless node.right.nil?
      if node.left.nil? && node.right.nil?
        return dpt
      end
    else
      if q.size != 0
        dpt += 1
        q.push sign_node
      end
    end
  end

end
```

Runtime 100ms...Are u kidding me...But it's ruby

*** 注意返回条件，并不是取每行最后的node返回，而是用第一个没有儿子的node *** 

`This refers to our methods of solving problem, the thought`

#### Way 2 : use two quene instead of magical node

i'm tired in this problem, so I decide to write down right now

##### **First, java style**
```ruby
def min_depth(root)
  return 0 if root.nil?
  dpt = 1
  sign_node = TreeNode.new -999
  q = Array.new
  dq = Array.new
  q.push root
  dq.push 1
  while q.size != 0 do
    node = q.shift
    cur_dpt = dq.shift
    
    if node.left.nil? && node.right.nil?
      return cur_dpt
    end
    unless node.left.nil?
      q.push node.left
      dq.push cur_dpt+1
    end
    unless node.right.nil?
      q.push node.right
      dq.push cur_dpt+1
    end
  end

end
```
This method runtime 94ms...

##### **Second, ruby style**\(like Python Turple\)
```ruby
def min_depth(root)
  return 0 if root.nil?
  q = Array.new
  q.push [root,1]
  while q.size != 0 do
    e = q.shift
    node = e[0]
    cur_dpt = e[1]
    
    if node.left.nil? && node.right.nil?
      return cur_dpt
    end
    q.push [node.left,cur_dpt+1] unless node.left.nil?
    q.push [node.right,cur_dpt+1] unless node.right.nil?
  end
end
```

### Mehtod 2


## Methods from LeetCode
