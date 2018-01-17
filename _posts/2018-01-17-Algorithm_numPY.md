---
layout: post
title:  "[Level1] 문자열 내 p와 y의 개수"
date:   2018-01-16 11:30:00 +0900
categories: Algorythm
---

numPY함수는 대문자와 소문자가 섞여있는 문자열 s를 매개변수로 입력받습니다.
s에 'p'의 개수와 'y'의 개수를 비교해 같으면 True, 다르면 False를 리턴하도록 함수를 완성하세요. 'p', 'y' 모두 하나도 없는 경우는 항상 True를 리턴합니다.
예를들어 s가 pPoooyY면 True를 리턴하고 Pyy라면 False를 리턴합니다.

```python
def numPY(s):
    pNum = 0
    yNum = 0
    for i in range(len(s)):
        if s[i] == "p" or s[i] =="P":
            pNum += 1
        elif s[i] == "y" or s[i] =="Y":
            yNum += 1
    if pNum == yNum:
        return True
    else:
        return False



# 아래는 테스트로 출력해 보기 위한 코드입니다.
print( numPY("pPoooyY") )
print( numPY("Pyy") )
```
