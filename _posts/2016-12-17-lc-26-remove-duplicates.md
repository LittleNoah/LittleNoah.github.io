---
layout: post
title:  "LeetCode 26 Remove Duplicates from Sorted Array"
categories: LeetCode
tags: LeetCode Array
author: 氯甲烷
---

* content
{:toc}

LeetCode 26 Remove Duplicates from Sorted Array

![](http://www.favicon.cc/logo3d/818131.png)





# 26 Remove Duplicates from Sorted Array

## 重复思想解法
这道题是*#27 Remove Elements*的姊妹题

```ruby
# @param {Integer[]} nums
# @return {Integer}
def remove_duplicates(nums)
  return 0 if nums.size == 0
  cur = 0
  i = 0
  val = nums[i]
  while cur < nums.size do 
    cur_val = nums[cur]
    if cur_val == val then
      cur += 1
      next
    else
      val = cur_val
      i += 1
      nums[i] = cur_val
    end
  end
  return (i+1)
end
```
这个解法的思想是和*Remove Elments*差不多，只是这里面的delete item在变，而且也不是交换末尾的元素而是前方不重复元素

和#27一样的最快解法，用了127ms...这有点慢啊，看看有没有别的解法

## leetCode参考答案
```java
public int removeDuplicates(int[] nums) {
    if (nums.length == 0) return 0;
    int i = 0;
    for (int j = 1; j < nums.length; j++) {
        if (nums[j] != nums[i]) {
            i++;
            nums[i] = nums[j];
        }
    }
    return i + 1;
}

```

## Final
似乎没啥区别...主要是Ruby太慢了，代码结构以后再改