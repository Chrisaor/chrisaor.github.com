---
layout: post
title:  "[Level2] JadenCase 문자열 만들기"
date:   2018-01-16 11:30:00 +0900
categories: Algorithm
---

Jaden_Case함수는 문자열 s을 매개변수로 입력받습니다.
s에 모든 단어의 첫 알파벳이 대문자이고, 그 외의 알파벳은 소문자인 문자열을 리턴하도록 함수를 완성하세요
예를들어 s가 3people unFollowed me for the last week라면 3people Unfollowed Me For The Last Week를 리턴하면 됩니다.

```python
def Jaden_Case(s):
    strList = s.lower().split(" ")
    Jaden = ""
    for i in strList:
        Jaden += i[0].upper() + i[1:]
        if i != strList[-1]:
            Jaden += " "
    return Jaden
 
    
# 아래는 테스트로 출력해 보기 위한 코드입니다.
print(Jaden_Case("3people unFollowed me for the last week"))
```
