---
layout: post
title:  "LeetCode 27 Remove Element"
categories: LeetCode
tags: LeetCode Array
author: 氯甲烷
---

* content
{:toc}

LeetCode 27 Remove Element

![](http://www.favicon.cc/logo3d/818131.png)





# 27 Remove Element

## Array.Delete
使用Ruby的话，可以用无脑Array.delete val方法，方法示例如下，返回找到的最后一个值或者nil或者自定义块
```ruby
a = [ "a", "b", "b", "b", "c" ]
a.delete("b")                   #=> "b"
a                               #=> ["a", "c"]
a.delete("z")                   #=> nil
a.delete("z") { "not found" }   #=> "not found"
```

So the simpliest solution:

```ruby
# @param {Integer[]} nums
# @param {Integer} val
# @return {Integer}
def remove_element(nums, val)
    nums.delete val
    nums.size
end
```

## 参考Delete的源码
但是这样速度非常慢，根据lc给出的结果来看呢，还是比某些孩子的方法快...我们可以看下一下Ruby的delete的源码是怎么实现的

```c
VALUE
rb_ary_delete(VALUE ary, VALUE item)
{
    VALUE v = item;
    long i1, i2;

    for (i1 = i2 = 0; i1 < RARRAY_LEN(ary); i1++) {
        VALUE e = RARRAY_AREF(ary, i1);

        if (rb_equal(e, item)) {
            v = e;
            continue;
        }
        if (i1 != i2) {
            rb_ary_store(ary, i2, e);
        }
        i2++;
    }
    if (RARRAY_LEN(ary) == i2) {
        if (rb_block_given_p()) {
            return rb_yield(item);
        }
        return Qnil;
    }

    ary_resize_smaller(ary, i2);

    return v;
}
```
可以看出，返回值类型是VALUE，传入array和要delete的item。

这里让e等于i1所指的变量的值，v等于item的值，如果e就是item，那么让v等于e并continue.
```ruby
rb_ary_store(ary, i2, e);
```
这行相当于`ary[i2] = e` ， 所以这里的意思是当有跳过的元素的时候i2始终是走的慢的"**指针**"，而每次不管是continue或者自然结束for，i1都会自增1，i2只有在不是要删除的item的情况下才会自增。

当出现了`i1 != i2`的情况的时候，我们就把e当前的值复制到i2所指的位置上去，i2所指的位置，始终比i1慢，或者就是要删除的元素的位置。而要复制的e肯定不会是要删除的元素，这样就相当于把item删除了。一直到i1走到末尾时，利用i2已经将除item以外的所有元素复制了一遍。

最后，判断`RARRAY_LEN(ary) == i2`的话，说明一个item都没找到，return nil。否则会利用`ary_resize_smaller(ary,i2)`函数重新设置Array的长度，这里就先不找源码了。

其实Ruby的delete实现思想就和LeetCode给出的解法是一样的，我们先照这个写一段代码试试
```ruby
# @param {Integer[]} nums
# @param {Integer} val
# @return {Integer}
def remove_element(nums, val)
    p2 = 0
    v = val
    for p1 in 0...nums.size
      e = nums[p1]
      
      if e == v then
          next
      end
      
      if p1 != p2 then
          nums[p2] = e
      end
      p2 += 1
  end
  p2
end
```
提交的结果是72ms，还有一个更快地68ms...迷茫，我们再去看看答案

## 处理Rare Value的更efficient的方法
答案给出了一种交换法，其实跟一开始的思想差不多，毕竟题目上让在原Array操作，把要删除的item和末位元素交换，当然交换回来的元素很有可能还是要删除的item，不过下次循环的时候只是把末尾指针前移，直到交换回来的不是那个item，这样不需要把所有元素都移动一遍，只需要处理仅有的item，按需处理。这里再重新写一下这个解法
```ruby
# @param {Integer[]} nums
# @param {Integer} val
# @return {Integer}
def remove_element(nums, val)
  tail_ptr = nums.size - 1
  i = 0
  while i <= tail_ptr do 
    cur_val = nums[i]
    if cur_val == val then
      nums[i] = nums[tail_ptr]
      tail_ptr -= 1
    else
      i += 1
    end
  end
  tail_ptr + 1
end
```
这段代码里值得注意的是，**while的循环条件其实是当两个*指针*相遇的时候**

## 终极疑问
然后submit,喵的比前一个还慢，变成80ms了是什么鬼样子！！

去掉一个取值操作...更慢了，变成了88ms，此时我已经崩溃，猜测是lc的问题，此题记住这几个思想也够了...再看看别人的答案吧

- 这几个解答都没有注意边界条件
- 以后再补充吧