---
layout: post
title:  "[Stage 13] Reverse Parentheses"
date:   2018-03-13 13:30:00 +0900
categories: Algorithm
---

You have a string `s` that consists of English letters, punctuation marks, whitespace characters, and brackets. It is guaranteed that the parentheses in `s` form a regular bracket sequence.

Your task is to reverse the strings contained in each pair of matching parentheses, starting from the innermost pair. The results string should not contain any parentheses.

**Example**

For string `s = "a(bc)de"`, the output should be
`reverseParentheses(s) = "acbde"`.

**Input/Output**

- **[execution time limit] 4 seconds (py3)**

- **[input] string s**

A string consisting of English letters, punctuation marks, whitespace characters and brackets. It is guaranteed that parentheses form a _regular bracket sequence._

**Constraints:**    
`5 ≤ s.length ≤ 55.`

**[output] string**

```python
def reverseParentheses(s):\
    def reverse(list1):
        list1 = list1[::-1]
        for i in range(len(list1)):
            if list1[i] == '(':
                list1[i] = ')'
            elif list1[i] == ')':
                list1[i] = '('
        return list1
    s_list = list(s)
    cnt = 0
    start = 0
    end = 0
    i = 0
    while i < len(s_list):
        if s_list[i] == '(':
            pass
            if cnt == 0:
                cnt += 1
                start = i
            else:
                cnt += 1
        elif s_list[i] == ')':
            cnt -= 1
            if cnt == 0:
                end = i
                s_list = s_list[:start]+reverse(s_list[start+1:end])+s_list[end+1:]
        i += 1
    s = ''.join(s_list)
    if '(' in s_list:
        return reverseParentheses(s)
    else:
        return s
```