---
layout: post
title:  "[TestDome] Palindrome"
date:   2018-01-24 14:05:00 +0900
categories: Algorithm
---

A palindrome is a word that reads the same backward or forward.

Write a function that checks if a given word is a palindrome. Character case should be ignored.

For example, is_palindrome("Deleveled") should return True as character case should be ignored, resulting in "deleveled", which is a palindrome since it reads the same backward and forward.

**_Difficulty : Easy_**  
**_Time : 10min_**

```python
class Palindrome:

    @staticmethod
    def is_palindrome(word):
        l_word = word.lower()
        if len(word)%2 ==0:
            if l_word[:len(word)//2] == l_word[:len(word)//2-1:-1]:
                return True
            else:

                return False
        else:
            if l_word[:len(word)//2] == l_word[:len(word)//2:-1]:

                return True
            else:
                return False


print(Palindrome.is_palindrome('Delseled'))
```