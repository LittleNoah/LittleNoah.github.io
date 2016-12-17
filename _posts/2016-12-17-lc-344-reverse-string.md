---
layout: post
title:  "LeetCode 344 Reverse String"
categories: LeetCode
tags: LeetCode Array String
author: 氯甲烷
---

* content
{:toc}

LeetCode 344 Reverse String

![](http://www.favicon.cc/logo3d/818131.png)






# 344 Reverse String

## 1.Simple Mind

```ruby
def reverse_string(s)
  len = s.size
  ed_index = (len)/2
  for n in 0...ed_index do
    s[n], s[len-n-1] = s[len-n-1], s[n]
  end
  s
end
```

Use length reverse the string, but **Time Limit Exceed**!

## 2. Use String.reverse

`s.reverse`, got Accepted, 84ms...wtf

So we need to look into the source code...

```c
rb_str_reverse(VALUE str)
{
    rb_encoding *enc;
    VALUE rev;
    char *s, *e, *p;
    int cr;

    if (RSTRING_LEN(str) <= 1) return rb_str_dup(str);
    enc = STR_ENC_GET(str);
    rev = rb_str_new_with_class(str, 0, RSTRING_LEN(str));
    s = RSTRING_PTR(str); e = RSTRING_END(str);
    p = RSTRING_END(rev);
    cr = ENC_CODERANGE(str);

    if (RSTRING_LEN(str) > 1) {
        if (single_byte_optimizable(str)) {
            while (s < e) {
                *--p = *s++;
            }
        }
        else if (cr == ENC_CODERANGE_VALID) {
            while (s < e) {
                int clen = rb_enc_fast_mbclen(s, e, enc);

                p -= clen;
                memcpy(p, s, clen);
                s += clen;
            }
        }
        else {
            cr = rb_enc_asciicompat(enc) ?
                ENC_CODERANGE_7BIT : ENC_CODERANGE_VALID;
            while (s < e) {
                int clen = rb_enc_mbclen(s, e, enc);

                if (clen > 1 || (*s & 0x80)) cr = ENC_CODERANGE_UNKNOWN;
                p -= clen;
                memcpy(p, s, clen);
                s += clen;
            }
        }
    }
    STR_SET_LEN(rev, RSTRING_LEN(str));
    OBJ_INFECT_RAW(rev, str);
    str_enc_copy(rev, str);
    ENC_CODERANGE_SET(rev, cr);

    return rev;
}
```

(Smile.jpg), wo kan bu dong

## 3. why method 1 doesn't wordk

## 4. Other ways
