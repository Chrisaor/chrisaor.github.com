---
layout: post
title:  "[Level3] 시저 암호"
date:   2018-01-16 11:30:00 +0900
categories: Algorithm
---

어떤 문장의 각 알파벳을 일정한 거리만큼 밀어서 다른 알파벳으로 바꾸는 암호화 방식을 시저 암호라고 합니다.

A를 3만큼 밀면 D가 되고 z를 1만큼 밀면 a가 됩니다. 공백은 수정하지 않습니다.

보낼 문자열 s와 얼마나 밀지 알려주는 n을 입력받아 암호문을 만드는 caesar 함수를 완성해 보세요.

“a B z”,4를 입력받았다면 “e F d”를 리턴합니다.

```python
def caesar(s, n):
    listUpper = ["A", "B", "C", "D", "E", "F", "G",
     "H", "I", "J", "K", "L", "M", "N", "O",
     "P", "Q", "R", "S", "T", "U", "V", "W", "X", "Y", "Z"]
    listLower = ["a", "b", "c", "d", "e", "f", "g", "h", "i", "j",
    "k", "l", "m", "n", "o", "p", "q", "r", "s", "t", "u", "v"
    , "w", "x", "y", "z"]
    encodedStr = ""
    encodedStr = ""
    for i in s:
        if i == " ":
            encodedStr += i
        elif i == i.upper():
            encodedStr += listUpper[(listUpper.index(i)+n)%26]

        elif i != i.upper():
            encodedStr += listLower[(listLower.index(i)+n)%26]

    return encodedStr

# 실행을 위한 테스트코드입니다.
print('s는 "a B z", n은 4인 경우: ' + caesar("a B z", 4))
```
