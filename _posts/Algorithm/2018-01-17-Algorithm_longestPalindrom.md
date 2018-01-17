---
layout: post
title:  "[Level2] 가장 긴 팰린드롬"
date:   2018-01-16 11:30:00 +0900
categories: Algorithm
---

앞뒤를 뒤집어도 똑같은 문자열을 palindrome이라고 합니다.
longest_palindrom함수는 문자열 s를 매개변수로 입력받습니다.
s의 부분문자열중 가장 긴 palindrom의 길이를 리턴하는 함수를 완성하세요.
예를들어 s가 토마토맛토마토이면 7을 리턴하고 토마토맛있어이면 3을 리턴합니다.

```python
def longest_palindrom(s):
    chkS = ""
    pldlist = []
    for i in range(len(s)):
        for j in range(len(s)):
            if s[i] == s[j]:
                chkS = s[i:j+1]
                if chkS == chkS[::-1]:
                    pldlist.append(chkS)
    return len(max(pldlist, key = len))


# 아래는 테스트로 출력해 보기 위한 코드입니다.
print(longest_palindrom("토마토맛토마토"))
print(longest_palindrom("토마토맛있어"))
```
