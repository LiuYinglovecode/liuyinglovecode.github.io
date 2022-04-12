---
title: 20ValidParentheses
date: 2020-9-10 14:35:28
tags:
- leetcode
- easy
categories:
- leetcode
---  
Here is the [Original Post](https://leetcode-cn.com/problems/valid-parentheses/solution/valid-parentheses-fu-zhu-zhan-fa-by-jin407891080/)

### Principle
- stack: first in & last out 如果左括号入栈，遇到右括号时将对应栈顶左括号出栈，则遍历完所有括号后`stack`仍然为空。
  
- establish hashtable `dic` construct corresponding relation, `key`-left brackets, `value`-right brackets, so it's just *O*(1) time complexity to check if two brackests are in one-to-one correspondence. Here we build a `stack` to iterate stirng `s`, and judge it by algorithm.

### Method

``` flow
st=>start: c
cond1=>condition: s.length() > 0
cond=>condition: Is c left bracket?
op=>operation: stack.pop() not corresponding with c
op1=>operation: return false
push=>operation: push

e=>end
st->cond1->cond
cond1(no)->op1
cond1(yes)->cond
cond(yes)->push
cond(no)->op->op1
```
